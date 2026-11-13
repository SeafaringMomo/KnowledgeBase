---
title: 技术笔记
tags: [index, wiki]
updated: "2026-11-10"
---

# 技术笔记

本目录收录的技术笔记列表，点击链接可在 Obsidian 中打开对应笔记。完整导航请参见 [00-Index.md](00-Index.md)。

## 笔记列表

| 笔记 | 链接 | 简要概述 |
|---|---|---|
| Analytical_Databases | [[02-back-end/02-database/Analytical_Databases]] | Analytical databases are optimized for complex queries over large datasets, supporting OLAP workloads and data warehousing. |
| Distributed_Databases_JDBC | [[02-back-end/02-database/Distributed_Databases_JDBC]] | JDBC provides a standard API for Java applications to interact with relational and distributed databases. |
| Kubernetes_Docker | [[02-back-end/04-container/Kubernetes_Docker]] | Docker provides container runtime capabilities; Kubernetes orchestrates containers at scale. |
| Data_Transport_Protocols | [[09-storage/Data_Transport_Protocols]] | Overview of protocols used for moving data across networks, from low‑level transport to application‑level streaming. |
| K3S_Deployment | [[02-back-end/04-container/K3S_Deployment]] | K3s is a lightweight Kubernetes distribution for edge, IoT, and resource‑constrained environments. |
| Doris_Overview | [[02-back-end/02-database/Doris_Overview]] | Apache Doris is a high‑performance, real‑time analytical database for reporting and ad‑hoc queries. |
| Disk_Failure_RAID5_RAID6 | [[02-back-end/03-storage/Disk_Failure_RAID5_RAID6]] | Explanation of how RAID5 and RAID6 handle disk failures, their fault tolerance mechanisms, and performance implications. |
| KV_Database | [[02-back-end/02-database/KV_Database]] | KV databases store data as simple key‑value pairs, offering fast retrieval and various consistency models. |
| File_Formats_INP_SOI | [[11-others/File_Formats_INP_SOI]] | Overview of INP and SOI file formats, their use cases in data acquisition, and parsing strategies. |
| MLP_and_XGBoost | [[07-ml/02-algorithms/MLP_and_XGBoost]] | Comparison of Multilayer Perceptron (MLP) neural networks and XGBoost gradient‑boosted trees, highlighting strengths and integration patterns. |
| System_Operations | [[10-system/System_Operations]] | Overview of common system operation and maintenance tools such as RBAC, Gateway, Polphin Schedules, Hermes integration, TSI/TOM, and DCS/ICS. |
| System_Maintenance | [[10-system/System_Maintenance]] | Analysis of system usage components like RBAC, Gateway, Polphin Schedules, Hermes integration, TSI/TOM, and DCS/ICS. |
| Security_and_Compliance | [[08-security/02-compliance/Security_and_Compliance]] | Explanation of security measures and compliance standards, including SQL injection protection, JWT authentication, and industrial safety assessments. |
| Hardware_Server | [[06-hardware/01-server/Hardware_Server]] | Summary of hardware server components and tools such as ARM, DDR4 64GB, MobaXterm, and Hyper‑V virtualization. |
| Databases_and_BigData | [[04-databases/Databases_and_BigData]] | Analysis of databases and big‑data technologies like JDBC, analytical databases, OpenGauss, TDengine, Spark, Doris, and IoTDB. |
| Containers_and_Orchestration | [[05-devops/01-containers/Containers_and_Orchestration]] | Overview of container technologies and orchestration tools including Docker, K3s, Kubernetes, Kafka, and HTTP/Stream data flows. |
| Storage_and_Fault_Tolerance | [[09-storage/Storage_and_Fault_Tolerance]] | Discussion of storage systems, KV databases, disk failure handling, RAID5/RAID6, and REDs fault‑tolerance solutions. |
| ML_Models | [[07-ml/01-models/ML_Models]] | Summary of common machine‑learning models such as MLP, XGBoost, INP file format, and Signal of Interest (SOI). |
| Agent_Paradigm_Pipeline_vs_ModelNative | [[12-ai-agent/01-agent/Agent_Paradigm_Pipeline_vs_ModelNative]] | Compares pipeline-based agent orchestration with the model-native paradigm where planning and tool use are internalized through reinforcement learning. |
| ReAct_and_Plan_Execute | [[12-ai-agent/01-agent/ReAct_and_Plan_Execute]] | Contrasts interleaved reasoning–acting (ReAct) with upfront planning followed by execution, including selection criteria. |
| Function_Call_vs_Skill | [[12-ai-agent/01-agent/Function_Call_vs_Skill]] | Separates the function-calling protocol layer from the skill knowledge layer, and explains why they are frequently confused. |
| Loop_Engineering | [[12-ai-agent/01-agent/Loop_Engineering]] | Engineering the outer control loop of an agent: triggers, verification, state, budgets and stop conditions. |
| RAG_Pipeline_Basics | [[12-ai-agent/02-rag/RAG_Pipeline_Basics]] | The five-stage RAG pipeline, per-stage pitfalls, evaluation metrics and when RAG is the wrong tool. |
| TinyRAG_Minimal_Implementation | [[12-ai-agent/02-rag/TinyRAG_Minimal_Implementation]] | A minimal teaching implementation of RAG and the four practical insights it reveals. |
| LightRAG_Graph_Retrieval | [[12-ai-agent/02-rag/LightRAG_Graph_Retrieval]] | Lightweight graph-augmented retrieval with dual-level search, incremental indexing and comparison to GraphRAG. |
| GraphRAG_and_Neo4j | [[12-ai-agent/02-rag/GraphRAG_and_Neo4j]] | Why graphs enter retrieval, Neo4j's dual role as graph and vector store, and the real cost of graph indexing. |
| Ontology_Design | [[12-ai-agent/02-rag/Ontology_Design]] | Designing the knowledge-graph schema: entity and relation granularity, methods, common failures and acceptance criteria. |
| WeKnora_Knowledge_Platform | [[12-ai-agent/02-rag/WeKnora_Knowledge_Platform]] | Tencent's open-source knowledge platform with three modes (RAG, ReAct agent, self-maintaining wiki) and its trade-offs. |
| Firecrawl_Web_Collector | [[12-ai-agent/03-tools/Firecrawl_Web_Collector]] | LLM-friendly web collection, self-hosting cost, alternatives and when it is worth introducing. |

## Dataview 表格示例

下面是一个在 Obsidian 中使用 Dataview 插件的示例代码，列出本仓库所有含 `index` 标签的章节索引。

```dataview
TABLE title AS "标题", file.link AS "链接", tags AS "标签"
FROM ""
WHERE contains(tags, "index")
SORT file.name ASC
```

在上述查询中，将 `"index"` 替换为你想要过滤的标签即可。
