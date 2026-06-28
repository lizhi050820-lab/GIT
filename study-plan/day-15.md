# Day 15 — 第三周复盘

> 🎯 目标：复盘偏好对齐 & 评估相关内容，查漏补缺

---

## 📖 回顾（20分钟）

快速回顾这三周的内容结构：

```
LLM 训练全流程：

预训练 → SFT → 偏好对齐 → 评估 → 量化部署
  ↑        ↑       ↑        ↑       ↑
Day5    Day8-10  Day11-13  Day14   Day16-18
```

---

## 💻 实操（40分钟）

### 自由探索时间

选一个你感兴趣的 Colab 笔记本重新跑一遍，但**这次改点东西**：

- 📓 换一个数据集
- 📓 换一个模型（比如用 Qwen 代替 Llama）
- 📓 改 LoRA 参数观察变化

推荐：如果你对中文模型感兴趣，试试 Qwen2.5：

```python
# 加载 Qwen（中文友好）
from unsloth import FastLanguageModel

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Qwen2.5-1.5B",
    max_seq_length=2048,
    load_in_4bit=True,
)

# 用中文测试
from transformers import TextStreamer

prompt = "请用简单的话解释什么是机器学习"
inputs = tokenizer(prompt, return_tensors="pt")
# ... 生成回复
```

---

## ✏️ 第三周复盘（30分钟）

### 本周 Checklist

- [ ] Day 11: 理解了 DPO vs PPO
- [ ] Day 12: 跑了 DPO 微调
- [ ] Day 13: 知道了 GRPO 和推理模型
- [ ] Day 14: 会看模型评估指标了
- [ ] Day 15: 自由探索 + 复盘

### 核心问题

1. SFT → DPO 这个两阶段流程，每一步解决什么问题？
2. 如果要训练一个"数学很好的模型"，该用什么方法？（GRPO / PPO + 推理）
3. 怎么证明你的模型比别人的好？（跑 benchmark + 人工评估）

### 三周学完了，你现在能做什么？

- [ ] 能解释 Transformer 架构
- [ ] 能用 LoRA 微调一个开源模型
- [ ] 能准备微调数据
- [ ] 能用 DPO 做偏好对齐
- [ ] 能评估模型好坏

### 写下你下一步最想做的事

1. _________________________________
