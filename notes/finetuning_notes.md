# Fine-tuning 技术笔记

## 什么是 Fine-tuning

Fine-tuning（微调）是指在预训练模型的基础上，使用特定任务的数据集对模型进行进一步训练，以适应下游任务需求的过程。

## Full Fine-tuning vs PEFT

### Full Fine-tuning（全参数微调）

- 更新模型所有参数
- 显存需求高（~4x 模型大小）
- 性能最佳，但成本高

### Parameter-Efficient Fine-tuning (PEFT)

- 只更新少量参数
- 显存需求低
- 代表方法：LoRA, QLoRA, Adapter, Prefix Tuning

## LoRA 核心思想

LoRA (Low-Rank Adaptation) 的核心公式：

```
h = Wx + ΔWx = Wx + BAx
```

其中：
- W ∈ R^(d×k) 是原始权重，被冻结
- B ∈ R^(d×r) 和 A ∈ R^(r×k) 是可训练的低秩矩阵
- r 是秩，通常 r << d, k

### LoRA 优势

1. 参数量极少（0.1%-1%）
2. 无推理延迟（可合并权重）
3. 可插拔设计
4. 不易灾难性遗忘

## QLoRA 核心思想

QLoRA 在 LoRA 基础上引入 4-bit NormalFloat (NF4) 量化：

1. **NF4 量化**：专为正态分布权重设计
2. **双重量化**：对量化常数再量化
3. **分页优化器**：支持 CPU 卸载

### QLoRA 效果

- 7B 模型仅需 6-10GB 显存
- 性能与 16-bit LoRA 相当

## 资源需求对比

| 方法 | 7B 模型显存 |
|------|------------|
| Full FT (FP16) | 80-100GB |
| LoRA (FP16) | 20-30GB |
| QLoRA (4-bit) | 6-10GB |

## 适用场景

- Full FT：数据充足、追求最佳性能
- LoRA：通用场景，平衡性能和成本
- QLoRA：显存受限、消费级硬件

## 参考资料

- Hu E J et al. LoRA: Low-Rank Adaptation of Large Language Models (2021)
- Dettmers T et al. QLoRA: Efficient Finetuning of Quantized LLMs (2023)
