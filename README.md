# DeepLearning_Project2_TaskB_Survey

## 任务选择

**本小组选择的是任务 B：Fine-tuning、Alignment 与 Evaluation Harness 技术调研**

## 小组成员及分工

- **姬祥发** - 技术调研、文档撰写
- **伍哲淼** - 技术调研、案例分析
- **杨骞** - 报告整理、图表制作

## 报告主题和主要内容

本报告围绕大语言模型 Post-training 技术展开系统调研，涵盖以下三大核心技术方向：

1. **Fine-tuning 技术**：详细阐述了全参数微调与参数高效微调的区别，深入分析了 LoRA、QLoRA 等代表性方法的原理与特点
2. **Alignment 技术**：系统梳理了从 RLHF 到 DPO、RLAIF、GRPO 等对齐方法的技术演进
3. **Evaluation Harness 技术**：重点分析了 EleutherAI lm-evaluation-harness 框架的功能与使用方法

## 调研的代表性论文、文档和开源项目

### 论文

- [1] Hu E J, Shen Y, Wallis P, et al. LoRA: Low-Rank Adaptation of Large Language Models[J]. arXiv preprint arXiv:2106.09685, 2021.
- [2] Dettmers T, Pagnoni A, Holtzman A, et al. QLoRA: Efficient Finetuning of Quantized LLMs[J]. arXiv preprint arXiv:2305.14314, 2023.
- [3] Ouyang L, Wu J, Jiang X, et al. Training language models to follow instructions with human feedback[J]. Advances in Neural Information Processing Systems, 2022.
- [4] Rafailov R, Park J, Li C, et al. Draft of Direct Preference Optimization: Your Language Model is Secretly a Reward Model[J]. arXiv preprint arXiv:2305.18290, 2023.
- [5] Bai Y, Kadavath S, Kundu S, et al. Constitutional AI: Harmlessness from AI Feedback[J]. arXiv preprint arXiv:2212.08073, 2022.
- [6] Team G. Anthropic's Claude Model Card[EB/OL]. https://www.anthropic.com/news/model-card-claude, 2023.
- [7] Taori R, Gulrajani I, Zhang T, et al. Stanford Alpaca: An Instruction-Following LLaMA Model[EB/OL]. https://github.com/tatsu-lab/stanford_alpaca, 2023.
- [8] Gao L, Biderman S, Black S, et al. A Framework for Few-Shot Language Model Evaluation[EB/OL]. https://github.com/EleutherAI/lm-evaluation-harness, 2024.

### 开源项目

- [EleutherAI/lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) - 大语言模型评估框架
- [huggingface/peft](https://github.com/huggingface/peft) - 参数高效微调库
- [huggingface/transformers](https://github.com/huggingface/transformers) - Transformers 库
- [stanford_alpaca](https://github.com/tatsu-lab/stanford_alpaca) - Stanford Alpaca 项目

## 技术路线图说明

本报告的技术路线图展示了大语言模型 Post-training 的完整流程：

```
Pretraining → SFT/Instruction Tuning → Alignment → Evaluation
```

### 主要阶段

1. **预训练 (Pretraining)**：在大规模文本上训练基础语言模型
2. **监督微调 (SFT)**：使用指令数据让模型学会遵循指令
3. **对齐训练 (Alignment)**：通过 RLHF/DPO 等方法对齐人类偏好
4. **评估 (Evaluation)**：使用 benchmark 全面评估模型能力

详见 [figures/post_training_pipeline.png](figures/post_training_pipeline.png)

## Evaluation Harness 示例命令说明

本报告提供了 lm-evaluation-harness 的详细使用示例：

### 基本评估命令

```bash
lm_eval \
    --model hf \
    --model_args pretrained=EleutherAI/pythia-160m \
    --tasks hellaswag,mmlu \
    --batch_size 8 \
    --device cuda:0
```

### 参数说明

- `--model hf`：指定模型类型为 Hugging Face 模型
- `--model_args pretrained=MODEL_NAME`：指定要评估的模型
- `--tasks TASK_NAME`：要评估的任务名称
- `--device cuda:0`：指定计算设备
- `--batch_size 1`：批大小，根据显存调整

详细示例请参考 [examples/lm_eval_example_commands.md](examples/lm_eval_example_commands.md)

## 小组对当前 LLM Post-training 技术的总结和理解

### Fine-tuning 发展趋势

- 从全参数微调向参数高效微调（LoRA、QLoRA）演进
- 降低了微调门槛，使更多人能够参与大模型微调

### Alignment 技术演进

- 从复杂的 RLHF 向更简单的 DPO 发展
- RLAIF 的兴起减少了对人类标注的依赖

### Evaluation 重要性

- 系统化、自动化的评估是模型研发不可或缺的工具
- 评估体系正从单一能力向多维度综合评估发展

## 目录结构

```
Project2_TaskB_Survey/
|
|-- README.md
|-- report/
|   |-- project_report.pdf
|-- figures/
|   |-- post_training_pipeline.png
|   |-- finetuning_comparison.png
|   |-- alignment_comparison.png
|-- notes/
|   |-- finetuning_notes.md
|   |-- alignment_notes.md
|   |-- evaluation_harness_notes.md
|-- examples/
|   |-- lm_eval_example_commands.md
|   |-- tool_usage_examples.md
|-- references/
    |-- references.md
```

## 使用说明

本项目主要作为技术调研报告展示。如需复现报告中的实验或评估命令，请参考 `examples/` 目录下的示例文件。

## 致谢

本报告使用了以下开源项目和工具：

- EleutherAI lm-evaluation-harness
- Hugging Face Transformers 和 PEFT
- Stanford Alpaca
- Matplotlib（用于图表生成）
