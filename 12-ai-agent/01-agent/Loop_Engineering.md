---
title: Loop Engineering 循环工程
tags: [技术, agent, 工程化, 自动化]
updated: "2026-11-10"
---

# Loop Engineering 循环工程

## 简介
> **Loop Engineering 是设计、实现和调优 AI Agent 外层控制回路的工程实践**——关注触发器、目标规格、上下文、工具权限、验证信号、状态持久化、停止条件和人工关口，而不是"这一句 prompt 写得好不好"。

一句话概括它与相邻概念的分工：**提示词工程管"怎么说"，上下文工程管"看什么"，Harness 工程管"在什么环境里跑"，Loop 工程管"这一轮之后怎么办"。**

## 四层演进
```
Prompt Engineering (2022–2024)   写好一句话
        ↓
Context Engineering (2025)        每轮给模型看什么
        ↓
Harness Engineering (2026 初)     外围：工具、权限、状态、可观测
        ↓
Loop Engineering (2026)           最外层：谁触发、何时继续、何时停、怎么验收
```
每一层包裹前一层。Loop 不替代前三者，而是把它们组织成一个能自我修正、但不会失控的系统。

## 内循环 vs 外循环

| | 内循环 | 外循环 |
|---|---|---|
| 是什么 | Agent 每一轮的"想→做→看结果" | 系统如何反复驱动这个内循环 |
| 谁在跑 | 模型自己（ReAct 已说清楚） | 你设计的调度与验收机制 |
| 例子 | 读错误→改代码→跑测试 | 每天 9 点扫 CI 失败，派给 Agent 修，修完提 PR 等人审 |

> **先决条件**：内循环跑不稳，加外循环毫无意义。外循环提高的是**执行次数**，不是**单次正确率**。

## 六个设计原语

| 原语 | 要回答的问题 | 常见实现 |
|---|---|---|
| 触发器 | 什么情况下启动？ | cron、PR 事件、CI 失败、人工 `/goal` |
| 目标规格 | 什么叫完成？ | 测试通过、lint clean、验收清单、rubric |
| 上下文 | 每轮该读什么？ | AGENTS.md、设计文档、issue、进度文件 |
| 执行边界 | Agent 能碰什么？ | 工具白名单、沙箱、git worktree、只读子 Agent |
| 验证器 | 谁证明结果可用？ | 单测、E2E、类型检查、LLM judge、人工抽检 |
| 状态与停止 | 下一轮怎么接上？何时停？ | progress.md、任务板、预算、轮数上限、无进展检测 |

## 最小可用循环
```python
while not done and within_budget:
    context     = build_context(goal, state, latest_observation)
    action      = agent.decide(context)
    observation = execute(action)                  # 受权限与沙箱约束
    state       = update_state(state, action, observation)
    done        = verify(goal, state, observation)  # 独立验证器
```

## 停止策略（最容易被忽略）
| 条件 | 作用 |
|---|---|
| success | 验证通过 |
| max_steps / max_time | 防无限迭代 |
| max_cost | 防 token 失控 |
| **no_progress** | 连续 N 轮无改善即停（最有价值的一条） |
| repeated_failure | 同类错误反复出现 → 升级给人 |
| risk_trigger | 部署、删库、花钱等高风险动作必须人工确认 |

> 一个不会停的 Agent 不叫自主，叫失控。**绝大多数失控不是"停不下来"，而是"在原地空转还自以为在进步"。**

## 常见误区
1. **把 Loop 等同于定时任务**：cron 只是入口，缺少验收、状态、预算的控制链路。
2. **信任模型自报完成**：验证器必须独立于执行者（测试、规则、另一模型、人工）。
3. **没有外部状态**：进度只存在对话历史里，重启即失忆。
4. **过早并行**：单次任务都跑不稳就扩大并发，只会扩大错误规模。

## 落地顺序
1. 把完成条件机器化（"改好代码" → "目标测试与 lint 全通过"）；
2. 搭好单次执行的 Harness（规则、权限、验证命令）；
3. 有人观察时跑内循环，收集失败类型；
4. 增加低风险外部触发（只读、易回滚）；
5. 最后才扩大并行度与写权限。

## 参考资料
- Addy Osmani 等关于 Loop Engineering 的公开讨论（2026）
- 本仓库：[Agent 范式](Agent_Paradigm_Pipeline_vs_ModelNative.md) · [Function Call 与 Skill](Function_Call_vs_Skill.md)

*Last updated: 2026-11-10*
