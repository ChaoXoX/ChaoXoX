---
title: "提示工程 (Prompt Engineering)"
date: 2025-01-05
tags: [Prompt Engineering, LLM]
---

# 提示工程 (Prompt Engineering)

提示工程是驾驭 LLM 的核心技能，从简单的指令到复杂的思维链，决定了模型输出的上限。

## 1. 基础原则 (The Basics)
- **清晰明确**：避免模棱两可，直接下达指令。
- **角色扮演 (Persona)**：设定专家角色（如“你是一位资深 Python 架构师”）。
- **分隔符**：使用 `###`, `"""`, `---` 将指令与上下文/数据隔开。
- **输出格式**：明确要求 JSON, Markdown, CSV 等格式。

## 2. 进阶技巧
- **Few-Shot Prompting (少样本提示)**：
    - 提供 1-3 个高质量的 "输入-输出" 示例。
    - 显著提升模型对特定格式或风格的遵循度。
- **CoT (Chain of Thought, 思维链)**：
    - 加上 "Let's think step by step" (请一步步思考)。
    - 引导模型展示推理过程，大幅提升逻辑题、数学题准确率。
- **CoT-SC (Self-Consistency)**：
    - 生成多条思维链，取投票结果（Majority Vote）。

## 3. 结构化提示框架 (Structured Prompts)
推荐使用类似以下的模板结构：

```markdown
# Role
你是一位 [角色名称]，擅长 [技能1] 和 [技能2]。

# Context
任务背景是 [背景描述]。

# Task
请完成以下任务：
1. [步骤1]
2. [步骤2]

# Constraints
- 输出必须是 [格式]
- 长度限制在 [字数]
- 严禁 [禁止事项]

# Examples
输入: [示例输入]
输出: [示例输出]

# Input
[用户实际输入]
```

## 4. 提示词优化与自动生成
- **DSPy**：斯坦福推出的框架，将 Prompt 视为模型参数，通过编程方式自动优化 Prompt。
- **Prompt 变体测试**：在评估集上测试不同措辞的 Prompt，选择分数最高的。
- **LLM 生成 Prompt**：让 LLM 帮你写 Prompt（Meta-Prompting）。"请帮我写一个用于生成 SQL 的高质量 Prompt..."

## 5. 安全与防御
- **Prompt Injection (提示注入)**：用户输入试图覆盖系统指令。
    - 防御：将用户输入放在 Prompt 末尾，并明确标记；使用专门的 Guardrails 模型检测恶意输入。
- **Jailbreak (越狱)**：诱导模型输出违禁内容。
    - 防御：系统级 Prompt 强制安全审查；输出过滤。
