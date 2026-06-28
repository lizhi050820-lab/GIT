# Day 10 — 微调进阶 + 第二周复盘

> 🎯 目标：了解更高效的微调工具 Unsloth，复盘本周内容

---

## 📖 阅读（20分钟）

### 1. Unsloth 介绍（必读，15分钟）
- 📄 [Fine-tune Llama 3.1 with Unsloth](https://huggingface.co/blog/mlabonne/sft-llama3)
  - Unsloth 是目前最快的微调工具，比标准 LoRA 快 2-5 倍
  - 不需要改代码，几乎直接替换即可

### 2. Axolotl 了解（浏览，5分钟）
- 🌐 [Axolotl 文档](https://axolotl.ai/)
  - 了解这是一个"微调配置化"的工具
  - 写一个 YAML 配置文件就能启动微调

---

## 💻 实操（45分钟）

### 用 Unsloth 微调（推荐）

在 Colab 里跑：

```python
# 安装 Unsloth
!pip install unsloth

from unsloth import FastLanguageModel
import torch

# 加载模型（Unsloth 自动优化）
model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/Llama-3.2-3B",
    max_seq_length=2048,
    load_in_4bit=True,
)

# 添加 LoRA
model = FastLanguageModel.get_peft_model(
    model,
    r=16,
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
    lora_alpha=16,
    lora_dropout=0,
)

print("✅ Unsloth 模型加载完成！")
print(f"可训练参数: {model.print_trainable_parameters()}")
```

### 对比昨天的代码

- 昨天用标准的 PEFT 库
- 今天用 Unsloth
- 加载速度明显更快

---

## ✏️ 第二周复盘（25分钟）

### 本周 Checklist

- [ ] Day 6: 理解了训练数据集的结构
- [ ] Day 7: 懂了合成数据是怎么生成的
- [ ] Day 8: 理解了 LoRA/QLoRA 原理
- [ ] Day 9: 亲手微调了一个模型！
- [ ] Day 10: 了解了 Unsloth 和 Axolotl

### 自我检测

1. SFT 数据的基本格式是什么？（instruction + response，或 messages 列表）
2. LoRA 比 Full Fine-tuning 省多少参数？（通常是原来的 0.1%~1%）
3. QLoRA 做了哪两件事？（4-bit 量化 + LoRA）
4. Unsloth 比标准 LoRA 快在哪里？（优化了 attention 计算和内存管理）
5. 你现在能自己微调一个中文模型吗？（能！换个中文数据集就行）

### 这段时间的感悟

1. 微调最难的不是代码，而是 _________
2. 如果要微调一个自己的模型，你想让它做什么？ _________

---

## 📝 今日产出

- [ ] 跑了 Unsloth 加载代码
- [ ] 完成第二周复盘
- [ ] 想好了自己要用微调做什么
