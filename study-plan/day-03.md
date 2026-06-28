# Day 3 — Tokenization 分词

> 🎯 目标：理解文本如何变成模型能处理的数字，以及不同分词策略的影响

---

## 📖 阅读（30分钟）

### 1. 看视频（必看，20分钟）
- 🎬 [Andrej Karpathy - Tokenization](https://www.youtube.com/watch?v=zduSFxRajkE)
  - 前 20 分钟即可（后面可以跳过）
  - Karpathy 是 OpenAI 联合创始人，讲的非常通俗

### 2. 补充阅读（10分钟）
- 📄 读课程 README 中 Tokenization 部分
  - 关注：BPE（字节对编码）、WordPiece、SentencePiece 的区别
  - 知道常见的 tokenizer：GPT 用 BPE，BERT 用 WordPiece

---

## 💻 实操（45分钟）

### 玩一下 tokenizer（必做）

打开 Colab，跑这个：

```python
# 安装 transformers
!pip install transformers

from transformers import AutoTokenizer

# 加载 GPT-2 的 tokenizer
tokenizer = AutoTokenizer.from_pretrained("gpt2")

# 测试中文和英文
texts = [
    "Hello, how are you?",
    "我今天学了 Transformer",
    "Let's learn about LLMs!",
    "一个中文词可能被切成好几个 token"
]

for text in texts:
    tokens = tokenizer.tokenize(text)
    ids = tokenizer.encode(text)
    print(f"原文: {text}")
    print(f"Token: {tokens}")
    print(f"ID: {ids}")
    print(f"Token数量: {len(tokens)}")
    print("---")
```

### 观察发现（10分钟）
- 中文一个字一个 token？还是多个字一个 token？
- 英文单词和中文词哪个"更省 token"？
- 同样的意思，英文和中文谁消耗的 token 更多？

---

## ✏️ 复习（15分钟）

1. 为什么需要 tokenizer？（文本 → 数字）
2. BPE 的基本原理是什么？（频繁出现的合并成一个）
3. 为什么中文在 LLM 里比英文"贵"？
4. GPT 系列和 Llama 系列的 tokenizer 有什么不同？（查一下）

---

## 📝 今日产出

- [ ] 看了 Karpathy 的 tokenization 视频
- [ ] 跑通了 tokenizer 代码
- [ ] 对比了中英文 token 数量
- [ ] 理解了为什么中文更费 token
