# Day 1 — Transformer 架构入门

> 🎯 目标：理解 Transformer 的整体结构，搞懂 Encoder-Decoder 到 GPT 的演变

---

## 📖 阅读（35分钟）

### 1. 看视频（必看，20分钟）
- 🎬 [3Blue1Brown - Visual intro to Transformers](https://www.youtube.com/watch?v=wjZofJX0v4M)
  - 不需要全懂，重点是理解"注意力"这个概念
  - 注意看视频里的动画演示

### 2. 看文章（必读，15分钟）
- 📄 阅读课程 README 中 **"The LLM Architecture"** 章节
  - 文件路径：`README.md` → `🧑‍🔬 The LLM Scientist` → `1. The LLM Architecture`
  - 重点看 "Architectural overview" 部分

---

## 💻 实操（40分钟）

### 玩一下交互式可视化（必做）
- 🔮 [LLM Visualization (bbycroft.net)](https://bbycroft.net/llm)
  - 这是一个 **3D 交互式可视化**，展示 LLM 内部结构
  - 用鼠标旋转、放大，点进去看每个组件
  - 尝试理解：输入文本 → Tokenization → Embedding → Attention → Output

### 可选（有时间就做）
- 打开 [nanoGPT 视频](https://www.youtube.com/watch?v=kCc8FmEb1nY) 看前 15 分钟，感受一下从零实现 GPT

---

## ✏️ 复习 & 笔记（15分钟）

用你自己的话写下：

1. Transformer 和之前的 RNN/LSTM 最大的区别是什么？
2. "注意力机制"解决的是什么问题？
3. GPT 和原始 Transformer 的结构有什么不同？（提示：只有 Decoder）
4. 写下 5 个你还不太理解的关键词，明天重点关注

---

## 📝 今日产出

- [ ] 看完了 3B1B 视频
- [ ] 玩了一遍 LLM Visualization
- [ ] 写下了 4 个问题的答案


今日心得：
1.观看了3B1B的介绍视频，大概理解了ai模型回答我们的方式，最让我感慨的是数学的作用
我想象中的ai回答问题是检索关键词然后在语言库以及其他库寻找资料来解答，没想到是通过
将我发送的东西转换成tokens即一个个矩阵，然后每个token间再相互寻找关联，有一个注意力
集中机制，把一个相同的词在不同语境中的意思中的矩阵表达出来，最后再进行一个多模态交互
理解出我发给他的那句话是什么意思，然后寻找相应的答案。
2.还有个让我印象深刻的点是他用三维坐标表示出一个个token在其中的位置，然后两个相近的
tokens可以通过矩阵的点积乘值来确定他们之间的关联程度，比如cat和animals,dragon和
monster,关联程度比较高，所以他们从原点到相应坐标向量的方向相机，点积出来是数值就是
正的。
3.通过上面所说的，我们假如不能描述出一个东西，可以通过几个向量的相加减来确定一个大致
范围，最后让ai来帮我检索出来，比如我想知道芯片，但是我不知道怎么描述，我就说手机，主板
核心，这几个tokens的组合，然后就可以得出大致范围。
