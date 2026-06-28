# Day 9 — 微调实战：用 QLoRA 微调 Llama

> 🎯 目标：在 Colab 里亲自动手微调一个小模型

---

## 📖 准备（10分钟）

打开课程提供的 Colab 笔记本：
- 📓 [Fine-tune Mistral-7b with QLoRA](https://colab.research.google.com/drive/1o_w0KastmEJNVwT5GoqMCciH-18ca5WS?usp=sharing)
- 先浏览一遍，不要急着跑

---

## 💻 实操（60分钟）

### 跟着笔记本一步步跑

这是你**第一个真正的微调实战**！

1. **Setup（5分钟）**
   - 点 `Runtime` → `Change runtime type` → 选 `T4 GPU`
   - 运行安装代码块

2. **加载模型（10分钟）**
   - 用 4-bit 量化加载 Mistral-7B
   - 观察显存占用：原模型 14GB → 量化后 ~4GB

3. **配置 LoRA（5分钟）**
   - 设置 r=16, alpha=32
   - 选 target_modules

4. **加载数据（10分钟）**
   - 看看训练数据长什么样

5. **训练（20分钟）**
   - 跑训练循环
   - 观察 loss 下降

6. **测试（10分钟）**
   - 和你微调后的模型聊天！

### 关键观察

记录这些数字：
- 训练前的 loss: _____
- 训练后的 loss: _____
- 显存占用峰值: _____
- 训练时间: _____

---

## ✏️ 复习（20分钟）

1. 你微调的模型和原始的有什么区别？
2. loss 下降了多少？
3. batch size 和 gradient accumulation 的关系是什么？
4. 如果显存不够怎么办？（减小 batch size、减小 rank、用更小的模型）

---

## 📝 今日产出

- [ ] 成功跑通了一次微调！
- [ ] 记录了训练的关键数字
- [ ] 和自己微调后的模型对话了
