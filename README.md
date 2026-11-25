---
title: 技术笔记
tags: [index, wiki]
updated: "2026-11-24"
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
| Autoencoder | [[07-ml/03-deep-learning/Autoencoder]] | Unsupervised representation learning through bottleneck reconstruction: variants (DAE, VAE, VQ-VAE), limits, and industrial anomaly detection. |
| Encoder_Decoder_Architecture | [[07-ml/03-deep-learning/Encoder_Decoder_Architecture]] | The split between understanding (encoder) and generation (decoder), three model shapes, and how attention broke the fixed-vector bottleneck. |
| Attention_Mechanism | [[07-ml/03-deep-learning/Attention_Mechanism]] | Query-key-value retrieval, scaled dot-product attention, multi-head design, positional encoding and the O(n²) cost wall. |
| Transformer_Architecture | [[07-ml/03-deep-learning/Transformer_Architecture]] | The 2017 architecture end to end: components, why it replaced RNNs, its three derivative forms and modern improvements. |
| LLM_Fundamentals | [[07-ml/04-llm/LLM_Fundamentals]] | Pretraining/SFT/alignment pipeline, scaling laws including Chinchilla, MoE and long-context trends, and the hard limits of LLMs. |
| AI_Overview | [[07-ml/Overview]] | AI evolution map, capability layers, and three learning routes organised by goal rather than difficulty. |
| Math_for_ML | [[07-ml/05-foundations/Math_for_ML]] | The math that actually matters: linear algebra as data shape, probability as the origin of loss functions, bias-variance diagnosis. |
| Glossary_and_Formulas | [[07-ml/05-foundations/Glossary_and_Formulas]] | Lookup tables for losses, metrics, attention formulas, abbreviations and commonly confused terms. |
| Optimizers | [[07-ml/03-deep-learning/Optimizers]] | From batch GD to AdamW: momentum, adaptive learning rates, and practical selection rules. |
| Classic_Models_Cheatsheet | [[07-ml/02-algorithms/Classic_Models_Cheatsheet]] | Model selection by task, data volume and deployment target. |
| NLP_Overview | [[07-ml/06-nlp/NLP_Overview]] | Tokenization to pretraining, task-model matching, and Chinese-specific cost and evaluation constraints. |
| Computer_Vision_Overview | [[07-ml/07-computer-vision/Computer_Vision_Overview]] | Backbone evolution, detection and segmentation schools, and industrial data/deployment realities. |
| Speech_Processing_Overview | [[07-ml/08-speech/Speech_Processing_Overview]] | Waveform to Mel features, ASR/TTS generations, speaker verification, and the noise and latency walls. |
| Multimodal_Learning | [[07-ml/09-multimodal/Multimodal_Learning]] | Three fusion levels, CLIP contrastive training, and how to verify a model actually looks at images. |
| RL_Fundamentals | [[07-ml/10-reinforcement-learning/RL_Fundamentals]] | MDP, value-based vs policy-based methods, PPO, and the RLHF/RLVR link to LLMs. |
| GNN_Overview | [[07-ml/11-graph-neural-network/GNN_Overview]] | Message passing, GCN/GAT/GraphSAGE/GIN, over-smoothing and heterophily. |
| Recommender_Systems | [[07-ml/12-recommender-system/Recommender_Systems]] | The recall-ranking funnel, DIN's dynamic-interest insight, and three industrial must-solves. |
| Data_Engineering | [[07-ml/13-data-engineering/Data_Engineering]] | Collection distribution, cleaning, labelling economics, versioning and augmentation limits. |
| Feature_Engineering | [[07-ml/13-data-engineering/Feature_Engineering]] | Preprocessing, feature construction, selection methods, and the four shapes of feature leakage. |
| MLOps_and_Deployment | [[07-ml/14-mlops/MLOps_and_Deployment]] | Serving patterns, inference engines, drift monitoring and silent-failure detection. |
| Training_and_Inference_Optimization | [[07-ml/14-mlops/Training_and_Inference_Optimization]] | Parallelism and ZeRO, mixed precision, LoRA/QLoRA, KV cache and quantization trade-offs. |
| Model_Evaluation_and_Experiments | [[07-ml/15-evaluation/Model_Evaluation_and_Experiments]] | Splitting rules, metric selection, ablation, significance testing, and the offline-online gap. |
| AI_Safety_and_Interpretability | [[07-ml/16-safety/AI_Safety_and_Interpretability]] | Attack surface by stage, prompt injection, data poisoning, privacy and fairness. |
| Project_Cases | [[07-ml/17-practice/Project_Cases]] | Eight project archetypes in problem-solution-pitfall-acceptance form, plus a launch checklist. |
| Research_Roadmap | [[07-ml/17-practice/Research_Roadmap]] | Paradigm-shifting papers by era, the three-pass reading method, and a staged learning roadmap. |
| HTAP_and_Workload_Models | [[02-back-end/02-database/HTAP_and_Workload_Models]] | OLTP/OLAP/HTAP workload comparison, two HTAP implementation routes, their costs, and why HTTP is unrelated. |
| CDC_Incremental_Data_Capture | [[02-back-end/02-database/CDC_Incremental_Data_Capture]] | Log-based change capture versus polling, six failure modes, and where CDC belongs in the pipeline. |
| Database_Access_Control | [[02-back-end/02-database/Database_Access_Control]] | Table/column/row permission granularity, four ways to implement row-level security, and the bypass holes. |
| OceanBase_Overview | [[02-back-end/02-database/OceanBase_Overview]] | OceanBase architecture, community versus enterprise edition gaps, and when a distributed database is overkill. |

## Dataview 表格示例

下面是一个在 Obsidian 中使用 Dataview 插件的示例代码，列出本仓库所有含 `index` 标签的章节索引。

```dataview
TABLE title AS "标题", file.link AS "链接", tags AS "标签"
FROM ""
WHERE contains(tags, "index")
SORT file.name ASC
```

在上述查询中，将 `"index"` 替换为你想要过滤的标签即可。
