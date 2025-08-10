# Step3 Model Deployment Guide

This document provides deployment guidance for Step3 model.

Currently, our open-source deployment guide only includes TP and DP+TP deployment methods. The AFD (Attn-FFN Disaggregated) approach mentioned in our [paper](https://arxiv.org/abs/2507.19427) is still under joint development with the open-source community to achieve optimal performance. Please stay tuned for updates on our open-source progress.

## Overview

Step3 is a 321B-parameter VLM with hardware-aware model-system co-design optimized for minimizing decoding costs. 

For out fp8 version, about 326G memory is required.
The smallest deployment unit for this version is 8xH20 with either Tensor Parallel (TP) or Data Parallel + Tensor Parallel (DP+TP).

For out bf16 version, about 642G memory is required.
The smallest deployment unit for this version is 16xH20 with either Tensor Parallel (TP) or Data Parallel + Tensor Parallel (DP+TP).

## Deployment Options

### vLLM Deployment

Please make sure to use nightly version of vllm after this [PR](https://github.com/vllm-project/vllm/pull/21998) is merged. For details, please refer to [vllm nightly installation doc](https://docs.vllm.ai/en/latest/getting_started/installation/gpu.html#pre-built-wheels).
```bash
uv pip install -U vllm \
    --torch-backend=auto \
    --extra-index-url https://wheels.vllm.ai/nightly
```

We recommend to use the following command to deploy the model:

**`max_num_batched_tokens` should be larger than 4096. If not set, the default value is 8192.**

To deploy the model for text-only inference, thereby offloading vision modules to increase available GPU memory for KV-cache allocation, append `--limit-mm-per-prompt '{"image": 0}'` to the commands below.

#### BF16 Model
##### Tensor Parallelism(Serving on 16xH20):

```bash
# start ray on node 0 and node 1

# node 0:
vllm serve /path/to/step3 \
    --tensor-parallel-size 16 \
    --reasoning-parser step3 \
    --enable-auto-tool-choice \
    --tool-call-parser step3 \
    --trust-remote-code \
    --max-num-batched-tokens 4096 \
    --port $PORT_SERVING
```

###### Data Parallelism + Tensor Parallelism(Serving on 16xH20):
Step3 only has single kv head, so attention data parallelism can be adopted to reduce the kv cache memory usage.

```bash
# start ray on node 0 and node 1

# node 0:
vllm serve /path/to/step3 \
    --data-parallel-size 16 \
    --tensor-parallel-size 1 \
    --reasoning-parser step3 \
    --enable-auto-tool-choice \
    --tool-call-parser step3 \
    --max-num-batched-tokens 4096 \
    --trust-remote-code \
```

#### FP8 Model
##### Tensor Parallelism(Serving on 8xH20):

```bash
vllm serve /path/to/step3-fp8 \
    --tensor-parallel-size 8 \
    --reasoning-parser step3 \
    --enable-auto-tool-choice \
    --tool-call-parser step3 \
    --gpu-memory-utilization 0.85 \
    --max-num-batched-tokens 4096 \
    --trust-remote-code \
```

###### Data Parallelism + Tensor Parallelism(Serving on 8xH20):

```bash
vllm serve /path/to/step3-fp8 \
    --data-parallel-size 8 \
    --tensor-parallel-size 1 \
    --reasoning-parser step3 \
    --enable-auto-tool-choice \
    --tool-call-parser step3 \
    --max-num-batched-tokens 4096 \
    --trust-remote-code \
```


##### Key parameter notes:

* `reasoning-parser`: If enabled, reasoning content in the response will be parsed into a structured format.
* `tool-call-parser`: If enabled, tool call content in the response will be parsed into a structured format.

### SGLang Deployment

0.4.10 or later is needed for SGLang.

```
pip3 install "sglang[all]>=0.4.10"
```

#### BF16 Model
##### Tensor Parallelism(Serving on 16xH20):

```bash
# node 1
python -m sglang.launch_server \
 --model-path stepfun-ai/step3 \
 --dist-init-addr master_ip:5000 \
 --trust-remote-code \
 --tool-call-parser step3 \
 --reasoning-parser step3 \
 --tp 16 \
 --nnodes 2 \
 --node-rank 0

# node 2
python -m sglang.launch_server \
 --model-path stepfun-ai/step3 \
 --dist-init-addr master_ip:5000 \
 --trust-remote-code \
 --tool-call-parser step3 \
 --reasoning-parser step3 \
 --tp 16 \
 --nnodes 2 \
 --node-rank 1
```

#### FP8 Model
##### Tensor Parallelism(Serving on 8xH20):

```bash
python -m sglang.launch_server \
    --model-path /path/to/step3-fp8 \
    --trust-remote-code \
    --tool-call-parser step3 \
    --reasoning-parser step3 \
    --tp 8
```


### TensorRT-LLM Deployment

[Coming soon...]


## Client Request Examples

Then you can use the chat API as below:
```python
from openai import OpenAI

# Set OpenAI's API key and API base to use vLLM's API server.
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

chat_response = client.chat.completions.create(
    model="step3",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://xxxxx.png"
                    },
                },
                {"type": "text", "text": "Please describe the image."},
            ],
        },
    ],
)
print("Chat response:", chat_response)
```
You can also upload base64-encoded local images:

```python
import base64
from openai import OpenAI
# Set OpenAI's API key and API base to use vLLM's API server.
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"
client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)
image_path = "/path/to/local/image.png"
with open(image_path, "rb") as f:
    encoded_image = base64.b64encode(f.read())
encoded_image_text = encoded_image.decode("utf-8")
base64_step = f"data:image;base64,{encoded_image_text}"
chat_response = client.chat.completions.create(
    model="step3",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": base64_step
                    },
                },
                {"type": "text", "text": "Please describe the image."},
            ],
        },
    ],
)
print("Chat response:", chat_response)

```

Note: In our image preprocessing pipeline, we implement a multi-patch mechanism to handle large images. If the input image exceeds 728x728 pixels, the system will automatically apply image cropping logic to get patches of the image.