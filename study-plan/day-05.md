# Day 5 — 预训练概览 + 第一周复盘

> 🎯 目标：了解预训练的全流程（不用自己训练），并复习本周内容

---

## 📖 阅读（35分钟）

### 1. 课程内容（必读，20分钟）
- 📄 读 README 中 **"2. Pre-Training Models"** 章节
  - 理解预训练的四个环节：数据准备 → 分布式训练 → 训练优化 → 监控
  - **不用深入细节**，知道大框架就行
  - 关键数字：Llama 3.1 用了 15 万亿 tokens 训练

### 2. 浏览 FineWeb 博客（速读，15分钟）
- 📄 [FineWeb - Hugging Face](https://huggingface.co/spaces/HuggingFaceFW/blogpost-fineweb-v1)
  - 看看真实的预训练数据长什么样
  - 重点理解"数据质量"的重要性

---

## 💻 实操（35分钟）

### 感受一下预训练的数据规模

在 Colab 里跑一个简单的文本统计：

```python
# 安装 datasets
!pip install datasets

from datasets import load_dataset

# 加载一个公开的小数据集（只是感受一下）
dataset = load_dataset("wikitext", "wikitext-2-raw-v1", split="train")

# 统计
total_chars = sum(len(text) for text in dataset["text"])
total_words = sum(len(text.split()) for text in dataset["text"])

print(f"数据条数: {len(dataset)}")
print(f"总字符数: {total_chars:,}")
print(f"总词数: {total_words:,}")
print(f"\n前 200 个字符:")
print(dataset[0]["text"][:200])

# 对比：Llama 3.1 用了 15 万亿 tokens
print(f"\n如果这个数据集是 1x，Llama 3.1 需要的量是它的 {15_000_000_000_000 / total_words:.0f} 倍")
```

---

## ✏️ 第一周复盘（20分钟）

### 本周 Checklist

- [ ] Day 1: Transformer 架构 → 理解了整体结构
- [ ] Day 2: 注意力机制 → 懂了 Q/K/V
- [ ] Day 3: Tokenization → 跑了 tokenizer
- [ ] Day 4: 采样策略 → 对比了不同策略
- [ ] Day 5: 预训练概览 → 知道了数据规模

### 自我检测

回答以下问题，不会的回头翻对应天的内容：

1. Transformer 的核心创新是什么？
2. Q、K、V 分别代表什么？
3. 为什么中文比英文消耗更多 token？
4. Temperature 和 Top-P 的区别是什么？
5. 预训练和监督微调（SFT）的关系是什么？

### 记录你的理解

写下 3 个你本周最深刻的理解：

1. 
2. 
3. 

记下 3 个你还没搞懂的问题，下周查：

1. 
2. 
3. 

---

## 📝 今日产出

- [ ] 读完预训练章节
- [ ] 跑完数据集统计代码
- [ ] 完成自我检测和复盘
