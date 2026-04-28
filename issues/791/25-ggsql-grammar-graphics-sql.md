---
layout: article
title: "ggsql：面向 SQL 的图形语法"
issue: 791
number: 25
category: data
original_url: "https://opensource.posit.co/blog/2026-04-20_ggsql_alpha_release/"
hn_url: "https://news.ycombinator.com/item?id=47833558"
date: 2026-04-27
---

## 文章摘要

Posit（前 RStudio）正式发布了 ggsql 的 alpha 版本——一个把"图形语法"（Grammar of Graphics）框架带入 SQL 世界的可视化工具。它让用户可以直接在 SQL 查询中创建可视化，并能在 Quarto、Jupyter Notebook、Positron 和 VS Code 等多种环境下运行。

ggsql 采用类 SQL 的声明式子句来组织绘图。比如最基础的散点图只需要一行：`VISUALIZE bill_len AS x, bill_dep AS y FROM ggsql:penguins DRAW point`。它继承了 ggplot2 经典的"图层组合"思想：用户先定义数据到视觉属性的映射（mappings），再叠加绘图图层、坐标轴尺度（scales）、标签（labels）等模块，逐步构建完整图表。此外它新增了一个 `PLACE` 子句，专门用来放置参考线、文本注释这类不依赖数据的图层，把数据驱动层与字面量注释层清晰分开。

Posit 给出了创造 ggsql 的五大动机：第一，服务那些"以 SQL 为主"、不熟悉 R 或 Python 的数据分析师，让他们也能享受现代可视化；第二，SQL 的声明式与可组合特性天然契合图形语法理念；第三，剥离 R/Python 运行时依赖，简化嵌入和部署；第四，SQL 是当下 LLM 最熟悉的语言之一，便于"自然语言到可视化"的工作流；第五，借助过去 18 年开发 ggplot2 积累的经验，在新的平台上做设计上的革新。

技术上，ggsql 的严格语法结构带来了显著的后端执行优势：例如要把 100 亿条交易数据画成柱状图，引擎只需取每个柱的计数值，无需把整张表拉到客户端。后续路线图包含基于 Rust 的写入器、主题系统、交互能力以及空间数据支持。它定位为 ggplot2 的补充而非替代。

## HN 评论精华

- **anentropic** 指出文章和文档都没把"它如何与 SQL 数据库交互"讲清楚，他从 FAQ 推断 ggsql 应该是一个 SQL 风格的可视化 DSL，"部分语法直接传给数据库"，但具体集成路径仍然模糊，建议官网把这一点讲透。

- **getnormality** 一开始也不理解 ggsql 解决什么问题：既然 R 已经有 dbplyr 能把 R 翻译成 SQL，为什么还要发明一种新 DSL？通读文档后他得出结论：ggsql 是一个独立的可视化应用，目前以 DuckDB/SQLite 为后端、用 Vega-Lite 作渲染器，目标用户是不会 Python/R 的 SQL 专家。

- **lmeyerov** 表示在自家 GFQL（基于 OpenCypher 的图查询语言）上得出了类似结论：要让可视化栈对 LLM 友好、并避免依赖代码沙箱，用声明式 DSL 是非常自然的选择，尤其在 GPU 视觉分析管线中收益巨大。

- **JHonaker** 是 ggplot2 长期用户，他赞同图形语法和 SQL 在概念上对齐，但质疑相对 ggplot2 究竟收益在哪——他在 R 里碰到的真正痛点是做"非标准"图形时 ggplot 扩展机制太繁琐，希望 ggsql 能从设计上规避这个问题，否则只是多了一个 DSL 而已。

- **tomjakubowski** 兴奋地表示自己一直想要"在 SQL REPL 里直接出图"的工具，原本计划用 Perspective + DuckDB 自己造一个，ggsql 可能正是他需要的方向。

- **efromvt** 称赞图层组合（layering）这一设计：以往很多 SQL/可视化混合方案在做超出基础图表的复杂可视化时都很难扩展，分层正好解决了这个痛点。
