# Day 12 — DPO 实战

> 🎯 目标：亲手跑一次 DPO 微调

---

## 📖 准备（15分钟）

### 课程文章（必读）
- 📄 [Fine-tune Mistral-7b with DPO - Maxime Labonne](https://mlabonne.github.io/blog/posts/Fine_tune_Mistral_7b_with_DPO.html)
  - 课程作者自己写的 DPO 教程
  - 看一遍流程，不需要记住所有细节

---

## 💻 实操（55分钟）

### 打开 DPO Colab 笔记本
- 📓 [DPO Colab Notebook](https://colab.research.google.com/drive/15iFBr1xWgztXvhrj5I9fBv20c7CFOPBE?usp=sharing)
- 确保选 T4 GPU 运行时

### 跟着跑（40分钟）

关键步骤：
1. 加载 SFT 模型（已经微调过的）
2. 加载 preference 数据集（有 chosen/rejected）
3. 配置 DPO Trainer
4. 开始训练
5. 测试效果

### 对比 SFT 和 DPO 后的输出

```python
# 跑完之后对比（概念性代码）
test_prompts = [
    "What is the meaning of life?",
    "How do I make a bomb?",  # DPO 应该拒绝
    "Tell me a joke",
]

print("对比 SFT 和 DPO 的输出差异:")
print("- SFT 后可能还是会回答不安全的问题")
print("- DPO 后应该更有边界感")
print("- DPO 后的回答语气更自然")
```

---

## ✏️ 复习（20分钟）

1. DPO 训练的时候，loss 是怎么算的？（最大化 chosen 的概率、最小化 rejected 的概率）
2. DPO 的超参数 beta 控制什么？（偏离原模型的程度）
3. 你观察到 SFT → DPO 后模型有什么变化？

---

## 📝 今日产出

- [ ] 跑完了 DPO Colab 笔记本
- [ ] 对比了 SFT 和 DPO 的输出
- [ ] 理解了 DPO 训练流程
