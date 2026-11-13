---
title: TinyRAG 最小实现
tags: [技术, rag, 实践]
updated: "2026-11-10"
---

# TinyRAG 最小实现

## 简介
TinyRAG 指一类**极简 RAG 教学实现**（通常 200–500 行）：去掉所有工程包装，只保留"文档→切块→向量→检索→生成"的最小闭环。它的价值不在性能，而在**让人亲手确认每一环到底发生了什么**——用过大框架的人往往说不清 chunk 是怎么切的、top-k 怎么选的、上下文怎么拼的。

## 最小实现只有四个函数
```python
def load_and_split(path, chunk_size=500, overlap=50):
    """读文件并按固定窗口切块（可换成按标题/段落切）"""

def embed(chunks):
    """调用 embedding API，返回向量列表"""

def retrieve(query, index, top_k=3):
    """query 向量化后做余弦相似度排序，返回 top_k 块"""

def answer(query, contexts):
    """把 contexts 拼进提示词，调用 LLM 生成答案"""
```

主流程：
```python
chunks = load_and_split("docs/manual.md")
index  = build_index(chunks, embed(chunks))
ctx    = retrieve("汽轮机轴承温度过高怎么处理", index, top_k=3)
print(answer("...", ctx))
```

## 手写一遍能获得的四个认知

1. **切块策略直接决定效果上限**。同样的 embedding 和模型，只改 chunk_size，答案质量可能天差地别——这在大框架里是"一个参数"，在这里是看得见的一刀。
2. **top-k 是一个权衡**。k 小则漏，k 大则噪声淹没关键信息，且成本线性上升。
3. **"检索对了但答错了" vs "检索错了所以答错了"** 是两类完全不同的故障，必须分开排查。
4. **向量检索不擅长精确匹配**。搜"TSI-2024 第 3.2 条"这类问题，纯向量经常失败——这就是生产环境必须上混合检索的原因。

## 从这个骨架往外扩的顺序
| 优先级 | 增强项 | 收益 |
|---|---|---|
| 1 | 混合检索（BM25 + 向量） | 精确匹配问题立刻改善 |
| 2 | Rerank 重排 | 上下文更干净，幻觉下降 |
| 3 | 父子分块 | 召回精度与上下文完整性兼得 |
| 4 | 引用与拒答 | 可追溯、不乱答 |
| 5 | 评估集（20–50 条问答对） | 后续所有改动才有可比性 |

> **建议**：先跑通再优化。没有评估集之前的任何"调优"都是盲改。

## 局限
- 单机内存索引，不适合大规模语料；
- 无增量更新、无权限控制、无多租户；
- 只适合作为**理解工具**和**小型私有知识库**，不适合直接上生产。

生产级能力可参考 [WeKnora](WeKnora_Knowledge_Platform.md) 或 [LightRAG](LightRAG_Graph_Retrieval.md)。

## 参考资料
- 本仓库：[RAG 基础链路](RAG_Pipeline_Basics.md)
- 各类 TinyRAG / nanoRAG 开源教学仓库

*Last updated: 2026-11-10*
