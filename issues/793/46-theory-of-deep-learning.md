---
layout: article
title: "一个关于深度学习的理论"
issue: 793
number: 46
category: learn
original_url: "https://elonlit.com/scrivings/a-theory-of-deep-learning/"
hn_url: "https://news.ycombinator.com/item?id=48027455"
date: 2026-05-08
---

## 文章摘要

Elon Litman 这篇长文试图为深度学习提出一个**统一的理论框架**——放弃以"参数空间"为视角的传统分析路径，转而把神经网络视作"输出空间"中的动力系统来研究。框架的核心对象是 **empirical Neural Tangent Kernel（eNTK，经验神经切核）**：度量在某个训练点上做一次梯度更新，会如何影响其它任何点的预测。

理论把训练动力学拆成两个互补空间：

- **Signal Channel（信号通道）**：训练过程中真正消耗了 loss、并且会影响测试预测的方向。
- **Reservoir（水库）**：训练没碰到、在测试时不可见的方向。

作者用一句话总结这个分解的力量："**网络在 reservoir 里记忆下来的任何东西，在测试时都是不可见的。**" 这一句话里他要解释的现代深度学习四大谜团全部串了起来：

1. **Benign Overfitting（良性过拟合）**：训练时被记忆下来的噪声坐在 reservoir 里，对测试集隐形，所以即使过拟合训练集也不会破坏泛化。
2. **Double Descent（双下降）**：随着模型容量增加，噪声在 channel 和 reservoir 之间迁移，造成测试损失的双谷曲线。
3. **Implicit Bias（隐式偏置）**：梯度流自然偏向"高 mobility"模式——它们总是先被学到，所以模型最终倾向于稀疏、 parsimonious 的解。
4. **Grokking**：信号最初被埋在 reservoir 里，随着 kernel 演化逐步迁移到 channel，于是出现"先记忆、后突然泛化"的曲线。

应用层面，作者基于这套理论提出了一种 Adam 的改进——**基于 SNR 的门控更新**，在玩具模型上获得了 5× 的 grokking 加速以及在 DPO fine-tuning 上的可观提升。最大胆的承诺是：未来或许可以**直接对 population risk 做训练**，甚至**解析地预测网络最终状态**，而不必跑完一整轮梯度下降。

## HN 评论精华

1. **refulgentis**：核心批评。文章描述了"网络如何把可记忆的噪声和可泛化的模式分离开"，但**没有解释 SGD 凭什么能做到这种分离**。这种推理"接近循环论证"——把现象重新命名再说"我解释了它"。"声称解决了 grokking、double descent、benign overfitting 等六大问题，在没有充分实证验证之前就这么说，调子太大了。"

2. **r0ze-at-hn**：相对正面。指出文章里很多看似新词其实是把已经成熟的领域 vocabulary 搬过来——control theory 里的 observability gramian、large deviation theory（用来预测 grokking 时间）、量子测量理论等等。"作者更像在做综合而不是发明。" 提到的那个 Adam 改进确实有论文给的实证：5× grokking 加速、DPO 微调改进——这是值得拎出来单独看的部分。

3. **yorwba**：实用性 push back。"如果在真实训练里典型 batch 的梯度信噪比已经很高，那 SNR-门控这个 optimizer 改动的影响就会很有限。" 实验主要在 toy problem 和小模型上跑，外推到标准深度学习实践有疑问。

4. **整条线**还有几条对**写作语气**的反应。多人吐槽 Borges 引言、宏大的"unification" 修辞让整篇文章在没有同行评审之前就给人 over-promising 的感觉。一位评论者直接把它和"AI 生成的学术写作"做了类比——读起来很有说服力，但结构性地缺少"为什么"这一层。

5. **整体观察**：HN 对这种"统一深度学习理论"类文章的态度长期一致——欢迎更好的工具与术语（如 NTK 在过去几年确实成了标准词），但本能地警惕"一个框架解释一切"的姿态。这次也不例外：评论里既有人认真指出 mobility / observability 这条思路与控制论的连接很值得发展，也有人反复提醒"在小模型上的 5× 改进还不能下 sweeping claim"。

6. **更具体的几个技术点被反复提到**：(a) eNTK 在大模型训练中演化得非常快，"channel vs reservoir"是不是稳定的分解需要更多实证；(b) 论文里宣称能"解析预测最终状态"——如果属实就是本文最大的实用贡献，但目前给出的例子全部是 toy 设置；(c) 这种 framework 最大的危险是"事后解释一切现象但事前预测能力很弱"，HN 评论里多次出现"please make a forward-looking prediction"。
