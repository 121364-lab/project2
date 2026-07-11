# Evaluation Harness 技术笔记

## 为什么需要评估

1. 指导研发方向
2. 支撑产品决策
3. 支撑模型迭代
4. 公平比较不同模型

## 为什么不能只依赖人工评价

- 成本高、耗时长
- 难以标准化
- 难以规模化
- 主观性强

## 基本概念

### Benchmark

标准化的测试基准，用于评估模型能力。

### Task

具体的评估任务，如问答、分类、摘要等。

### Metric

评估指标，如准确率(accuracy)、F1、BLEU、ROUGE 等。

### Few-shot Setting

使用少量示例帮助模型理解任务。

## lm-evaluation-harness

EleutherAI 开发的开源评估框架。

### 主要功能

- 支持 200+ 基准测试
- 支持多种模型类型（Hugging Face、VLLM、OpenAI 等）
- 支持自定义任务
- 标准化评估流程

### 安装

```bash
pip install git+https://github.com/EleutherAI/lm-evaluation-harness
```

或

```bash
git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e .
```

### 基本用法

```bash
lm_eval \
    --model hf \
    --model_args pretrained=MODEL_NAME \
    --tasks TASK_NAME \
    --device cuda:0 \
    --batch_size 1
```

### 参数说明

- `--model`：模型类型（hf, vllm, openai, anthropic 等）
- `--model_args`：模型参数
  - `pretrained`：模型名称或路径
  - `pretrained=EleutherAI/pythia-160m`
- `--tasks`：任务名称（逗号分隔多个）
  - `hellaswag,mmlu,gsm8k`
- `--device`：计算设备
- `--batch_size`：批大小

### 常用任务

| 任务 | 类型 | 说明 |
|------|------|------|
| MMLU | 知识 | 多任务语言理解 |
| GSM8K | 推理 | 数学问题 |
| HumanEval | 代码 | Python 编程 |
| HellaSwag | 推理 | 常识推理 |
| TruthfulQA | 安全 | 真实性问答 |

### 结果解读

评估结果包含：
- **Task**：任务名称
- **Version**：版本号
- **Metric**：指标名称
- **Value**：指标数值
- **Stderr**：标准误差

### 局限性

1. 基准测试可能被污染
2. 只能衡量特定能力
3. 可能存在过拟合基准
4. 自动评估指标有缺陷
5. 无法评估安全和对齐

## 参考资料

- Gao L et al. A Framework for Few-Shot Language Model Evaluation (2024)
- https://github.com/EleutherAI/lm-evaluation-harness
