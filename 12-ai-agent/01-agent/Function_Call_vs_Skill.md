---
title: Function Call 与 Skill 的边界
tags: [技术, agent, 工具调用, skill]
updated: "2026-11-10"
---

# Function Call 与 Skill 的边界

## 简介
两者在表面上都是"模型决定去调用某个东西"，因此极易混淆。但它们处在**完全不同的抽象层级**：一个是调用协议，一个是能力封装。分不清会导致两个后果——把 Skill 当函数用（丢失流程知识），或把 Function Call 当 Skill 用（每次重复描述流程）。

## Function Call：协议层
模型把自然语言意图翻译成对预定义函数的**结构化参数**（通常是 JSON），由宿主程序执行，再把结果回填给模型。

```json
{"name": "search_web", "arguments": {"query": "DeepSeek Harness 插件机制"}}
```

**本质特征**
- **无状态**：一次调用一个动作，不记上下文；
- **单次**：不包含"什么时候该调、调几次、失败怎么办"；
- **有 schema 约束**：参数类型由开发者定义，模型只能填值，不能改结构；
- **谁定义边界**：开发者（函数签名）。

## Skill：知识层 / 流程层
把"一件事该怎么做"封装成可复用的单元：指令（instructions）+ 脚本 + 资源 + 参考文档，由 Agent 在需要时加载。

**本质特征**
- **有状态**：包含多步流程、判断分支、失败处理；
- **可组合**：一个 Skill 内部可以调用多次工具 / function call；
- **按需加载**：不占用常驻上下文，用到时才注入；
- **谁定义边界**：封装者（流程与最佳实践）。

## 对照

| 维度 | Function Call | Skill |
|---|---|---|
| 抽象层级 | 协议层（怎么调） | 知识层（该怎么做） |
| 状态 | 无状态 | 有状态、多步 |
| 粒度 | 单个动作 | 完整工作流 |
| 定义者 | 开发者（schema） | 封装者（指令+脚本） |
| 失败处理 | 由调用方决定 | 内建于流程 |
| 典型形态 | JSON schema / tool definition | SKILL.md + scripts/ + references/ |

## 一句话区分
- **Function Call 解决"能不能调"**——提供动作的接口；
- **Skill 解决"该不该调、按什么顺序调、失败了怎么办"**——提供做事的章法。

## 常见混淆点
1. **把 Skill 写成一个大函数**：所有逻辑塞进一个 function，模型无法在中间做判断，等于放弃了 Agent 的价值。
2. **把 Function Call 写成 Skill**：为每个简单动作都写一套流程文档，维护成本爆炸。
3. **忽视加载开销**：Skill 描述太长会挤占上下文；Function Call schema 太多会让模型选错工具。

## 实践建议
- 先用 Function Call 把**原子能力**定义清楚（少而精，5–15 个为宜）；
- 再把**高频组合**沉淀成 Skill（例如"发布一篇文档"= 查规范 + 写稿 + 配图 + 提交）；
- 判断标准：**需要模型做决策的，放 Skill；只需要执行的，放 Function**。

## 参考资料
- OpenAI Function Calling 文档
- Anthropic Agent Skills 规范
- 本仓库：[Agent 范式](Agent_Paradigm_Pipeline_vs_ModelNative.md)

*Last updated: 2026-11-10*
