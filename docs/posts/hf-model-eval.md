---
title: "HuggingFace 模型评价"
date: 2025-01-05
tags: [HuggingFace, Evaluation]
---

# HuggingFace 模型评价

## 选型要点
- 任务匹配：指令/对话/分类/代码，先看是否有对应榜单。
- 许可证：商用/非商用，遵守模型与数据集的 license。
- 体量与推理成本：参数量、量化可行性、支持的硬件。

## 评价维度
- 通用指标：准确率、F1、BLEU/ROUGE、code pass@k。
- 中文表现：选择包含中文的基准（CEval、CMMLU、Gaokao-Bench）。
- 安全与偏见：red-teaming、敏感词过滤、输出长度限制。
- 资源：推理时长、显存占用、每 token 成本。

## 在 HF 上做快速对比
1. 选 3-5 个候选模型（参数量 + 训练数据透明度）。
2. 使用 `pipeline` 做小样本评测；保持相同提示、种子与温度。
3. 记录输出质量、延迟、成本，整理成表格。

## 自动化评估脚手架（示例）
```python
from transformers import pipeline

model_id = "Qwen/Qwen2-7B-Instruct"
pipe = pipeline("text-generation", model=model_id, device_map="auto")

prompts = ["给出一个安全的密码策略", "解释 RAG 如何减少幻觉"]
results = []
for p in prompts:
    out = pipe(p, max_new_tokens=256, temperature=0.2)[0]["generated_text"]
    results.append({"prompt": p, "output": out})

for row in results:
    print("---")
    print(row["prompt"])
    print(row["output"][:400])
```

## 发布时的指标看板
- 每日采样评测：10~50 条关键用例，低成本校验。
- 在线监控：延迟 P95、错误率、超时次数、成本上限。
- 反馈闭环：收集用户反馈进入新一轮微调或提示优化。
