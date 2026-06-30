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
1.因为自我注意里，每个词是根据上面那个词来预测概率最大的词最后选出来的，所以看其他所有词的目的是自我联系上下文得出一个最合理，概率最大衔接上下文的词。也是破解了RNN的长距离依赖困境，即信息是一步一步传播的，这个能使信息同时被阅读和传递（虽然有点反直觉）。
2.我觉得主要计算的是"词与词之间的注意力原始分数"——即每个 Query 和每个 Key 的相似度
3.均值为0，方差为1
4.一种关系不够，让模型同时从多个角度理解句子
---

## 📝 今日产出

- [ ] 读完了 Lilian Weng 的文章
- [ ] 跑通了 Attention 代码
- [ ] 改了参数观察变化
- [ ] 回答了 4 个问题

总结：1.LSTM-long short term memory,长短期记忆网络。用来帮助机器联系上下文并且能循环思考，
  用我能表达的形式就是，我在小说的开头埋下伏笔，再末尾做解释时，读者能回忆起这个看似无关紧要的
  伏笔，显然我在和ai交流时不会使用这么隐晦的伏笔，但是ai也需要记忆下我在早时间给出的信息，并在
  之后的交流推理工作中记住这些信息，而不至于随着输入信息过多而丢失。
  2.RNN，即循环神经网络，普通的神经网络（前馈网络）就像一个人每次只看当前输入就做决定，完全不管之前发生了什么。比如你看到单词
  "dog"，就去判断词性，但不能利用前面 "The quick brown" 这个上下文。 RNN（Recurrent Neural Network，循环神经网络）则有一
  个**"记忆"机制**——它每处理一个词，不只看当前的输入，还会参考上一步留下的"印象"（隐藏状态）。
  3.LSTM的四个核心公式，遗忘门，输入门，细胞状态更新和输出门。
  4."第一个词"的概率 = 以起始标记为条件的条件概率】，它编码了模型从训练数据中学到的"句子如何开头"的先验知识 +
  ▎ 初始隐藏状态的偏置。在实际对话中，如果前面有提示或系统指令，这些全部会作为上下文参与第一个词的预测。
  5.提交测试
