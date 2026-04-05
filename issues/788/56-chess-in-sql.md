---
layout: article
title: "用纯 SQL 实现国际象棋"
issue: 788
number: 56
category: fun
original_url: "https://www.dbpro.app/blog/chess-in-pure-sql"
hn_url: "https://news.ycombinator.com/item?id=47562961"
date: 2026-04-03
---

## 文章摘要

DB Pro 博客作者 Jay 展示了一个令人印象深刻的技术实验：完全使用纯 SQL 渲染和操作一个国际象棋棋盘，不依赖 JavaScript 或任何框架。文章从最基础的数据建模开始，将棋盘表示为一张包含 32 行（每个棋子一行）的表，记录每个棋子的位置（file 列和 rank 行）。

核心技术亮点在于"行转列"（pivoting）。SQL 天然输出行数据，但通过条件聚合（conditional aggregation）技术，作者将行数据转换为 8x8 的网格显示。具体方法是：使用 CTE（WITH full_board AS ...）通过 CROSS JOIN 生成所有 64 个格子，再 LEFT JOIN 棋子表；然后通过 MAX(CASE WHEN file = N THEN piece END) 提取每列的棋子；用 COALESCE 填充空格为点号；最后按 ORDER BY rank DESC 排列使黑方在顶部。走棋操作通过简单的 DELETE（移除起始位置棋子）和 INSERT（在目标位置放置棋子）或 UPDATE 语句实现。文章还提供了交互式 SQL 沙箱，读者可以直接在浏览器中尝试意大利开局、后翼弃兵等经典开局。作者的核心观点是：SQL 可以表示任何有状态的 2D 网格，不仅是棋盘，还包括日历、热力图、座位表、生命游戏等。

## HN 评论精华

**作者的核心观点引发讨论**：作者 upmostly 强调文章真正的重点是 SQL 可以表示任何有状态的 2D 网格。eastbound 反驳说 SQL 虽然能处理 2D 数据但效率极差，这恰恰暴露了需要改进的地方。

**SQL 技术讨论**：tn1 指出 DuckDB 和 Microsoft Access 有原生 PIVOT 关键字。amichal 提到 PostgreSQL 的 crosstab 函数。h3lp 演示了 SQLite 的 FILTER 子句作为传统 pivot 操作的替代方案。andersmurphy 推荐了 R*Trees 用于多维数据处理，指出 SQLite 实现支持最多 5 个维度。danielszlaski 询问是否可以通过 CHECK 约束或触发器来强制走棋合法性。

**AI 生成内容争议**：多位评论者（mwigdahl、slopinthebag、catlifeonmars、streetfighter64）质疑文章是否由 LLM 生成，批评写作风格听起来过于通用。mahogany 表达了对 LLM 生成内容降低网络写作质量的担忧。

**幽默与延伸**：cat-whisperer 问"有人能用 SQL 做 DOOM 吗？"，evnc 回复了一个用 DuckDB 构建的光线追踪器的链接。jimgoneill 问为什么不叫"ChessQL"，作者回复说 ChessQL 已经是 PyPI 上的一个 Python 包了。herodoturtle 称赞这篇文章是 dbpro.app 的有效品牌推广。

**棋局准确性**：jollygoodshow 指出文章中关于"歌剧游戏"（Opera Game）将杀状态的示例可能有不准确之处。Beestie 提出了将棋子移动参数单独存储的替代方案设计。
