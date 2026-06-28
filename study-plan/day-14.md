# Day 14 — 模型评估

> 🎯 目标：理解怎么判断一个 LLM 好不好

---

## 📖 阅读（35分钟）

### 1. 课程章节（必读，20分钟）
- 📄 读 README 中 **"6. Evaluation"** 章节
  - 三种评估方式：
    - **自动化基准测试**: MMLU、HellaSwag、GSM8K
    - **人类评估**: Chatbot Arena
    - **模型评估**: 用 GPT-4 当裁判

### 2. 浏览排行榜（15分钟）
- 🌐 [Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard)
  - 看看目前排名靠前的开源模型
  - 点进几个模型看看他们的分数
- 🌐 [Chatbot Arena](https://lmarena.ai/)
  - 这是最受认可的排行榜，人类盲测投票

---

## 💻 实操（40分钟）

### 用 lm-evaluation-harness 评估模型

Colab 里跑（需要 GPU）：

```python
!pip install lm-eval

# 评估一个小模型在 GSM8K（小学数学题）上的表现
# 注意：这个需要一点时间

!lm_eval --model hf \
    --model_args pretrained=gpt2 \
    --tasks gsm8k \
    --device cuda:0 \
    --batch_size 1 \
    --limit 5 \
    --output_path ./eval_results
```

### 理解评估指标

```python
print("常见评估基准：")
print("=" * 50)
benchmarks = {
    "MMLU": "多学科知识问答（57个学科）",
    "GSM8K": "小学数学应用题",
    "HellaSwag": "常识推理（选正确的下一句）",
    "HumanEval": "代码生成能力",
    "TruthfulQA": "是否倾向于胡说八道",
    "MT-Bench": "多轮对话质量（GPT-4 当裁判）",
}

for name, desc in benchmarks.items():
    print(f"  {name:15s} → {desc}")
```

---

## ✏️ 复习（15分钟）

1. MMLU 测的是什么？（知识广度）
2. 为什么自动化评估可能不准确？（数据污染、指标片面）
3. Chatbot Arena 为什么更靠谱？（真人盲测、持续更新）
4. 如果你微调了一个模型，用哪个 benchmark 来证明它变好了？

---

## 📝 今日产出

- [ ] 浏览了排行榜
- [ ] 理解了三种评估方式
- [ ] 知道了常见的评估 benchmark
