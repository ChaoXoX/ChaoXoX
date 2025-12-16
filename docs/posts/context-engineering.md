---
title: "上下文工程 (Context Engineering)"
date: 2025-01-05
tags: [Context, LLM, Memory]
---

# 上下文工程 (Context Engineering)

随着模型上下文窗口 (Context Window) 的增大（128k, 1M+），如何高效利用上下文成为了新的挑战。不仅仅是塞进去更多数据，而是要让模型“注意”到关键信息。

## 1. 上下文窗口的迷思
- **Lost in the Middle (中间迷失)**：模型往往对 Context 开头和结尾的信息记忆最深，中间部分容易被忽略。
- **策略**：将最重要的指令放在开头和结尾；关键参考文档放在两端。

## 2. 上下文压缩与管理
- **摘要 (Summarization)**：对长对话历史进行滚动摘要，保留核心意图，丢弃寒暄。
- **选择性保留**：只保留与当前 Query 语义相关的历史对话（通过向量检索历史记录）。
- **KV Cache 优化**：在推理侧，利用 PagedAttention (vLLM) 等技术优化显存，但逻辑上的 Context 管理依然重要。

## 3. 动态上下文构建 (Dynamic Context Construction)
在 RAG 或 Agent 流程中，Context 是动态组装的：

1. **System Prompt**: 固定指令与安全边界。
2. **Few-Shot Examples**: 动态检索最相关的示例（Dynamic Few-Shot）。
3. **Retrieved Knowledge**: 检索到的文档块。
4. **Conversation History**: 最近 k 轮对话。
5. **User Query**: 当前问题。

## 4. 长上下文 vs RAG
- **长上下文 (Long Context)**：
    - 优势：全局理解能力强，能处理跨文档的复杂归纳。
    - 劣势：推理成本高（二次方复杂度或线性增长但基数大），延迟高。
- **RAG**：
    - 优势：成本低，速度快，易于更新知识。
    - 劣势：碎片化，可能丢失全局视角。
- **混合方案**：先用 RAG 粗筛出较多文档（如 50k tokens），再利用长上下文模型进行精读和回答。

## 5. 调试与监控
- **Token 计数**：始终监控输入 Token 数量，防止溢出或产生意外高额账单。
- **Attention Visualization**：在研究阶段，可视化模型的 Attention 权重，看它到底在关注 Context 的哪一部分。
