---
title: ReAct 与 Plan-and-Execute
tags: [技术, agent, 提示工程, 规划]
updated: "2026-11-10"
---

# ReAct 与 Plan-and-Execute

## 简介
两者都是**提示驱动的智能体控制模式**，区别在于"规划"与"执行"是分开还是交织。理解这一对概念，是读懂后续所有 Agent 框架的基础——它们几乎都是这两种模式的变体或组合。

## ReAct：思考与行动交织
论文 *ReAct: Synergizing Reasoning and Acting in Language Models*（2022）提出的范式。

```
Thought  →  我在想什么、下一步做什么
Action   →  调用工具（搜索、读文件、执行命令）
Observation → 工具返回的结果
   ↓ 回到 Thought，直到任务完成
```

**特点**
- 每一步都基于**最新的观察**重新决策，适应性强；
- 推理过程外显，可追溯、可调试；
- 缺点：步数多、token 消耗大；一旦中途跑偏容易在错误路径上反复迭代。

**适用**：环境反馈不确定、需要边走边看的任务（检索问答、调试、探索式研究）。

## Plan-and-Execute：先规划再执行
由 planner 先产出完整的分步计划，再由 executor 逐步执行（或由系统按计划调用工具）。

**特点**
- 全局视角清晰，步骤可预估、可审计、可并行；
- 缺点：**计划一旦遇到意外就失效**——现实偏离预设路径时缺乏重规划能力。

**适用**：步骤明确、可预先穷举的任务（批量处理、报表生成、固定流程自动化）。

## 对照

| 维度 | ReAct | Plan-and-Execute |
|---|---|---|
| 规划时机 | 每步即时决策 | 一次性前置规划 |
| 适应变化 | 强 | 弱（需显式重规划机制） |
| Token 成本 | 高（每步都带上下文） | 较低（计划一次） |
| 可审计性 | 中（过程长） | 高（计划即文档） |
| 典型失败 | 原地打转、越走越偏 | 计划僵化、一步错步步错 |

## 选型建议
- **不确定环境 + 需要探索** → ReAct；
- **流程稳定 + 要求可审计** → Plan-and-Execute；
- **两者结合**：先 Plan 出骨架，每个步骤内部用 ReAct 执行，遇到偏差触发重规划（这也是当前工程实践中最常见的形态）。

## 与模型原生范式的关系
ReAct 和 Plan-and-Execute 都属于**流水线范式**：循环由外部代码驱动，模型只负责生成下一步。当模型通过 RL 内化了工具调用策略后，这种"外部循环"会逐渐被模型自身的策略取代，但**可审计性的损失**需要工程手段补偿（见 [Agent 范式](Agent_Paradigm_Pipeline_vs_ModelNative.md)）。

## 参考资料
- Yao et al., *ReAct: Synergizing Reasoning and Acting in Language Models*, 2022
- LangChain / LangGraph 官方文档中的 Agent 模式章节

*Last updated: 2026-11-10*
