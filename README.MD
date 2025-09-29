<div align="center">

# LegalEval-Q

## A New Benchmark for The Quality Evaluation of LLM-Generated Legal Text

[README_ZH](./README_zh.MD.md)

</div>

## Resource Occupancy 

python = 3.12.9; 

Disk usage: approximately 17G; 

Approximate VRAM usage: 18G;

## Configure the environment
```bash
pip install -r requirements.txt
```
## Model Download

Qwen_7B_Review_Tuned_model : https://www.modelscope.cn/l424102993/LLM_TQ_Tuned_model.git

Regression_model_base : https://www.modelscope.cn/iic/nlp_bert_backbone_base_std.git

Regression_model_regression : https://www.modelscope.cn/l424102993/LLM_TQ_Regression_model.git

Use Git for download (choose one between Git and Magic Tower SDK)

```bash
cd data/models
git clone https://www.modelscope.cn/l424102993/LLM_TQ_Tuned_model.git
git clone https://www.modelscope.cn/iic/nlp_bert_backbone_base_std
git clone https://www.modelscope.cn/l424102993/LLM_TQ_Regression_model.git
```

Download using the Magic Tower SDK (recommended)

```python
#SDK Model Download
from modelscope import snapshot_download
model_dir = snapshot_download('l424102993/LLM_TQ_Tuned_model', cache_dir = "./data/models/")
model_dir = snapshot_download('iic/nlp_bert_backbone_base_std', cache_dir = "./data/models/")
model_dir = snapshot_download('l424102993/LLM_TQ_Regression_model', cache_dir = "./data/models")
## Replace other paths
```
## Usage Method 
### 1. Modify the configuration file 

Modify the configuration file: ./configs/config.yaml 

Modify the model directory in config.ymal and specify the CUDA number based on the download model's address. 

### 2. Start the requests scoring service 

2.1 Directly use the "bass" command to start the request service

```bash
bash serve.py
```

2.2 Manual Startup

If you need to specify a virtual environment or other requirements, you can manually start ./src/evaluator_request.py

Note that when starting manually, you may need to modify the relative path for reading config.yaml in evaluator_request.py, as well as the relative path for the model in config.yaml

### 3. Add API keys

At the corresponding location in Evaluate_example.ipynb, add the API keys for Qwen, or modify the relevant code to call other models 

### 4. Model Testing

Use Evaluate_example.ipynb to conduct a single model's scoring test once.

## Batch Evaluation 

1. Use/notebooks/Evaluate_batch.ipynb to call the API interface or the local model to conduct scoring tests for multiple models 

2. Use the notebook "Generated_Result_Visualization_Analysis.ipynb" to visually view and compare the analysis results.

# Research Results 

## Factors Affecting Model Quality 

Model parameter size: 7B - 14B - Best value for money 
Due to the influence of the model training method and the training dataset, the text quality score and reasoning ability of the model approach a bottleneck around 7B-14B.

![文本得分及数学能力对比图](./images/fig_dis_f2_Score_and_math.png)

When deploying large models in vertical domains, there is no need to aim for overly large models, as their marginal effects tend to diminish significantly. Instead, it is the update of the model architecture or the quality of the original dataset/micro-tuned dataset that has a greater impact on the model. 

## Reasoning Model: The reasoning model is superior to the ordinary model. 

Although there have been numerous papers demonstrating the effectiveness of CoT and the performance of the reasoning models, most of the arguments have focused on the differences in reasoning abilities (such as compliance with instructions, mathematical ability, etc.) of the reasoning models. However, we have found that for the quality of the output text alone, the reasoning models also perform significantly better than ordinary models.

![推理模型和普通模型](./images/fig_dis_f4_Reasoning_Base.png)


## Selection of the Best Model: 

### Price 

Different models vary greatly in their usage prices due to their suppliers and API channels. This experiment compared the usage prices of APIs (RMB/1M tokens) with the quality of the output text. Among the currently tested models, the ones with the highest cost-performance ratio are vocles-lite, qwen-plus, and deepseek-v3-0324.

![推理模型和普通模型](./images/fig_dis_f6_Price_AdjCV.png)

## Cite
```
@misc{yunhan2025legalevalqnewbenchmarkquality,
      title={LegalEval-Q: A New Benchmark for The Quality Evaluation of LLM-Generated Legal Text}, 
      author={Li yunhan and Wu gengshen},
      year={2025},
      eprint={2505.24826},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2505.24826}, 
}
```
