# Day 4 — 文本生成与采样策略

> 🎯 目标：理解模型如何"一个字一个字"生成文本，以及不同采样方式的效果

---

## 📖 阅读（30分钟）

### 1. 读课程文章（必读，20分钟）
- 📄 [Decoding Strategies in LLMs - Maxime Labonne](https://mlabonne.github.io/blog/posts/2023-06-07-Decoding_strategies.html)
  - 课程作者自己写的，非常清晰
  - 重点理解每种策略的原理和区别

### 2. 概念速览（10分钟）
这几种采样方式，先记住对比：

| 策略 | 怎么选下一个词 | 特点 |
|------|------|------|
| Greedy | 选概率最高的 | 确定性强，但容易重复 |
| Beam Search | 保留 Top-K 条路径 | 比 Greedy 好，但多样性差 |
| Temperature | 调整概率分布的"平滑度" | T>1 更有创意，T<1 更保守 |
| Top-K | 只从概率最高的 K 个里选 | K 小=保守，K 大=多样 |
| Top-P (Nucleus) | 累积概率到 P 就截断 | 目前最常用 |

---

## 💻 实操（45分钟）

### 跑采样对比实验（必做）

打开 Colab，跑这个（需要先安装 transformers）：

```python
!pip install transformers torch

from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

# 加载一个小模型（GPT-2 的中文不太行，但够演示）
model_name = "gpt2"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)

# 设置 pad_token
tokenizer.pad_token = tokenizer.eos_token

prompt = "The future of AI is"

def generate_with_strategy(prompt, **kwargs):
    inputs = tokenizer(prompt, return_tensors="pt")
    with torch.no_grad():
        outputs = model.generate(
            inputs.input_ids,
            max_new_tokens=50,
            do_sample=True,
            pad_token_id=tokenizer.eos_token_id,
            **kwargs
        )
    return tokenizer.decode(outputs[0], skip_special_tokens=True)

# 对比不同策略
print("=" * 50)
print("🌡️ Temperature = 0.7 (默认)")
print(generate_with_strategy(prompt, temperature=0.7, top_k=0, top_p=0))

print("\n" + "=" * 50)
print("🌡️ Temperature = 1.5 (更有创意)")
print(generate_with_strategy(prompt, temperature=1.5))

print("\n" + "=" * 50)
print("🎯 Top-K = 5 (很保守)")
print(generate_with_strategy(prompt, top_k=5))

print("\n" + "=" * 50)
print("🎯 Top-P = 0.9 (Nucleus Sampling)")
print(generate_with_strategy(prompt, top_p=0.9, top_k=0))
```

### 自己试（10分钟）
- 换个你自己的 prompt
- 把 temperature 调到 2.0 看看效果
- 把 temperature 调到 0.1，对比输出

---

## ✏️ 复习（15分钟）

1. 为什么不能用纯 Greedy？（输出会重复）
2. Temperature = 0 会怎样？Temperature = 100 会怎样？
3. Top-P 和 Top-K 能同时用吗？
4. 你觉得 ChatGPT 可能用了哪种策略？

---

## 📝 今日产出

- [ ] 读完 Decoding Strategies 文章
- [ ] 跑通了采样对比代码
- [ ] 自己改了 prompt 和参数
