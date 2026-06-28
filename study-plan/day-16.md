# Day 16 — 量化基础

> 🎯 目标：理解模型量化的原理，知道怎么把大模型变小

---

## 📖 阅读（35分钟）

### 1. 课程文章（必读，25分钟）
- 📄 [Introduction to Quantization - Maxime Labonne](https://mlabonne.github.io/blog/posts/Introduction_to_Weight_Quantization.html)
  - 课程作者写的，非常透彻
  - 理解 FP32 → FP16 → INT8 → INT4 的含义
  - 核心概念：
    - **absmax quantization**: 用绝对最大值缩放
    - **zero-point quantization**: 用零点偏移来保留分布
    - **LLM.int8()**: 把异常值单独处理

### 2. 直观理解（10分钟）

```
FP32 (32位浮点):  ████████████████████████████████  4 bytes/参数
FP16 (16位浮点):  ████████████████                  2 bytes/参数
INT8 (8位整数):   ████████                          1 byte/参数
INT4 (4位整数):   ████                              0.5 byte/参数

Llama-7B:
  FP32: 28 GB  → 需要 2 张 A100
  FP16: 14 GB  → 需要 1 张 A100
  INT4: 3.5 GB → 一张 4090 就能跑！
```

---

## 💻 实操（40分钟）

### 亲手量化一个模型

打开 Colab（需要 GPU）：

```python
!pip install transformers torch bitsandbytes

import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig

model_name = "Qwen/Qwen2.5-0.5B"

# 先看在 FP16 下的显存
print("=" * 40)
print("加载 FP16 模型...")
model_fp16 = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.float16,
    device_map="auto"
)
fp16_size = model_fp16.get_memory_footprint() / 1e9
print(f"FP16 显存占用: {fp16_size:.2f} GB")

# 清理
del model_fp16
torch.cuda.empty_cache()

# 再用 4-bit 量化加载
print("\n" + "=" * 40)
print("加载 4-bit 量化模型...")
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.float16,
    bnb_4bit_quant_type="nf4",
)
model_4bit = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=bnb_config,
    device_map="auto"
)
bit4_size = model_4bit.get_memory_footprint() / 1e9
print(f"4-bit 显存占用: {bit4_size:.2f} GB")
print(f"节省: {(1 - bit4_size/fp16_size)*100:.0f}% 显存！")
```

---

## ✏️ 复习（15分钟）

1. INT4 量化后模型变小了多少？（FP32 → INT4 = 大约 75% 显存减少）
2. LLM.int8() 的核心 trick 是什么？（把 outlier 特征单独用 FP16 算）
3. 量化会降低模型效果吗？（INT8 几乎无损，INT4 略有损失）

---

## 📝 今日产出

- [ ] 读完量化入门文章
- [ ] 跑了量化对比代码
- [ ] 看到显存节省的数字
