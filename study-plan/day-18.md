# Day 18 — GPTQ / AWQ 与部署方案

> 🎯 目标：了解 GPU 端的量化方案，知道生产环境怎么部署

---

## 📖 阅读（30分钟）

### 1. GPTQ（15分钟）
- 📄 [4-bit Quantization using GPTQ - Maxime Labonne](https://mlabonne.github.io/blog/4bit_quantization/)
  - GPTQ = GPU 专用的量化方法
  - 比 GGUF 在 GPU 上跑得快
  - 需要校验数据集（calibration data）

### 2. AWQ（15分钟）
- 📄 [Understanding AWQ](https://medium.com/friendliai/understanding-activation-aware-weight-quantization-awq-boosting-inference-serving-efficiency-in-10bb0faf63a8)
  - AWQ = Activation-Aware Weight Quantization
  - 核心思想：重要的 channel 不量化或少量化

---

## 💻 实操（40分钟）

### 对比三种量化方案

```python
print("三大量化方案对比：\n")
print("┌──────────┬─────────┬──────────┬──────────┐")
print("│   方案    │  运行在  │   速度   │   质量   │")
print("├──────────┼─────────┼──────────┼──────────┤")
print("│  GGUF    │  CPU ✓  │  中等    │   ★★★★  │")
print("│  GPTQ    │  GPU    │   快     │   ★★★★  │")
print("│  AWQ     │  GPU    │  最快    │   ★★★★½ │")
print("│  bitsandbytes│ GPU  │  中等    │   ★★★★  │")
print("└──────────┴─────────┴──────────┴──────────┘")
print()
print("选择建议：")
print("  个人电脑/笔记本 → GGUF (Q4_K_M)")
print("  Colab 开发测试  → bitsandbytes (load_in_4bit)")
print("  生产部署(GPU)   → AWQ 或 GPTQ")
print("  生产部署(CPU)   → GGUF")
```

### 可选的 Colab 笔记本
- 📓 [AutoQuant - 一键量化成多种格式](https://colab.research.google.com/drive/1b6nqC7UZVt8bx4MksX7s656GXPM-eWw4?usp=sharing)
- 这个 notebook 可以把模型转成 GGUF、GPTQ、AWQ 等多种格式

---

## ✏️ 复习（20分钟）

1. GGUF、GPTQ、AWQ 各适用于什么场景？
2. 为什么 GPTQ 需要校准数据？（需要统计每一层的敏感度）
3. AWQ 的 "Activation-Aware" 是什么意思？（看激活值的分布来决定量化策略）
4. 你现在能在自己电脑上跑一个本地 LLM 了吗？（能！GGUF + llama.cpp）

---

## 📝 今日产出

- [ ] 理解了三种量化方案的区别
- [ ] 知道了生产部署怎么选
- [ ]（可选）跑了 AutoQuant notebook
