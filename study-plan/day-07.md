# Day 7 — 合成数据生成

> 🎯 目标：理解如何用 LLM 生成训练数据，以及数据质量过滤

---

## 📖 阅读（30分钟）

### 1. 课程章节（必读，20分钟）
- 📄 继续读 README 的 **"3. Post-Training Datasets"** 
  - 重点："Synthetic data generation" 部分
  - "Quality filtering" 部分

### 2. 了解 Distilabel（浏览，10分钟）
- 🌐 [Distilabel 文档](https://distilabel.argilla.io/dev/sections/pipeline_samples/)
  - 这是目前最流行的合成数据工具
  - 浏览一下 Pipeline 的概念

---

## 💻 实操（45分钟）

### 用一个小模型感受一下数据生成

Colab 里跑：

```python
!pip install openai

# 模拟用 GPT 生成训练数据
# 思路：给一个 seed topic，让模型生成问答对

import random

# 种子问题（实际生产中用 GPT-4 来 expand）
seed_questions = [
    "What is machine learning?",
    "Explain gradient descent",
    "What is a neural network?",
    "How does backpropagation work?",
]

# 模拟生成更多变体（实际中会用 LLM）
templates = [
    "Can you explain {topic} in simple terms?",
    "What is {topic} and why is it important?",
    "How would you describe {topic} to a 5-year-old?",
    "Give me a step-by-step explanation of {topic}",
]

print("=== 模拟合成数据生成 ===\n")
for seed in seed_questions[:2]:
    for tpl in templates:
        topic = seed.lower().replace("what is ", "").replace("explain ", "")
        prompt = tpl.format(topic=topic)
        # 在实际中，这里会调用 LLM API 生成 answer
        print(f"📝 Prompt: {prompt}")
        print(f"💬 (会由 LLM 生成对应的回答)\n")

print(f"种子问题 {len(seed_questions)} 个 × 模板 {len(templates)} 个 = {len(seed_questions) * len(templates)} 条数据")
```

### 数据去重演示

```python
# 简单的文本去重（实际中用 MinHash 或嵌入）
from difflib import SequenceMatcher

def similarity(a, b):
    return SequenceMatcher(None, a, b).ratio()

samples = [
    "Machine learning is a subset of artificial intelligence.",
    "Machine learning is a subset of AI that enables systems to learn.",
    "The weather today is sunny and warm.",
    "Machine learning is part of artificial intelligence.",
]

# 两两比较
for i, a in enumerate(samples):
    for j, b in enumerate(samples):
        if i < j:
            sim = similarity(a, b)
            if sim > 0.5:
                print(f"⚠️ 相似度 {sim:.1%}:")
                print(f"   A: {a}")
                print(f"   B: {b}\n")
```

---

## ✏️ 复习（15分钟）

1. 为什么要用合成数据？（人工标注太贵了）
2. 合成数据最大的风险是什么？（质量不高、同质化）
3. 去重为什么重要？（过拟合、数据污染）

---

## 📝 今日产出

- [ ] 理解了合成数据的流程
- [ ] 跑了数据去重演示
- [ ] 了解了 Distilabel 是什么
