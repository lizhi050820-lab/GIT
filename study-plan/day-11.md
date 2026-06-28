# Day 11 — 偏好对齐：DPO 理论

> 🎯 目标：理解为什么 SFT 之后还需要偏好对齐，以及 DPO 的原理

---

## 📖 阅读（40分钟）

### 1. 课程章节（必读，20分钟）
- 📄 读 README 中 **"5. Preference Alignment"** 章节
  - 理解 SFT vs 偏好对齐的关系：
    - **SFT**: 教模型"怎么说"
    - **偏好对齐**: 教模型"什么是对的"

### 2. RLHF 概述（必读，20分钟）
- 📄 [Illustrating RLHF - Hugging Face](https://huggingface.co/blog/rlhf)
  - 这是经典文章，值得仔细读
  - RLHF 三步走：
    1. SFT（监督微调）
    2. 训练 Reward Model（奖励模型）
    3. PPO 强化学习

---

## 💻 实操（35分钟）

### 理解 DPO vs PPO

```python
# 概念对比（不需要跑，阅读即可）

print("=" * 50)
print("RLHF 流程（PPO）:")
print("=" * 50)
print("""
  SFT模型 → 生成多个回答 → Reward Model打分
       ↓
  用 PPO 更新模型（需要同时跑4个模型！）
       ↓
  对齐后的模型
""")

print("=" * 50)
print("DPO 流程（更简单）:")
print("=" * 50)
print("""
  准备好 (prompt, chosen, rejected) 三件套
       ↓
  直接用 chosen/rejected 的对比来优化
       ↓
  对齐后的模型 ✓
  
  DPO 优势：不需要 Reward Model，不需要 PPO
  DPO 劣势：效果略逊于 PPO
""")
```

### 看看 DPO 数据集长什么样

```python
!pip install datasets

from datasets import load_dataset

# 加载一个 preference 数据集
dataset = load_dataset("argilla/ultrafeedback-binarized-preferences-cleaned", split="train[:5]")

for i, example in enumerate(dataset):
    print(f"\n=== 样本 {i+1} ===")
    print(f"Prompt: {example['prompt'][:100]}...")
    print(f"Chosen (好的回答): {str(example['chosen'])[:100]}...")
    print(f"Rejected (差的回答): {str(example['rejected'])[:100]}...")
```

---

## ✏️ 复习（15分钟）

1. SFT 已经教了模型怎么说，为什么还需要偏好对齐？（减少幻觉、改善语气、对齐安全）
2. DPO 比 PPO 好在哪？（简单、不需要 reward model）
3. PPO 比 DPO 好在哪？（效果略好、适合推理模型）

---

## 📝 今日产出

- [ ] 读完 RLHF 文章
- [ ] 理解了 DPO vs PPO
- [ ] 看了 DPO 数据集格式
