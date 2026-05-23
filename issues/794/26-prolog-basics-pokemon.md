---
layout: article
title: "用宝可梦教 Prolog 入门"
issue: 794
number: 26
category: code
original_url: "https://unplannedobsolescence.com/blog/prolog-basics-pokemon/"
hn_url: "https://news.ycombinator.com/item?id=48147091"
date: 2026-05-22
---

## 文章摘要

作者 Alex Petros 写了一篇**用宝可梦战斗机制教 Prolog 入门**的长文。选这个题材的理由很简单——宝可梦战斗本身就是一个**极其复杂的规则引擎**——属性相克、状态变化、招式优先级、特性、双打规则……这种结构化的、需要在大量事实和规则上做查询的场景，正是**逻辑编程的甜区**。

文章从**事实（facts）**开始建库——`pokemon(bulbasaur).`、`type(squirtle, water).`。然后展示**查询的双向性**——这是 Prolog 区别于 SQL 的核心魅力之一：

- `?- type(squirtle, Type).` → 反查这只宝可梦的属性
- `?- type(Pokemon, grass).` → 反查所有草属性宝可梦

同一条规则两个方向都能跑。接着引入**复合查询**——逗号是 AND、分号是 OR；以及**规则定义**——比如"造成伤害的招式 = 招式且伤害值 > 0"。

文章真正精彩的部分是**逐步精化（progressive refinement）**——用"找所有具有优先招式的宝可梦"做范例：先写一个最简单的版本；发现命中了双打专用招式，加排除；发现保护类招式不算"攻击"，加排除；发现 Prankster 特性会让所有变化招式变成优先招式，再加一层条件。**这种自然的、增量的、知识表达式的迭代过程，是过程式语言写起来痛苦但 Prolog 写起来非常优雅的**。

作者最后用一个直白的对比收尾——同一个查询"找特攻 > 120、会冻干、ice 属性的宝可梦"，**Prolog 版本的简洁程度和灵活度远高于 SQL JOIN 版本**——你不需要预先定义 JOIN 关系，引擎自己会合一。"**这是初学者用 spreadsheet 解决但严肃用户应该懂的工具。**"

## HN 评论精华

- **jodrellblank**（澄清新手最困惑的点）：很多初学者被 Prolog 答案后面的 "or false" 弄糊涂——作者自己也承认不完全明白。jodrellblank 引用 **The Power of Prolog** 解释——Prolog 的答案不是"打印到终端的文本"，而是**合法的 Prolog terms（数据/代码）**。所以分号 OR 既是输入也是输出语法。`(x ; y ; false)` 的意思是"这个查询可以由 x 或 y 回答，没有其他答案了"。"这让你可以做**元编程**——用 Lisp 那种 data-as-code 方式去 reasoning 和 rewrite 结果——但需要更高水平。"

- **triska**（Scryer Prolog 维护者）：补充了一个非常专业的视角——他和 Alex 在 Scryer Prolog 仓库里讨论过一些可能的改进，包括用**元编程自动生成更一般的关系**。他还推荐了 Alex 另一个项目——[factgraph.pl](https://github.com/alexpetros/factgraph.pl)——**用 Prolog 实现美国 IRS 的 Fact Graph**——一个真正的"Law as Code"应用。

- **deosjr**（最强实战案例）：他在玩 *Pokemon Emerald* 的难度 romhack **Run&Bun**——这游戏的核心玩法是"calcs"——battle 前用模拟器和 Excel 表格做详细计算。他直接 fork 了一个 JS 模拟器、套了 **Prolog wrapper**——让 solver 根据他手上的宝可梦队伍**自动给出每场战斗的解法**。他把每场战斗写成 test case，让 solver 找解、再对照实际打的结果。这是把 Prolog 真正用在**约束求解**而非演示的范例。

- **macintux**（历史段子）："**Joe Armstrong 用 Prolog 写出 Erlang 的最初实现这件事，对我来说一直令人震惊。**真希望当年问过他能不能要那份源码。"——这条评论让你瞬间意识到逻辑编程和并发编程的渊源比大多数人以为的近。

- **lagrange77**：抛出一段大学回忆——"**我们大学教 Prolog 和 Lisp 的那门课叫'工程师人工智能'**。"——这是 80-90 年代 AI 主流范式留下的痕迹，现在已基本绝迹。

- **Modified3019**（被说服的怀疑者）：开头读到时"不为所动"——但越往后越发现，**用宝可梦做例子恰好把 Prolog 解决问题的能力展示得很到位**——"我开始有点好奇想真去试试 Prolog 了。"

- **fl1pper**（外行的反向请求）："**我不熟悉宝可梦——谁能用 Prolog 给我解释一下宝可梦？**"——一条让评论区会心一笑的反向请求。

- **Almondsetat**（有趣的引申问题）："有没有公开的宝可梦比赛，要求选手用**特定算法范式**（逻辑编程、神经网络、线性规划）来对战？"——这其实是个相当不错的 AI/教学竞赛创意，目前似乎还没人办。

- **stellartux**（认真的勘误）：指出文中 `learns_priority/3` 规则有 typo——"`move_priority #> 0` 应该是 `P #> 0`"——典型 HN 式精确反馈。

- **admeliora01**（迁移到万智牌）：被这个 use case 启发，想给**万智牌（MTG）**实现类似的东西——他常用 scryfall，但对于像 Commander 这种卡池不断膨胀的永恒赛制，**用一个声明式规则的 CLI 来做 brewing** 显然比关键词搜索更合适。

- **Joker_vD**（SQL 派的较劲）：把作者举的 SQL 反例改写成了 **NATURAL JOIN** 版本——其实 SQL 写得好也能简洁很多。这条评论代表了一种诚实的反驳——文章里展示的 SQL "丑陋"很大程度是写法选择，不是 SQL 本身的局限。

- **cluckindan**（最妙的吐槽）：作者说"我将详细描述一款儿童电子游戏的机制"——cluckindan 接梗："**那 Prolog 应该是给成年人用的语言。**"
