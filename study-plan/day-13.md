# Day 13 — GRPO 与推理模型

> 🎯 目标：理解 GRPO（DeepSeek-R1 用的方法）和推理模型的思路

---

## 📖 阅读（40分钟）

### 1. GRPO 原理（20分钟）
- 📄 读 README 中关于 GRPO 的部分
- GRPO = Group Relative Policy Optimization
  - 这是 DeepSeek-R1 用的训练方法
  - 关键创新：不需要 Reward Model，用**组内对比**来算奖励

### 2. 推理模型（20分钟）
- 🌐 了解 DeepSeek-R1 的训练流程
- 核心概念：
  - **Chain-of-Thought (CoT)**: 让模型"先想再答"
  - **Test-time compute**: 推理时多算几步，效果更好
  - **Process Reward Model (PRM)**: 不仅看最终答案，还看中间步骤

---

## 💻 实操（35分钟）

### 概念代码：理解 GRPO 的"组内对比"

```python
import torch
import torch.nn.functional as F

# GRPO 的核心思想：同一道题，生成多个答案，好的加分，坏的减分

# 模拟：一道数学题，生成 4 个答案
responses = ["答案A=42", "答案B=100", "答案C=41", "答案D=0"]
# 正确答案是 42

# 模拟奖励
rewards = torch.tensor([1.0, -0.5, 0.8, -1.0])  # A最好, D最差

# GRPO: 用组内平均作为 baseline
baseline = rewards.mean()
advantages = rewards - baseline

print("GRPO 组内对比:")
print(f"组平均 reward: {baseline:.3f}")
print()
for i, (resp, r, adv) in enumerate(zip(responses, rewards, advantages)):
    sign = "+" if adv > 0 else ""
    print(f"  回答{i+1}: {resp:15s} | reward={r:5.1f} | advantage={sign}{adv:.3f}")
print()
print("关键思想：超过平均的 → 加强，低于平均的 → 抑制")
print("这样就不需要外部的 Reward Model 了！")
```

### 可选的 Colab 练习
- 📓 [Fine-tune with GRPO](https://huggingface.co/learn/llm-course/en/chapter12/5)（如果有时间）

---

## ✏️ 复习（15分钟）

1. GRPO 和 PPO 最大的区别是什么？（GRPO 用组内 baseline，不需要单独的 value model）
2. 推理模型和普通对话模型的区别？（推理模型会展示思考过程）
3. DeepSeek-R1 为什么火了？（用 GRPO 在推理任务上超过了 GPT-4o）

---

## 📝 今日产出

- [ ] 理解了 GRPO 的思路
- [ ] 知道了推理模型是什么
- [ ] 跑了 GRPO 概念代码
