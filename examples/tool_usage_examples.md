# 工具使用示例

## PEFT (Parameter-Efficient Fine-Tuning)

### 安装

```bash
pip install peft transformers bitsandbytes accelerate
```

### LoRA 微调示例

```python
from peft import LoraConfig, get_peft_model, TaskType
from transformers import AutoModelForCausalLM, AutoTokenizer

# 加载模型
model = AutoModelForCausalLM.from_pretrained("meta-llama/Llama-2-7b-hf")
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-2-7b-hf")

# 配置 LoRA
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=8,
    lora_alpha=16,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.05,
    bias="none"
)

# 应用 LoRA
model = get_peft_model(model, lora_config)
model.print_trainable_parameters()
# 输出: trainable params: 8,388,608 || all params: 6,738,415,616 || trainable%: 0.1244%
```

### QLoRA 微调示例

```python
from peft import LoraConfig, get_peft_model, TaskType, prepare_model_for_kbit_training
from transformers import BitsAndBytesConfig, AutoModelForCausalLM, AutoTokenizer
import torch

# 4-bit 量化配置
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_compute_dtype=torch.float16,
    bnb_4bit_use_double_quant=True
)

# 加载量化模型
model = AutoModelForCausalLM.from_pretrained(
    "meta-llama/Llama-2-7b-hf",
    quantization_config=bnb_config,
    device_map="auto"
)

# 准备训练
model = prepare_model_for_kbit_training(model)

# 配置 LoRA
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=16,
    lora_alpha=32,
    target_modules=["q_proj", "v_proj"],
    lora_dropout=0.05,
    bias="none"
)

model = get_peft_model(model, lora_config)
```

## Hugging Face TRL (Training Reinforcement Learning)

### 安装

```bash
pip install trl transformers datasets
```

### DPO 训练示例

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from trl import DPOTrainer
from datasets import load_dataset

# 加载模型
model = AutoModelForCausalLM.from_pretrained("gpt2")
tokenizer = AutoTokenizer.from_pretrained("gpt2")

# 准备数据
dataset = load_dataset("anthropic/hh-rlhf", split="train")
dataset = dataset.select(range(1000))

# DPO 训练器
dpo_trainer = DPOTrainer(
    model=model,
    ref_model=None,  # 或指定参考模型
    args=training_args,
    beta=0.1,
    train_dataset=dataset,
    tokenizer=tokenizer,
)

dpo_trainer.train()
```

### RLHF PPO 训练示例

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from trl import PPOConfig, PPOTrainer
from datasets import load_dataset

ppo_config = PPOConfig(
    model_name="gpt2",
    learning_rate=1.41e-5,
    batch_size=1,
    forward_batch_size=1,
)

ppo_trainer = PPOTrainer(
    model=ppo_config.model_name,
    config=ppo_config,
    dataset=dataset,
    tokenizer=tokenizer,
)

# 训练循环
for epoch, batch in enumerate(ppo_trainer.dataloader):
    # 生成响应
    responses = [generate_response(query) for query in batch["query"]]
    
    # 计算奖励
    rewards = [compute_reward(response) for response in responses]
    
    # PPO 步骤
    ppo_trainer.step(batch["query"], responses, rewards)

ppo_trainer.save_model("ppo_model")
```

## lm-evaluation-harness API 使用

### Python API 评估

```python
from lm_eval import evaluator, tasks

# 评估模型
results = evaluator.simple_evaluate(
    model="hf",
    model_args="pretrained=gpt2",
    tasks="hellaswag",
    num_fewshot=10,
    batch_size=8,
    device="cuda:0",
)

print(results["results"])
```

## 参考链接

- PEFT: https://github.com/huggingface/peft
- TRL: https://github.com/huggingface/trl
- lm-evaluation-harness: https://github.com/EleutherAI/lm-evaluation-harness
