---
title: GraphRAG 与 Neo4j
tags: [技术, rag, 知识图谱, neo4j]
updated: "2026-11-10"
---

# GraphRAG 与 Neo4j

## 简介
向量检索擅长"找相似的片段"，但答不了两类问题：**需要多跳推理的**（A 影响 B，B 导致 C）和**需要全局归纳的**（这批文档整体讲了什么）。GraphRAG 用知识图谱补这块短板；Neo4j 则是承载这张图的常用数据库。

## 图为什么进入检索

| 问题类型 | 纯向量 RAG | 图增强 RAG |
|---|---|---|
| "轴承温度高的原因" | 能召回相关片段 | 能顺着"温度→润滑→油质→换油周期"多跳 |
| "所有故障的共性是什么" | 召回零散片段，无法归纳 | 社区摘要/主题节点可回答 |
| "这两台设备有什么关联" | 依赖文本共现 | 显式关系边直接回答 |

## Neo4j 的双重角色
1. **图存储**：节点（实体）+ 关系，用 Cypher 查询，天然适合多跳遍历；
2. **向量索引**：5.x 起内建向量索引，可在同一平台内做"向量相似 + 图遍历"的混合检索，减少系统拼装成本。

```cypher
// 典型的实体关系写入
MERGE (d:Device {name: "1号汽轮机"})
MERGE (f:Fault  {name: "轴承温度过高"})
MERGE (d)-[:HAS_FAULT {since: "2026-03"}]->(f)
```

```cypher
// 向量检索 + 图扩展：先找相似实体，再取邻居
MATCH (n) WHERE n.embedding IS NOT NULL
WITH n, gds.similarity.cosine(n.embedding, $qvec) AS score
ORDER BY score DESC LIMIT 5
MATCH (n)-[r]-(m) RETURN n, r, m LIMIT 50
```

## 索引成本：图增强的真正门槛
GraphRAG 类方案的代价不在查询，而在**建索引**：
- 每个块都要调用 LLM 抽实体与关系，token 成本高；
- 社区检测 + 层级摘要需要多轮 LLM 调用；
- 文档更新往往触发重建，增量能力弱。

> 第三方评测普遍显示：GraphRAG 的索引成本比 LightRAG 高 1–2 个数量级，耗时从分钟级变为小时级。**这意味着图增强不是"免费的准确性提升"，而是用建索引成本换回答质量。**

## 何时不值得
- 语料 < 几百页，且问题主要是"查事实" → 混合检索 + rerank 足够；
- 实体关系不稳定（每篇文档说法不一）→ 图会变成噪声源；
- 没有维护本体的能力 → 图谱半年后一定腐化；
- 更新极频繁（每日新增大量文档）→ 重建成本吃掉全部收益。

## 实践建议
1. **先做混合检索**（BM25 + 向量 + rerank），这是 baseline；
2. 用 20–50 条真实问题测一遍，**定位到底是不是"多跳/全局"问题在拖后腿**；
3. 确认是，再引入图；先小语料验证，再全量；
4. 图建好后必须有**实体消歧与定期清理**机制，否则会持续退化。

## 参考资料
- Microsoft GraphRAG 官方文档与论文
- Neo4j GraphRAG 实践文档 / Cypher 手册
- 本仓库：[LightRAG](LightRAG_Graph_Retrieval.md) · [Ontology 设计](Ontology_Design.md)

*Last updated: 2026-11-10*
