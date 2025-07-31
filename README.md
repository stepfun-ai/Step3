<div align="center">
  <picture>
      <img src="figures/stepfun-logo.png" width="30%" alt="StepFun: Cost-Effective Multimodal Intelligence">
  </picture>
</div>

<hr>

<div align="center" style="line-height:1">
  <a href="https://stepfun.com/" target="_blank"><img alt="Chat" src="https://img.shields.io/badge/Chat-StepFun-ff6b6b?color=1783ff&logoColor=white"/></a>
  <a href="https://stepfun.com/" target="_blank"><img alt="Homepage" src="https://img.shields.io/badge/Homepage-StepFun-white?logo=StepFun&logoColor=white"/></a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://huggingface.co/collections/stepfun-ai/step3-688a3d652dbb45d868f9d42d" target="_blank"><img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-StepFun-ffc107?color=ffc107&logoColor=white"/></a>
  <a href="https://www.modelscope.cn/models/stepfun-ai/step3" target="_blank"><img alt="ModelScope" src="https://img.shields.io/badge/ModelScope-StepFun-white?logo=modelscope&logoColor=white"/></a>
  <a href="https://x.com/StepFun_ai" target="_blank"><img alt="Twitter Follow" src="https://img.shields.io/badge/Twitter-StepFun-white?logo=x&logoColor=white"/></a>
</div>

<div align="center" style="line-height: 1;">
<a href="https://discord.com/invite/XHheP5Fn" target="_blank"><img alt="Discord" src="https://img.shields.io/badge/Discord-StepFun-white?logo=discord&logoColor=white"/></a>
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/License-Apache%202.0-blue?&color=blue"/></a>
</div>

<div align="center">
<b>📰&nbsp;&nbsp;<a href="https://stepfun.ai/research/step3">Step3 Model Blog</a></b> &nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp; <b>📄&nbsp;&nbsp;<a href="https://arxiv.org/abs/2507.19427">Step3 System Tech Report</a></b>
</div>

## Introduction

Step3 is our cutting-edge multimodal reasoning model—built on a Mixture-of-Experts architecture with 321B total parameters and 38B active. 
It is designed end-to-end to minimize decoding costs while delivering top-tier performance in vision–language reasoning. 
Through the co-design of Multi-Matrix Factorization Attention (MFA) and Attention-FFN Disaggregation (AFD), 
Step3 maintains exceptional efficiency across both flagship and low-end accelerators.

### Step3 model card:

|          Config        |  Value  |
|------------------------|---------|
| **Number of Layers (Dense layer included)**|61|
|**Number of Dense Layers**| 5|
| **Hidden Dimension**       | 7168    |
| **Attention Mechanism**    | MFA     |
| **Low-rank Query Dimension** | 2048  |
| **Number of Query Heads**          | 64      |
| **Head Dimension**        | 256     |
|**Number of Experts** |48|
|**Selected Experts per Token**|3|
|**Number of Shared Experts**| 1|
| **Max Context Length** | 65536 |
| **Tokenizer** | Deepseek V3 |
| **Total Parameters (LLM)** | 316B |
| **Activated Params per Token** | 38B |
| **Total Parameters (VLM)** | 321B |


## Evaluation Results
<table>
  <thead>
    <tr>
      <th></th>
      <th>Model</th>
      <th>Total Params.</th>
      <th>MMMU</th>
      <th>MathVision</th>
      <th>ZeroBench(sub)</th>
      <th>DYNAMATH</th>
      <th>SimpleVQA</th>
      <th>HallusionBench</th>
      <th>AIME25</th>
      <th>HMMT25</th>
      <th>CNMO24</th>
      <th>GPQA-Diamond</th>
      <th>LiveCodeBench<br>(24.8-25.5)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="6">Open-Source VLM</td>
      <td>Step3</td>
      <td>321B</td>
      <td>74.2</td>
      <td>64.8</td>
      <td>23.0</td>
      <td>50.1</td>
      <td>62.2</td>
      <td>64.2</td>
      <td>82.9</td>
      <td>70.0</td>
      <td>83.7</td>
      <td>73.0</td>
      <td>67.1</td>
    </tr>
    <tr>
      <td>ERINE4.5 - thinking</td>
      <td>300B/424B</td>
      <td>70.0</td>
      <td>47.6</td>
      <td>22.5</td>
      <td>46.9</td>
      <td>59.8</td>
      <td>60.0</td>
      <td>35.1</td>
      <td>40.5*</td>
      <td>75.5</td>
      <td>76.8</td>
      <td>38.8</td>
    </tr>
    <tr>
      <td>GLM-4.1V-thinking</td>
      <td>9B</td>
      <td>68.0</td>
      <td>49.4</td>
      <td>22.8</td>
      <td>41.9</td>
      <td>48.1</td>
      <td>60.8</td>
      <td>13.3</td>
      <td>6.7</td>
      <td>25.0</td>
      <td>47.4</td>
      <td>24.2</td>
    </tr>
    <tr>
      <td>MiMo-VL</td>
      <td>7B</td>
      <td>66.7</td>
      <td>60.4</td>
      <td>18.6</td>
      <td>45.9</td>
      <td>48.5</td>
      <td>59.6</td>
      <td>60.0</td>
      <td>34.6</td>
      <td>69.9</td>
      <td>55.5</td>
      <td>50.1</td>
    </tr>
    <tr>
      <td>QvQ-72B-Preview</td>
      <td>72B</td>
      <td>70.3</td>
      <td>35.9</td>
      <td>15.9</td>
      <td>30.7</td>
      <td>40.3</td>
      <td>50.8</td>
      <td>22.7</td>
      <td>49.5</td>
      <td>47.3</td>
      <td>10.9</td>
      <td>24.1</td>
    </tr>
    <tr>
      <td>LLaMA-Maverick</td>
      <td>400B</td>
      <td>73.4</td>
      <td>47.2</td>
      <td>22.8</td>
      <td>47.1</td>
      <td>45.4</td>
      <td>57.1</td>
      <td>19.2</td>
      <td>8.91</td>
      <td>41.6</td>
      <td>69.8</td>
      <td>33.9</td>
    </tr>
    <tr>
      <td rowspan="4">Open-Source LLM</td>
      <td>MiniMax-M1-80k</td>
      <td>456B</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>76.9</td>
      <td>-</td>
      <td>-</td>
      <td>70.0</td>
      <td>65.0</td>
    </tr>
    <tr>
      <td>Qwen3-235B-A22B-Thinking</td>
      <td>235B</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>81.5</td>
      <td>62.5</td>
      <td>-</td>
      <td>71.1</td>
      <td>65.9</td>
    </tr>
    <tr>
      <td>DeepSeek R1-0528</td>
      <td>671B</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>87.5</td>
      <td>79.4</td>
      <td>86.9</td>
      <td>81.0</td>
      <td>73.3</td>
    </tr>
    <tr>
      <td>Qwen3-235B-A22B-Thinking-2507</td>
      <td>235B</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>92.3</td>
      <td>83.9</td>
      <td>-</td>
      <td>81.1</td>
      <td>-</td>
    </tr>
    <tr>
      <td rowspan="6">Proprietary VLM</td>
      <td>O3</td>
      <td>-</td>
      <td>82.9</td>
      <td>72.8</td>
      <td>25.2</td>
      <td>58.1</td>
      <td>59.8</td>
      <td>60.1</td>
      <td>88.9</td>
      <td>70.1</td>
      <td>86.7</td>
      <td>83.3</td>
      <td>75.8</td>
    </tr>
    <tr>
      <td>Claude4 Sonnet (thinking)</td>
      <td>-</td>
      <td>76.9</td>
      <td>64.6</td>
      <td>26.1</td>
      <td>48.1</td>
      <td>43.7</td>
      <td>57.0</td>
      <td>70.5</td>
      <td>-</td>
      <td>-</td>
      <td>75.4</td>
      <td>55.9</td>
    </tr>
    <tr>
      <td>Claude4 opus (thinking)</td>
      <td>-</td>
      <td>79.8</td>
      <td>66.1</td>
      <td>25.2</td>
      <td>49.3</td>
      <td>47.2</td>
      <td>59.9</td>
      <td>75.5</td>
      <td>-</td>
      <td>-</td>
      <td>79.6</td>
      <td>56.6</td>
    </tr>
    <tr>
      <td>Gemini 2.5 Flash (thinking)</td>
      <td>-</td>
      <td>73.2</td>
      <td>57.3</td>
      <td>20.1</td>
      <td>57.1</td>
      <td>61.1</td>
      <td>65.2</td>
      <td>72.0</td>
      <td>-</td>
      <td>-</td>
      <td>82.8</td>
      <td>61.9</td>
    </tr>
    <tr>
      <td>Gemini 2.5 Pro</td>
      <td>-</td>
      <td>81.7</td>
      <td>73.3</td>
      <td>30.8</td>
      <td>56.3</td>
      <td>66.8</td>
      <td>66.8</td>
      <td>88.0</td>
      <td>-</td>
      <td>-</td>
      <td>86.4</td>
      <td>71.8</td>
    </tr>
    <!-- 新增 Grok 4 -->
    <tr>
      <td>Grok 4</td>
      <td>-</td>
      <td>80.9</td>
      <td>70.3</td>
      <td>22.5</td>
      <td>40.7</td>
      <td>55.9</td>
      <td>64.8</td>
      <td>98.8</td>
      <td>93.9</td>
      <td>85.5</td>
      <td>87.5</td>
      <td>79.3</td>
    </tr>
  </tbody>
</table>

Note: Parts of the evaluation results are reproduced using the same settings.  
†: Evaluation results of Gemini 2.5 Flash (thinking) may be lower than real model performance, especially on MathVision, due to insufficient instruction following ability. 
## Deployment


> You can access Step3's API on https://platform.stepfun.com/ , we provide OpenAI/Anthropic-compatible API for you.
>

Our model checkpoints are stored in bf16 and block-fp8 format, you can find it on [Huggingface](https://huggingface.co/collections/stepfun-ai/step3-688a3d652dbb45d868f9d42d).

Currently, it is recommended to run Step3 on the following inference engines:

* vLLM
* SGLang

Deployment and Request examples for vLLM and SGLang can be found in the [Model Deployment Guide](docs/deploy_guidance.md).

## Contact Us
If you have any questions, please reach out at [contact@stepfun.com](mailto:contact@stepfun.com) .

## License
Both the code repository and the model weights are released under the [Apache License (Version 2.0)](./LICENSE).

## Citation
```
@misc{step3system,
      title={Step-3 is Large yet Affordable: Model-system Co-design for Cost-effective Decoding}, 
      author={StepFun Team},
      year={2025},
      eprint={2507.19427},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={https://arxiv.org/abs/2507.19427}, 
}

@misc{step3blog,
      title={Step3: Cost-Effective Multimodal Intelligence}, 
      author={StepFun Team},
      url={https://stepfun.ai/research/step3}, 
}
```
