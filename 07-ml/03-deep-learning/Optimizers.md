---
title: 优化器
tags: [技术, 深度学习, 优化器, 训练]
updated: "2026-11-10"
---

# 优化器

## 简介
优化器要解决的问题只有一个：**给定一个损失函数（高维地形），如何用最少的步数走到谷底**。

经典比喻是"盲人下山"：看不见全局，只能感知当前位置的坡度，于是朝着最陡的下坡方向迈一步，反复迭代。梯度给出**方向**，学习率给出**步长**——所有优化器的改进，都是在改这两件事怎么做。

$$
\theta_{t+1} = \theta_t - \eta \cdot g_t
$$

## 一、从全量到随机：三种基础形态

| 方法 | 每次用什么数据算梯度 | 优点 | 缺点 |
|---|---|---|---|
| **BGD（批量）** | 全部样本 | 方向精确、曲线平滑 | 每步代价极高，大数据集不可行 |
| **SGD（随机）** | 单个样本 | 每步极快、噪声可跳出局部最优 | 抖动剧烈、收敛慢 |
| **Mini-batch** | 一小批（32–1024） | 兼顾速度与稳定，能打满 GPU | batch 大小需调 |

> **实践默认选 mini-batch**。batch 太小噪声大、吞吐低；batch 太大泛化可能变差（"大 batch 陷阱"），且显存吃紧。

## 二、动量族：解决"抖动"与"沟谷"

**问题**：在狭长的山谷里，SGD 会在陡峭方向来回震荡，在平缓方向前进缓慢。

**Momentum**：引入物理惯性，把历史梯度做指数滑动平均。

$$
v_t = \beta v_{t-1} + g_t,\qquad \theta_{t+1} = \theta_t - \eta v_t
$$

- 效果：**同向梯度累积加速，反向梯度相互抵消**，震荡被抑制；
- $\beta$ 常取 0.9。

**NAG（Nesterov）**：先用当前动量"预判"下一步的位置，再在那个位置算梯度——相当于提前刹车，收敛更稳。

## 三、自适应族：解决"每个参数该走多大步"

**问题**：不同参数的梯度尺度差异巨大（embedding 稀疏、bias 敏感），统一学习率不合适。

**AdaGrad**：累积历史梯度平方，学习率按参数自适应衰减。

$$
\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{G_t + \epsilon}} g_t
$$

- 优点：稀疏特征（如 NLP 词表）能获得更大更新；
- **致命缺陷**：$G_t$ 单调累加，学习率最终衰减到零，训练提前停滞。

**RMSProp**：把"累加"改成"指数滑动平均"，只看近期梯度。

$$
v_t = \beta v_{t-1} + (1-\beta) g_t^2,\qquad \theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{v_t+\epsilon}} g_t
$$

- 修复了 AdaGrad 的停滞问题，至今仍是 RNN 时代的标准选择。

## 四、Adam 与 AdamW：集大成者

**Adam = Momentum + RMSProp**，同时估计一阶矩（均值）和二阶矩（方差），并做**偏差修正**（因为初始时刻估计偏向 0）。

$$
m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t,\quad
v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2
$$
$$
\hat m_t = \frac{m_t}{1-\beta_1^t},\quad \hat v_t = \frac{v_t}{1-\beta_2^t},\quad
\theta_{t+1} = \theta_t - \eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}
$$

默认超参：$\beta_1=0.9$、$\beta_2=0.999$、$\epsilon=10^{-8}$、$\eta=1e^{-3}$（**Adam 对学习率不敏感，但对 $\epsilon$ 和权重衰减敏感**）。

**AdamW**：修掉 Adam 的一个设计缺陷。原版 Adam 把权重衰减混进梯度里，导致它被自适应学习率**二次缩放**——梯度大的参数衰减反而小。AdamW 把衰减**解耦**出来直接作用于参数：

$$
\theta_{t+1} = \theta_t - \eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon} - \eta\lambda\theta_t
$$

> **这是当前微调与预训练的事实标准**（GPT/LLaMA/DeepSeek 全系都用 AdamW）。

## 五、选型对照

| 场景 | 推荐 | 理由 |
|---|---|---|
| 通用默认 | **AdamW** | 鲁棒、少调参 |
| 大模型预训练 | AdamW + 梯度裁剪 + warmup | 稳定性优先 |
| CV 经典 CNN | SGD + Momentum | 泛化往往更好（大 batch 时尤其明显） |
| 稀疏特征（推荐/NLP） | AdaGrad 或 Adam | 自适应步长有优势 |
| 显存受限 | Lion / 8-bit Adam | 优化器状态占显存可观（Adam 需存 2 份） |
| 需要极致泛化 | SGD 调好 lr + 长训练 | 经验上 test 表现常更稳 |

## 六、边界与常见错误

1. **Adam 不保证更低的训练损失**：它收敛快，但最终泛化可能不如调好的 SGD；论文里的"最优"依赖任务。
2. **忘记 warmup**：训练初期梯度噪声大，直接大学习率容易发散。大模型必须 warmup（几百到几千步）。
3. **不裁剪梯度**：梯度爆炸时 Adam 的自适应反而会掩盖问题，导致静默劣化。建议 `clip_grad_norm=1.0`。
4. **混淆 L2 正则与权重衰减**：在 Adam 下二者**不等价**，这就是 AdamW 存在的理由。
5. **优化器状态吃显存**：Adam 为每个参数存 m 和 v，等于模型显存 ×2（混合精度下更多）。7B 模型全量微调光优化器状态就几十 GB。
6. **学习率没调对就换优化器**：大多数"优化器不好用"其实是学习率错了。先调 lr，再换算法。

## 参考资料
- Ruder, *An overview of gradient descent optimization algorithms*, 2016
- Kingma & Ba, *Adam*, 2014；Loshchilov & Hutter, *Decoupled Weight Decay Regularization*（AdamW）, 2017
- 本仓库：[数学基础](../05-foundations/Math_for_ML.md) · [训练与推理优化](../14-mlops/Training_and_Inference_Optimization.md)

*Last updated: 2026-11-10*
