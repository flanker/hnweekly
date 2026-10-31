---
layout: article
title: "一篇声称推翻指数时间假设的预印本"
issue: 816
number: 47
category: learn
original_url: "https://github.com/openai/math/blob/main/preprints/The-Exponential-Time-Hypothesis-is-False-October-25-2026/paper.pdf"
hn_url: "https://news.ycombinator.com/item?id=50251661"
date: 2026-10-30
---

## 文章摘要

OpenAI 发布的这篇预印本提出一个确定性算法：对于固定的 k，精确计算 k-CNF 布尔公式的满足赋值数量，声称达到次指数时间并使用多项式空间。因为判断满足赋值数量是否大于零即可决定可满足性，作者据此宣称反驳了关于 3-SAT 最坏情况时间复杂度的指数时间假设 ETH。

论文也明确划出结论边界：次指数时间界不能经过任意多项式规模的归约原样保留，因此它不推出所有 NP 问题都有次指数算法，更不等于证明 P=NP。作者另讨论了图着色、完美匹配等计数问题的后果。

这里概述的是论文的主张与限定条件，不是对长篇证明的独立认证。理论渐近界与实际 SAT 求解器的表现还隔着常数、输入规模和实现成本，不能据标题推断现有计算任务立即大幅加速。

## HN 评论精华

- **首先分清 ETH 与 P=NP。** caughtinthought 用 3-SAT 的次指数求解解释 ETH；jibalt 指出这并没有解决 P 与 NP 的关系。
- **归约会改变规模。** cochne 强调，多项式归约可能扩大变量数，原问题的次指数界不一定因此成立，并引用论文自己的限定。
- **不能倒推已有条件下界。** strommec 问这是否让注意力计算获得次二次算法；rokob 回应，某假设推出下界，不表示假设被否定就自动产生更快算法。
- **理论和工程距离仍需研究。** criemen 关心实际 SAT 应用；infamouscow 提醒现实输入常与最坏情况不同。评论中的兴奋和质疑都不能替代证明审查。
