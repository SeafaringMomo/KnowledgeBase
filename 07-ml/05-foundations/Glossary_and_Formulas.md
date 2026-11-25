---
title: 常用公式与术语速查
tags: [技术, 速查, 公式, 术语]
updated: "2026-11-10"
---

# 常用公式与术语速查

## 简介
按"查得到"而非"读得完"组织的速查表。公式给直觉解释，术语给一句话定义 + 常见误用。

## 一、损失函数

| 公式 | 表达式 | 用在哪 | 直觉 |
|---|---|---|---|
| **MSE** | $\frac{1}{n}\sum(y-\hat y)^2$ | 回归 | 惩罚大误差；由高斯噪声假设推出 |
| **MAE** | $\frac{1}{n}\sum\lvert y-\hat y\rvert$ | 回归（有离群点） | 对异常值不敏感 |
| **交叉熵** | $-\sum y\log\hat y$ | 分类 | 惩罚"自信地答错" |
| **二元交叉熵** | $-[y\log p+(1-y)\log(1-p)]$ | 二分类 | 交叉熵的二类特例 |
| **KL 散度** | $\sum p\log\frac{p}{q}$ | VAE、蒸馏、分布对齐 | 衡量两个分布的差异（非对称） |
| **对比损失 InfoNCE** | $-\log\frac{e^{s^+/\tau}}{\sum e^{s/\tau}}$ | 检索 embedding、CLIP | 拉近正样本、推远负样本 |

## 二、常用函数

| 名称 | 公式 | 说明 |
|---|---|---|
| Sigmoid | $\frac{1}{1+e^{-x}}$ | 二分类输出；易梯度消失 |
| Softmax | $\frac{e^{x_i}}{\sum_j e^{x_j}}$ | 多分类概率；注意数值稳定（减最大值） |
| Tanh | $\frac{e^x-e^{-x}}{e^x+e^{-x}}$ | 零中心，仍会梯度消失 |
| ReLU | $\max(0,x)$ | 默认选择；注意"死亡 ReLU" |
| GELU | $x\Phi(x)$ | Transformer 常用，更平滑 |
| Softplus | $\log(1+e^x)$ | ReLU 的平滑版本 |

## 三、评估指标

| 指标 | 公式 | 适合场景 | 陷阱 |
|---|---|---|---|
| Accuracy | 正确数/总数 | 类别均衡 | 类别极不均衡时严重虚高 |
| Precision | TP/(TP+FP) | 误报代价高（垃圾邮件） | 忽略漏报 |
| Recall | TP/(TP+FN) | 漏报代价高（疾病筛查） | 忽略误报 |
| F1 | 2PR/(P+R) | 两者平衡 | 对极端不均衡仍不敏感 |
| ROC-AUC | ROC 曲线下面积 | 排序能力 | 极度不均衡时偏乐观 |
| PR-AUC | PR 曲线下面积 | 正样本极少 | 更真实但方差大 |
| BLEU / ROUGE | n-gram 重叠 | 翻译 / 摘要 | 与人类判断相关性有限 |

## 四、注意力与 Transformer

$$
\text{Attention}(Q,K,V)=\text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)V
$$

| 项 | 含义 |
|---|---|
| 复杂度 | $O(n^2 d)$，n 为序列长度 |
| $\sqrt{d_k}$ | 防止点积随维度增大导致 softmax 饱和 |
| 多头 | h 组独立投影，总计算量不变，表达力增强 |
| 位置编码 | 自注意力对顺序无感知，必须额外注入（RoPE 为主流） |

## 五、缩放定律与训练

| 概念 | 结论 |
|---|---|
| 训练算力近似 | $C \approx 6ND$（N 参数、D token） |
| Chinchilla 计算最优 | 参数量与 token 数同比扩展，约 **20 token/参数** |
| 注意力显存 | 与 $n^2$ 成正比（除非用稀疏/线性注意力或 FlashAttention） |

## 六、常用缩写

| 缩写 | 全称 | 中文 |
|---|---|---|
| CNN | Convolutional Neural Network | 卷积神经网络 |
| RNN | Recurrent Neural Network | 循环神经网络 |
| GNN | Graph Neural Network | 图神经网络 |
| RL | Reinforcement Learning | 强化学习 |
| PPO | Proximal Policy Optimization | 近端策略优化 |
| DPO | Direct Preference Optimization | 直接偏好优化 |
| RLHF | RL from Human Feedback | 人类反馈强化学习 |
| RLVR | RL from Verifiable Rewards | 可验证奖励强化学习 |
| LLM | Large Language Model | 大语言模型 |
| ASR | Automatic Speech Recognition | 语音识别 |
| TTS | Text To Speech | 语音合成 |
| OCR | Optical Character Recognition | 光学字符识别 |
| MoE | Mixture of Experts | 混合专家 |
| KV Cache | Key-Value Cache | 键值缓存 |
| SFT | Supervised Fine-Tuning | 监督微调 |
| RAG | Retrieval-Augmented Generation | 检索增强生成 |

## 七、易混淆术语

| 容易混淆 | 区别 |
|---|---|
| 参数 vs 超参数 | 参数由训练得到；超参数由人设定（学习率、层数） |
| Batch size vs 学习率 | 通常同向调整；batch 翻倍时 lr 可适度放大 |
| Epoch vs Iteration | 一个 epoch = 遍历全部数据一次；一个 iteration = 一次参数更新 |
| 梯度消失 vs 梯度爆炸 | 前者深层学不动，后者数值溢出；对策相反 |
| 微调 vs 提示 | 前者改参数，后者改上下文；成本差 2–3 个数量级 |
| 幻觉 vs 错误 | 幻觉是"自信地编造"，错误是"答错"；缓解手段不同 |

*Last updated: 2026-11-10*
