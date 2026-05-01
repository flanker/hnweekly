---
layout: article
title: "为何我仍然选择 Lisp 和 Scheme 而非 Haskell"
issue: 792
number: 25
category: code
original_url: "https://jointhefreeworld.org/blog/articles/lisps/why-i-still-reach-for-scheme-instead-of-haskell/index.html"
hn_url: "https://news.ycombinator.com/item?id=47945707"
date: 2026-05-01
---

## 文章摘要

作者 Josep Bigorra 在尝试过 Go、Java、Scala、Kotlin 与多种 Lisp 方言后，撰文阐述自己为何在面对函数式编程任务时，仍然偏爱 Lisp / Scheme，而不是被学院派和评测人员推崇备至的 Haskell。文章并非全盘否定 Haskell，相反作者花了相当篇幅承认 Haskell 在类型系统上的非凡成就：代数数据类型、模式匹配、类型类、Monad 等抽象使其成为函数式语言谱系中的"柏拉图理想"，是计算机科学研究人员心目中近乎完美的工具。

然而作者认为这种数学上的纯洁性正是 Haskell 在工业实践中的双刃剑。他举了一个具体例子：自己想为一个书签管理工具写一个原型——任务本身在 Java 中只需"十分钟"，但用 Haskell 却消耗了"令人沮丧的一个多小时"。他被卡在 Cabal/Stack 依赖、HTTP 库的不同抽象层次、以及从纯函数 API 中调用副作用所必须串入的 IO Monad 之间。换句话说，Haskell 强迫程序员在动笔之前就把整个程序从最底层副作用边界规整出来，这与"先写 print 看看再说"的日常黑客式开发风格格格不入。

作者随后总结 Scheme/Lisp 的核心优势：

- **务实而灵活的极简主义**：S-expression 语法没有歧义，没有缩进规则的陷阱，让人几乎可以立刻"想到什么写什么"。
- **可即时插入副作用**：调试时直接 `(write ...)` 一行就能观察中间状态，无需重写函数签名。
- **原生宏系统**：在不依赖 Template Haskell 这种语言扩展的情况下就能写出强大的领域特定语言（DSL）。在 Lisp 中，"代码即数据"是默认现实，而非需要额外语法机制才能企及的特性。
- **REPL 与编辑器深度集成**：通过 Emacs + SLIME/Geiser，开发者获得"对话式开发流"——可以在程序运行时修改函数定义、重定义类型、注入新值，调试体验远超传统编译流程。

作者把两种语言的差异整理成对比表：Haskell 在哲学上追求数学纯净、调试需要重构以加入 print、元编程依赖语言扩展、原型阶段慢；Scheme 则以务实灵活著称，可即时观察、原生宏、迭代速度极快。最终结论并非贬低 Haskell——作者承认它在某些场景下仍是无可替代的工具——而是说明在自己日常构建小工具、写脚本和探索性编程的语境中，Scheme 是平衡"力量"与"开发者体验"的甜蜜点，而 Haskell 像"过分严肃的、太硬的雕塑"，对大多数场合来说有些过度工程化。

## HN 评论精华

- **crabbone** 对 Haskell 的可读性提出尖锐批评：他认为 Haskell 是"词语沙拉"——同一段代码常需反复阅读才能解析。给定一个表达式 `A B C`，程序员无法直接看出它是 `A(B, C)` 还是 `B(A, C)` 这种解析层面的歧义；加上 Haskell 古怪的缩进规则，使得快速原型开发的体验远不如 SML 或 Erlang 那样直接。

- **ngruhn** 则替 Haskell 辩护：他承认 Haskell 不是最易读的语言，但坚持认为它在语法和概念优雅性上仍属"顶级"，比 JavaScript 和 C++ 优雅得多，许多丑陋只是不熟悉造成的错觉。

- **floxy** 补充经验之谈：阅读速度与熟练度直接相关。一旦把 `map`、`filter`、`fold` 这些 Haskell 风格的高阶函数当作日常词汇，扫描代码的速度甚至比命令式语言更快。

- **moron4hire** 高度推崇 Racket（Scheme 方言）：他第一次接触 Racket 时"看到了这份职业本可以是什么样"，"代码即数据"的哲学从根本上改变了他对程序结构的认知，这是 Haskell 严格静态类型世界给不了的东西。

- **evdubs** 强调 Common Lisp 的工程实用性：SWANK/SLIME 让开发者能在程序运行时直接修改、重定义函数，"在执行过程中编辑程序本身"。这种交互式调试能力是绝大多数静态编译语言（包括 Haskell）所欠缺的，而它对长跑服务、游戏引擎、实验性算法尤为重要。

- 多位评论者达成的共识是：两种语言代表了不同的世界观——Haskell 追求"先证明后执行"的严谨性，而 Lisp/Scheme 追求"边写边发现"的可塑性。何者更优，最终取决于个人对语法清晰度、性能与开发流的权重。
