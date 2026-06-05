---
layout: article
title: "用了大约一个月 Clojure 之后的一些想法"
issue: 796
number: 20
category: code
original_url: "https://www.acdw.net/clojure/"
hn_url: "https://news.ycombinator.com/item?id=48375393"
date: 2026-06-05
---

## 文章摘要

作者之前用 GNU Make 加 shell 脚本搭建了自己的静态站点生成器，这次用 Clojure 把它重写了一遍，把这个项目当作学习这门语言的载体。一个月下来，作者分享了自己的体会。

在优点方面，作者首先称赞 Clojure 的"连贯性"。和由委员会设计、充满妥协的 Common Lisp 不同，Clojure 出自一人之手（Rich Hickey），因此非常一致：统一的函数（比如 `nth`）可以跨多种数据类型工作；相等性系统也更简洁，用 `=`、`==`、`identical?` 取代了 Lisp 中令人困惑的 `eq`、`eql`、`equal`、`equalp` 等一堆变体。其次是务实的设计。相比 Scheme 的极简主义哲学，Clojure 走的是"开箱即电池齐全"的路线——完善的标准库、运行在 JVM 之上、以及对业余爱好者和专业人士都有价值的生态系统支持。第三是数据结构。四种核心类型（list、vector、hash-map、set）都被当作一等公民，在使用体验上享受同等待遇，在 Lisp"万物皆 list"的哲学理想与实际需求之间取得了平衡。

在挑战方面，作者也直言不讳。一是语法复杂性：Clojure 其实背离了传统 Lisp 的简洁，引入了多种括号类型、符号的特殊含义、以及像 unquote `~` 这样陌生的语法；在嵌套闭包之间穿梭依然别扭。二是 Java 知识的鸿沟：尽管目前做基础的 Clojure 工作还用不到 Java，但因为语言依赖 JVM，作者感到有学习 Java 的压力。

结论是积极的：作者打算继续使用 Clojure，觉得它既有趣又实用，并计划通过做 Project Euler 题目来积累经验。

## HN 评论精华

**语言设计与可移植性**：Jeaye 点出了 Clojure 的一大优势——"一旦你学会了 Clojure 的语法和语义，你就不再被绑定在 JVM 上。"它存在多个实现（ClojureScript、babashka、jank），通过共享的 `.cljc` 文件可以实现跨平台代码复用。

**运行时 vs 语法之争**：一条很长的讨论线围绕"运行时是否比语法更重要"展开。pdimitar 主张"编程语言的语法几乎无关紧要"，相比之下运行时保证才是关键，他更青睐 Erlang/Elixir 的绿色线程和不可变性。但 weavejester 反驳说，语法从根本上塑造了系统架构和设计模式。

**实践优势**：HiPhish 谈到用宏来构建自定义工具（静态站点生成器、模板引擎）的那种满足感，但也承认其扩展性有局限。多位评论者称赞了 Clojure 的 REPL 驱动开发，以及配合 paredit/parinfer 的结构化编辑能力。

**JVM 能力**：有几条评论纠正了一些过时的假设。cogman10 解释说，"从 Java 24 开始，虚拟线程已经等价于" Go 的并发模型，"基本没有任何掣肘或坑"。

值得一提的贡献者包括文章作者 **acdw** 本人、jank 创造者 **Jeaye**（两人提供了权威的技术洞见）、讨论跨平台 Portal 实现的 **djblue**，以及分享"30 分钟内把 `.cljc` 移植到 Python"经历的 **EnigmaCurry**。
