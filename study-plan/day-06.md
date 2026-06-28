# Day 6 — 训练后数据集

> 🎯 目标：理解微调需要什么样的数据，长什么样子

---

## 📖 阅读（35分钟）

### 1. 课程章节（必读，20分钟）
- 📄 读 README 中 **"3. Post-Training Datasets"** 章节
  - 重点理解 "chat templates"（对话模板）的概念
  - 知道 ShareGPT 格式 vs OpenAI 格式的区别

### 2. 看实际数据（15分钟）
- 🌐 打开 [LLM Datasets repo](https://github.com/mlabonne/llm-datasets)
  - 浏览一下有哪些公开数据集
  - 关注：OpenHermes、UltraChat、Orca 这几个名字

---

## 💻 实操（40分钟）

### 看看微调数据长什么样

打开 Colab：

```python
!pip install datasets

from datasets import load_dataset

# 加载一个 SFT 数据集
dataset = load_dataset("HuggingFaceH4/ultrachat_200k", split="train_sft[:100]")

# 看看格式
print("数据集大小:", len(dataset))
print("\n--- 第一条数据 ---")
print(dataset[0])

# 看看对话结构
for i, example in enumerate(dataset[:3]):
    print(f"\n=== 对话 {i+1} ===")
    for msg in example["messages"]:
        role = msg["role"]
        content = msg["content"][:150]
        print(f"[{role}]: {content}...")
```

### 理解 Chat Template

```python
from transformers import AutoTokenizer

# 加载 Llama 3 的 tokenizer
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Meta-Llama-3-8B")

# 看它的 chat template
print(tokenizer.chat_template[:500] if tokenizer.chat_template else "No chat template")
```

---

## ✏️ 复习（15分钟）

1. SFT 数据和预训练数据最大的区别是什么？（有对话结构 vs 纯文本）
2. Chat Template 的作用是什么？
3. 为什么不能直接用网上爬的对话来微调？（需要清洗）

---

## 📝 今日产出

- [ ] 读完 Post-Training Datasets 章节
- [ ] 跑通了数据集查看代码
- [ ] 理解了对话数据的结构
