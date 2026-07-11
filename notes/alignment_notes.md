# Alignment 技术笔记

## 什么是 Alignment

Alignment（对齐）是指让大语言模型的行为符合人类价值观、意图和期望的过程。

核心关注维度：
- **Helpfulness（有用性）**：能真正帮助用户
- **Harmlessness（无害性）**：不生成有害内容
- **Honesty（诚实性）**：不编造事实
- **Reasoning（推理能力）**：复杂思维活动

## 为什么需要 Alignment

指令微调（Instruction Fine-tuning）存在的问题：
1. 行为对齐不充分
2. 难以覆盖所有边界情况
3. 无法有效处理偏好问题
4. 容易被越狱（Jailbreak）
5. 推理能力提升有限

## RLHF 流程

Reinforcement Learning from Human Feedback 包括三个阶段：

### 1. 监督微调 (SFT)

收集高质量人类示范数据，在预训练模型上进行监督微调。

### 2. 奖励模型训练 (Reward Modeling)

- 收集人类偏好数据（回答排序）
- 训练奖励模型预测人类偏好
- 使用排序损失优化

### 3. 强化学习优化 (PPO)

- 使用奖励模型作为奖励信号
- PPO 算法更新策略模型
- KL 散度惩罚防止偏离

## DPO 流程

Direct Preference Optimization 直接优化偏好：

```
损失 = -log(σ(r(x,y+) - r(x,y-)))
```

优点：
- 简单稳定，训练快
- 不需要奖励模型
- 不需要 PPO

缺点：
- 离线学习，假设较强
- 效果可能略逊于 RLHF

## RLAIF 流程

RL from AI Feedback 使用 AI 反馈替代人类反馈：

- 使用强模型（如 GPT-4）生成偏好
- 降低成本，提高可扩展性
- 依赖教师模型质量

## GRPO 流程

Group Relative Policy Optimization：

- 对同一问题生成多个回答
- 基于奖励函数选择最优
- 适合可以计算奖励的任务（如推理）

## 方法对比

| 方法 | 复杂度 | 数据需求 | 主要优势 | 主要局限 |
|------|--------|----------|----------|----------|
| RLHF | 高 | 大量人类偏好 | 效果好 | 复杂不稳定 |
| DPO | 低 | 偏好数据对 | 简单稳定 | 离线学习 |
| RLAIF | 中 | AI生成偏好 | 成本低 | 依赖教师 |
| GRPO | 中 | 可计算奖励 | 适合推理 | 需要采样 |

## 参考资料

- Ouyang L et al. Training language models to follow instructions with human feedback (2022)
- Rafailov R et al. Direct Preference Optimization (2023)
- Bai Y et al. Constitutional AI (2022)
