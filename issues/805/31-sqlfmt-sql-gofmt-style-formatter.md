---
layout: article
title: "sqlfmt：一款 gofmt 风格的 SQL 格式化工具"
issue: 805
number: 31
category: data
original_url: "https://tapoueh.org/blog/2026/08/introducing-sqlfmt-an-sql-gofmt-style-formatter/"
hn_url: "https://news.ycombinator.com/item?id=49275599"
date: 2026-08-14
---

## 文章摘要

作者 Dimitri Fontaine 是 PostgreSQL 的 Major Contributor，也是《The Art of PostgreSQL》一书的作者。他在这篇文章里发布了自己写的 SQL 格式化工具 sqlfmt，定位非常明确：一个 gofmt 风格的格式化器——只有一种固定风格，没有任何配置开关。跑一遍、提交结果、继续干活，不要在团队里为「关键字要不要大写」「逗号放行首还是行尾」这类问题反复争论。

他动手写这个工具的直接动机是：多年来他形成了自己的一套 SQL 排版习惯，但一直找不到能实现这套习惯的工具；同时也不断有读者问他，书里那几百个手工排版的示例查询到底是用什么工具格式化的。现在终于有了答案。

这套风格最核心的特征叫做 **river alignment（河流对齐）**：在每一层查询嵌套里，所有子句关键字（select、from、where、group by、having、order by）都被右侧补齐到同一列，于是这些关键字在视觉上形成一条垂直的「河流」，而后面的表达式自然地向右流淌。举例来说，`select` 是六个字母，`group by` 和 `order by` 是八个字母，所以后两者会顶到基准缩进的最左边——这不是针对它们的特例规则，而是对齐规则本身的副产品。

完整规则写在仓库的 STYLE.md 里，是从书中语料库里 343 个手工排版的 .sql 文件反向工程出来的。几条要点：所有 SQL 关键字和函数名一律小写（`count(*)`、`coalesce(...)`、`row_number() over(...)`）；列清单从第二列开始每行一列、使用行尾逗号；`and` / `or` 放在续行开头，并右对齐到与 `where` 结尾同一列；CREATE TABLE 里的列名做左侧补齐，让所有数据类型从同一列开始；注释永远不会被丢弃——前置注释会重新缩进并按 78 列重排，块内的行尾注释会补齐到共享列。作者也坦承，这套风格最适合独立维护在 .sql 文件里的查询，而不是嵌在其他语言源码里的字符串。

工具形态上，CLI 完全照抄 gofmt 的接口：直接输出到 stdout、`-w` 原地重写、`-l` 列出会被改动的文件、`-d` 输出 unified diff、也支持 stdin 到 stdout 的管道用法。`-l` 在 CI 里特别有用：只要有文件需要改动就以退出码 1 结束，因此可以在每个 PR 上强制风格检查，而不必把格式化结果存进流水线。安装用 `go install`，同时提供了 Emacs 和 Vim/Neovim 的编辑器集成（Vim 侧是把 sqlfmt 接到 formatprg / equalprg 上，于是 `gqip`、`gg=G` 这些常规动作都能直接用）。

此外他还做了一个纯浏览器端的在线版本：整个 Go 引擎用 TinyGo 编译成 WebAssembly，压缩后大约 130 KB——相比标准 Go WASM 构建产出的 2.9 MB，这个体积才适合嵌进网页。

文中技术上最值得一读的是「为什么用 tokenizer 而不是 AST」这一节。作者指出，主流的生产级 SQL 格式化器（pg_format、sqlfluff、prettier-plugin-sql）几乎都建立在 token 流而非语法树之上，sqlfmt 也一样，理由有两条：其一是**注释**——PostgreSQL 自己的解析器会丢弃注释，任何基于 AST 的格式化器都得额外做一遍注释恢复，那时候「让解析器处理结构」的优势基本已经被抵消；其二是**健壮性**——网页版必须能处理访问者粘贴进来的任何东西，包括半截语句、从大文件里抠出来的片段、语法并不完全合法的代码，token 流方案能优雅降级，而基于文法的方案会硬失败。更进一步，河流对齐本质上关心的是 token 在页面上的位置，而不是查询的语义结构，所以 tokenizer 天然契合这个问题。

正确性验证上，测试套件用 pganalyze/pg_query_go（包装了真正的 PostgreSQL C 解析器）作为「预言机」：只要 `fingerprint(input)` 等于格式化后的 fingerprint，就能保证排版没有悄悄改变查询语义。作者也如实说明了现状：tokenizer、河流对齐布局引擎、注释挂载和 CLI 都已就位，近期修掉了几个边角问题（`<->` KNN 距离运算符曾被误词法分析；CTE 之间的 UNION ALL / INTERSECT / EXCEPT 现在能正确重置河流；`with recursive` 不再悄悄丢掉 RECURSIVE 关键字）；但深度嵌套的子查询和冷门 DDL 仍然是尽力而为——STYLE.md 本身就承认这些是整套风格里机械规则性最弱的地方，因为书中的语料在这些地方也是手工微调而非遵循统一规则。

## HN 评论精华

这条提交在 Hacker News 上的反响非常冷清：截至抓取时只有 4 分，**零条评论**。

因此本条目没有可供翻译总结的社区讨论。为避免虚构，这里不编造任何用户名和发言。有兴趣的读者可以直接参考上面的原文要点：这个项目最有讨论价值的两个点分别是「零配置的强观点格式化器（gofmt 哲学）能否适用于 SQL 这种风格分歧极大的语言」，以及「格式化器该用 token 流还是 AST」——作者给出的注释保留与容错降级这两条理由，对写任何语言格式化器的人都有参考价值。
