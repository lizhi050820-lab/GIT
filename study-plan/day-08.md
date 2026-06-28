# Day 8 — 监督微调 SFT 入门

> 🎯 目标：理解 SFT 是什么，Full Fine-tuning vs LoRA vs QLoRA 的区别

---

## 📖 阅读（35分钟）

### 1. 课程章节（必读，20分钟）
- 📄 读 README 中 **"4. Supervised Fine-Tuning"** 章节
  - 重点对比两种方式：
    - **Full Fine-tuning**: 更新所有参数，贵但效果好
    - **LoRA**: 只训练一个小插件，省显存
    - **QLoRA**: 在 LoRA 基础上再加量化，更省

### 2. LoRA 原理速览（15分钟）
- 📄 [LoRA insights - Sebastian Raschka](https://lightning.ai/pages/community/lora-insights/)
  - Raschka 是 ML 教育大神，讲得特别清楚
  - 记住核心：LoRA = 在原模型旁边加两个小矩阵，只训这两个

---

## 💻 实操（40分钟）

### 可视化 LoRA 的思想

Colab 里跑：

```python
import torch

# 模拟 LoRA 的思想

# 原始权重矩阵（大）
original_weight = torch.randn(1000, 1000)  # 1000×1000 = 1M 参数
print(f"原始矩阵参数: {original_weight.numel():,}")

# LoRA：拆成两个小矩阵 A × B
rank = 16
lora_A = torch.randn(1000, rank)   # 1000×16
lora_B = torch.randn(rank, 1000)   # 16×1000

lora_params = lora_A.numel() + lora_B.numel()
print(f"LoRA 参数:       {lora_params:,}")
print(f"参数减少:        {original_weight.numel() / lora_params:.0f} 倍")
print(f"\n原始: W × input")
print(f"LoRA: (W + A×B) × input  ← 只训练 A 和 B")

# LoRA 的更新量
lora_update = lora_A @ lora_B  # 16-rank 的低秩近似
print(f"\nLoRA 更新矩阵: {lora_update.shape}")
```

### 理解 LoRA 的三个超参数

```python
print("LoRA 核心参数：")
print("  r (rank):     一般 8~128，越大越强但越慢")
print("  alpha:        一般是 r 的 1~2 倍，控制更新的强度")
print("  target_modules: 通常是 attention 层的 Q、K、V、O")
print("")
print("实际缩放因子 = alpha / r")
print("  r=16, alpha=32 → 缩放 2.0 → 更新更强")
print("  r=64, alpha=16 → 缩放 0.25 → 更新更保守")
```

---

## ✏️ 复习（15分钟）

1. Full Fine-tuning 和 LoRA 的显存差距有多大？（前者可能要 8×A100，后者单卡 4090 就行）
2. QLoRA 比 LoRA 多做了什么？（把原模型量化为 4-bit）
3. LoRA 的 rank 选多少合适？（入门 16-32 足够）

---

## 📝 今日产出

- [ ] 理解了 Full FT / LoRA / QLoRA 的区别
- [ ] 跑了 LoRA 参数对比代码
- [ ] 懂了 rank、alpha 的含义
