---
layout: article
title: "软件工程法则（Laws of Software Engineering）：56 条塑造软件世界的原则与模式"
issue: 791
number: 6
category: favorites
original_url: "https://lawsofsoftwareengineering.com"
hn_url: "https://news.ycombinator.com/item?id=47847179"
date: 2026-04-27
---

## 文章摘要

`lawsofsoftwareengineering.com` 是一个相当克制但内容扎实的学习型网站，它把过去半个多世纪里影响软件系统、团队协作和工程决策的 **56 条经典法则与模式**汇集到一处，配上简洁的解释、典型案例与彼此之间的关联。除了网页，作者还把这些内容整理成了书和海报两种形式，方便团队在工位或讨论室里"挂一份"。

网站把这 56 条法则按照六大领域分类组织：

- **团队（Teams）**：Conway's Law（组织即架构）、Brooks's Law（往延期项目加人只会更晚）、Dunbar's Number（150 人社交极限）、Bus Factor（巴士因子）、Peter Principle（彼得原理）等。
- **规划（Planning）**：Premature Optimization（过早优化是万恶之源）、Parkinson's Law（工作总会膨胀填满给定时间）、Ninety-Ninety Rule（前 90% 用 90% 时间，剩下 10% 再用 90% 时间）、Hofstadter's Law（事情总是比你预期的要久，哪怕你考虑了这条定律本身）。
- **架构（Architecture）**：Hyrum's Law（任何 API 一旦有足够用户，所有可观察行为都会被某人依赖）、Gall's Law（复杂可用系统都源自简单可用系统）、CAP Theorem、Law of Leaky Abstractions（一切抽象都会泄漏）。
- **质量（Quality）**：Boy Scout Rule（让代码比你来时更好）、Murphy's Law、Testing Pyramid、Lehman's Laws（演化软件必然腐化）。
- **设计（Design）**：YAGNI、DRY、KISS、SOLID、Law of Demeter（最少知识原则）。
- **决策（Decisions）**：Dunning-Kruger Effect、Occam's Razor、Pareto Principle、Inversion 反向思维。
- **规模（Scale）**：Amdahl's Law（并行加速的天花板）、Metcalfe's Law（网络价值与节点数平方成正比）。

整个站点的可贵之处不在于"罗列了多少条"，而在于它把这些散落在不同书籍、论文、行业经验中的智慧聚到一起，并刻意地把"组织心理学"维度（Conway、Brooks、Dunbar、Peter）与"技术工程"维度（Hyrum、Gall、CAP、Lehman）平起平坐——这是对"软件工程不仅是写代码"的一次结构化重申。

## HN 评论精华

- **"过早优化"是被误解最深的一句话**：austin-cheney 直接喊话："这是整个编程界被误读最严重的一句话。Knuth 的本意是——别在没量过的地方花精力做优化，**除非那 ~10% 性能从一开始就关键的场景**。结果开发者把它简化成'永远别测、永远别优化'。"
- **架构层面的"过早优化"才是今天的主战场**：GuB-42 与 jandrewrogers 都强调，1974 年 Knuth 写下这句话时，"优化"指的是写汇编、抠循环；今天的软件早已是带宽受限而非计算受限，**性能问题大多是架构选择**，必须前置思考。把"过早优化是万恶之源"机械搬到 2026 年的微服务时代是个时代错位。
- **自文档代码的悖论**：iamflimflam1 指出，"代码应该自文档化"被很多人扭曲成"完全不写注释的借口"——和"过早优化"被滥用是一模一样的认知模式：把一句**有上下文的告诫**简化成**无条件的禁令**，结果意思走向反面。
- **资深工程师的工具盲区**：F1shy 提出一个相当尖锐的观察："不少高级工程师其实不会基本的调试和性能剖析（profiling）——这就像找一个号称很有经验的修车工，结果他不知道螺丝刀是什么。这种基本功的缺失让'去测一下再优化'变成了一句空话。"
- **架构层面的"想象中的规模"**：enraged_camel 列了一份让人会心一笑的清单——"为根本不存在的用户量做优化、在 PMF 都没找到时就上微服务、在没数据支撑的情况下加缓存层……这些'架构层面的过早优化'带来的长期代价远高于代码层面的微优化。"
- **SOLID 与"过早抽象"**：GuB-42 把矛头对准 SOLID："SOLID 鼓励的是'过早抽象'，导致接口爆炸、企业级过度工程。今天软件工程的真正麻烦不是过早优化，而是过早架构复杂度——而这玩意一旦写下来就更难拆掉。"
- **原则不是教条**：ghosty141 给出了最稳的总结："SOLID 也好，DRY 也好，是 guidelines 不是 dogma。它们的价值完全取决于上下文——核心组件值得精心设计，一次性脚本就不值得。最大的错误是不顾领域和阶段地把这些规则当成普适律。"
