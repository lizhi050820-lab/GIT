# Day 17 — GGUF 与 llama.cpp

> 🎯 目标：学会把模型转成 GGUF 格式，用 llama.cpp 在本地跑

---

## 📖 阅读（20分钟）

### 1. 为什么需要 GGUF？
- 一般模型只能在 GPU 上跑
- GGUF 格式可以在 **CPU** 上跑！普通电脑也能用
- 把所有东西（权重、tokenizer、配置）打包成一个文件
- 配合 llama.cpp，在 Mac/Windows/Linux 都能跑

### 2. 课程文章（10分钟）
- 📄 [Quantize Llama models with llama.cpp](https://mlabonne.github.io/blog/posts/Quantize_Llama_2_models_using_ggml.html)

---

## 💻 实操（55分钟）

### 方法一：用课程的 Colab（推荐）
- 📓 [Quantization with GGUF and llama.cpp](https://colab.research.google.com/drive/1pL8k7m04mgE5jo2NrjGi8atB0j_37aDD)
- 跟着跑一遍，会把模型转成 GGUF 并上传到 Hugging Face

### 方法二：简单理解 GGUF 的结构

```python
# 概念演示：GGUF 是什么
print("GGUF 文件结构（简化为一个字典）：")
print("""
┌─────────────────────────┐
│  Header (元数据)         │
│  - 模型名称              │
│  - 架构类型 (llama)      │
│  - 量化级别 (Q4_K_M)     │
├─────────────────────────┤
│  Tokenizer               │
│  - 词表                  │
│  - Chat Template         │
├─────────────────────────┤
│  权重 (量化后的)          │
│  - layer_0.weight (INT4) │
│  - layer_1.weight (INT4) │
│  - ...                   │
└─────────────────────────┘

单个文件 ≈ 4 GB
拷贝到任意电脑 → llama.cpp 加载 → 本地运行！
""")
```

### 常见 GGUF 量化级别

```python
quants = [
    ("Q2_K",   "2-bit, 最小, 质量差"),
    ("Q4_K_M", "4-bit, 均衡, ★推荐"),
    ("Q5_K_M", "5-bit, 质量好, 稍大"),
    ("Q6_K",   "6-bit, 质量很好"),
    ("Q8_0",   "8-bit, 几乎无损"),
]
print("GGUF 量化级别:")
for q, desc in quants:
    print(f"  {q:10s} → {desc}")
```

---

## ✏️ 复习（15分钟）

1. GGUF 相比直接加载 HF 模型有什么优势？（CPU 运行、单文件、跨平台）
2. Q4_K_M 的 "K" 和 "M" 是什么意思？（K-quant 系列，M=Medium 大小）
3. 你的电脑能跑 GGUF 模型吗？（能！哪怕没有 GPU）

---

## 📝 今日产出

- [ ] 理解了 GGUF 的用途
- [ ] 跑了 Colab 转换 notebook（可选）
- [ ] 知道了怎么选量化级别
