# Day 19 — 模型合并 & 多模态 & 前沿话题

> 🎯 目标：了解 LLM 领域的最新趋势

---

## 📖 阅读（35分钟）

### 1. 模型合并（必读，15分钟）
- 📄 [Merge LLMs with MergeKit - Maxime Labonne](https://mlabonne.github.io/blog/posts/2024-01-08_Merge_LLMs_with_mergekit%20copy.html)
  - 模型合并 = 把两个微调好的模型"融合"成一个
  - 不需要训练！纯数学操作
  - 方法：SLERP、DARE、TIES

### 2. 多模态（浏览，10分钟）
- 🌐 了解一下 CLIP、LLaVA、Stable Diffusion
  - 多模态 = 文字 + 图片 + 音频 + 视频
  - GPT-4V / Gemini 都是多模态模型

### 3. Test-time Compute（10分钟）
- 📄 读 README 中关于 "Test-time compute" 的部分
  - 让模型在回答前"多想几步"
  - DeepSeek-R1、OpenAI o1/o3 都用了这个技术

---

## 💻 实操（40分钟）

### 了解模型合并的概念

```python
# 概念演示：SLERP (Spherical Linear Interpolation)
import numpy as np

def slerp(w1, w2, t=0.5):
    """球面线性插值 - 把两个模型的权重在球面上混合"""
    omega = np.arccos(np.dot(w1, w2))
    sin_omega = np.sin(omega)
    return (np.sin((1-t)*omega) / sin_omega) * w1 + \
           (np.sin(t*omega) / sin_omega) * w2

# 模拟两个专家模型的权重
model_math = np.array([0.8, 0.3, 0.9, 0.1])   # 擅长数学
model_code = np.array([0.2, 0.9, 0.4, 0.8])   # 擅长编程

# 合并！
merged = slerp(model_math, model_code, t=0.5)
print("数学模型:", model_math)
print("编程模型:", model_code)
print("合并后的:", merged)
print("\n不需要重新训练，直接数学合并两个模型！")
```

### 可选的 Colab
- 📓 [Merge LLMs with MergeKit](https://colab.research.google.com/drive/1_JS7JKJAQozD48-LhYdegcuuZ2ddgXfr?usp=sharing)
- 📓 [LazyMergekit - 一键合并](https://colab.research.google.com/drive/1obulZ1ROXHjYLn6PPZJwRR6GzgQogxxb?usp=sharing)

---

## ✏️ 复习（15分钟）

1. 模型合并为什么不需要训练？（权重在参数空间做插值）
2. 多模态模型是怎么"看到"图片的？（图片也编码成向量，和文字向量放一起）
3. Test-time compute 的思路是什么？（推理时多用算力换更好的答案）

---

## 📝 今日产出

- [ ] 理解了模型合并的概念
- [ ] 知道了多模态的基本原理
- [ ] 了解了 test-time compute
