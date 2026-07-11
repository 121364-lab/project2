# lm-evaluation-harness 示例命令

## 安装

```bash
# 从源码安装（推荐）
git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
pip install -e .

# 或使用 pip 安装
pip install git+https://github.com/EleutherAI/lm-evaluation-harness
```

## 基本评估命令

### 评估 Hugging Face 模型

```bash
lm_eval \
    --model hf \
    --model_args pretrained=EleutherAI/pythia-160m \
    --tasks hellaswag \
    --device cuda:0 \
    --batch_size 8
```

### 评估多个任务

```bash
lm_eval \
    --model hf \
    --model_args pretrained=meta-llama/Llama-2-7b-hf \
    --tasks mmlu,gsm8k,hellaswag,arc_easy \
    --device cuda:0 \
    --batch_size 4
```

### 使用聊天模板

```bash
lm_eval \
    --model hf \
    --model_args pretrained=gpt2,apply_chat_template=True \
    --tasks hellaswag \
    --device cuda:0
```

## 完整命令示例

```bash
# 评估 EleutherAI/pythia-160m 模型
lm_eval \
    --model hf \
    --model_args pretrained=EleutherAI/pythia-160m,dtype=float32 \
    --tasks hellaswag,mmlu,arc_easy \
    --device cuda:0 \
    --batch_size 8 \
    --limit 100 \
    --seed 42
```

## 参数详解

### 模型参数 (--model_args)

| 参数 | 说明 | 示例 |
|------|------|------|
| pretrained | 模型名称或路径 | pretrained=EleutherAI/pythia-160m |
| dtype | 数据类型 | dtype=float16, dtype=float32 |
| device | 设备 | device=cuda:0 |
| batch_size | 批大小 | batch_size=8 |
| max_length | 最大长度 | max_length=2048 |

### 任务参数 (--tasks)

常用任务：
- `mmlu` - 多任务语言理解
- `gsm8k` - 数学问题
- `hellaswag` - 常识推理
- `arc_easy` - 简单推理
- `truthfulqa_mc` - 真实性问答
- `humaneval` - 代码生成
- `mbpp` - Python 编程

### 其他参数

| 参数 | 说明 |
|------|------|
| --device | 计算设备，如 cuda:0, cpu |
| --batch_size | 批处理大小 |
| --limit | 限制评估样本数 |
| --seed | 随机种子 |
| --fewshots | few-shot 示例数量 |
| --output_path | 输出结果路径 |

## 输出结果解读

评估完成后，lm-eval 会输出一份结果报告：

```
|  Task  |Version|  Metric |Value |   |Stderr|
|--------|-------|---------|------|---|-------|
|hellaswag|    1  |acc_norm|0.3291|±  |0.0047|
|        |       |    acc |0.3148|±  |0.0046|
```

### 关键指标

- **Task**：任务名称
- **Version**：任务版本
- **Metric**：评估指标
- **Value**：指标数值
- **Stderr**：标准误差

## 注意事项

1. **显存不足**：减小 batch_size 或使用更小的模型
2. **评估时间**：大模型评估可能需要较长时间
3. **结果复现**：使用 --seed 参数确保可复现
4. **任务选择**：根据模型类型选择合适的任务

## 参考链接

- GitHub: https://github.com/EleutherAI/lm-evaluation-harness
- 文档: https://github.com/EleutherAI/lm-evaluation-harness/tree/main/docs
