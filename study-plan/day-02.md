# Day 2 — 注意力机制深入

> 🎯 目标：彻底搞懂 Self-Attention、Multi-Head Attention、以及 Q/K/V 是什么

---

## 📖 阅读（35分钟）

### 1. 读文章（必读，25分钟）
- 📄 [Attention? Attention! - Lilian Weng](https://lilianweng.github.io/posts/2018-06-24-attention/)
  - 重点读：Seq2Seq → Attention → Self-Attention 的演化过程
  - 公式不用全推，理解 Q、K、V 的物理意义就行：
    - **Q (Query)**: "我在找什么？"
    - **K (Key)**: "我有什么标签？"
    - **V (Value)**: "我的实际内容是什么？"

### 2. 回顾昨天的可视化（10分钟）
- 再打开 [LLM Visualization](https://bbycroft.net/llm)，这次重点关注 Attention 层的动画

---

## 💻 实操（40分钟）

### 跑一个简单的 Attention 代码（必做）
- 📓 打开 Colab，新建一个 notebook
- 运行下面这段代码，感受 Attention 的计算过程：

```python
import torch
import torch.nn.functional as F

# 模拟：序列长度=4, 每个词的向量维度=8
Q = torch.randn(4, 8)   # 4个词的Query
K = torch.randn(4, 8)   # 4个词的Key
V = torch.randn(4, 8)   # 4个词的Value

# 计算注意力分数
scores = Q @ K.T / (8 ** 0.5)      # 点积 + 缩放
weights = F.softmax(scores, dim=-1) # Softmax 变成概率
output = weights @ V                 # 加权求和

print("注意力权重矩阵 (谁关注谁):")
print(weights)
print(f"\n每行加起来 = 1 吗？ {weights.sum(dim=-1)}")
```

### 改参数玩一玩（15分钟）
- 把序列长度从 4 改成 8
- 把向量维度从 8 改成 16
- 观察注意力权重矩阵的变化

---

## ✏️ 复习（15分钟）

回答这些问题：

1. Self-Attention 里,一句话中的每个词为什么要"看"其他所有词？
2. `Q @ K.T` 计算的是什么？（提示：相似度）
3. 为什么要除以 `sqrt(d_k)`？（缩放防止梯度消失）
4. Multi-Head Attention 中 "Multi-Head" 的好处是什么？

---

## 📝 今日产出

- [ ] 读完了 Lilian Weng 的文章
- [ ] 跑通了 Attention 代码
- [ ] 改了参数观察变化
- [ ] 回答了 4 个问题
