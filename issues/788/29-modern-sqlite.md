---
layout: article
title: "现代 SQLite：你不知道它已经拥有的功能"
issue: 788
number: 29
category: data
original_url: "https://slicker.me/sqlite/features.htm"
hn_url: "https://news.ycombinator.com/item?id=47616704"
date: 2026-04-03
---

## 文章摘要

这篇文章全面介绍了 SQLite 在近年来发展出的一系列现代化功能，证明它早已超越了"简单嵌入式数据库"的定位，成为一个功能丰富的数据库引擎，适用于桌面应用、本地优先（local-first）工具和小型服务等场景。

**JSON 数据支持**：SQLite 内置 JSON 扩展，允许在表中直接存储和查询 JSON 文档。通过 `json_extract()` 函数可以提取 JSON 字段，还支持在 JSON 表达式上创建索引，使半结构化数据的查询性能出人意料地高效。新的箭头语法（`->>`）提供了比 `json_extract` 更简洁的写法，且能根据值类型返回适当的类型。

**FTS5 全文搜索**：SQLite 的 FTS5 扩展将其变成了一个可用的全文搜索引擎。通过创建虚拟表并指定分词器（如 porter），可以实现排名查询、短语搜索、前缀搜索等功能，无需管理外部搜索服务。支持 `NEAR` 操作符进行邻近搜索。

**窗口函数与 CTE**：SQLite 支持公共表表达式和窗口函数，解锁了一整类分析查询能力。文章展示了使用 `SUM() OVER (PARTITION BY ... ORDER BY ... ROWS BETWEEN ...)` 计算累计总额的示例，结合 CTE 可以在单个 SQLite 文件上构建丰富的报表和仪表板。

**STRICT 表与更好的类型系统**：SQLite 以灵活的类型模型著称（或臭名昭著）。现代 SQLite 添加了 `STRICT` 表，强制执行类型约束，类似于 PostgreSQL 等传统数据库。在插入时拒绝无效类型，使数据模式更可预测，减少隐微 bug。

**生成列（Generated Columns）**：允许将表达式存储为虚拟列或存储列，保持派生数据与源数据紧密关联，可以为生成列创建索引并高效查询。

**预写式日志（WAL 模式）**：WAL 是一种日志模式，显著改善并发性能——读者不阻塞写者，写者不阻塞读者。只需一条 `PRAGMA journal_mode = WAL;` 即可启用。

## HN 评论精华

**SQLite 的生产级能力**：aaviator42 分享了使用 SQLite 作为唯一存储层服务日均数十万用户网站的经验，证明 SQLite 的鲁棒性远超一般认知。tombert 分享了用 SQLite 替代原始文件写入后获得了崩溃恢复能力的经历。

**STRICT 模式的实际价值**：crazygringo 描述了 STRICT 模式如何帮助捕获 Python 和 NumPy 整数类型不匹配的 bug。krylon 赞赏 STRICT 表并使用约束来强制验证 TEXT 列中的 JSON 有效性。momo_dev 也确认 STRICT 表帮助发现了类型插入 bug。

**Richard Hipp（SQLite 作者）亲自回复**：SQLite 的创建者 Dr. Richard Hipp 以用户名 "SQLite" 出现在评论区，解释了灵活类型系统如何与 JSON 良好配合——`->>` 操作符能返回适当类型，而 PostgreSQL 中则不行。他还表达了对脚本环境中动态类型的偏好，并质疑了严格类型的研究基础。

**FTS5 的局限性**：FooBarWidget 发现 FTS5 在子词搜索方面有限（如搜索 "Daemon" 无法匹配 "DaemonSet"）。ers35 建议使用 trigram 分词器改善这一问题。kherud 也询问了改善 SQLite 全文搜索质量的方法。

**类型系统争论**：andrewstuart 认为 STRICT 应该成为默认行为，但 sgbeal 解释了向后兼容性的考量。QuadrupleA 质疑松散类型的实际好处，而 lateforwork 认为在单应用场景中灵活性有利于 schema 演进。fushihara 希望 SQLite 添加原生布尔类型和日期时间类型。

**并发写入改进**：nikisweeting 提到 Turso 的多写入者支持正在解决 SQLite 高并发场景的限制，这是 SQLite 生态系统中的重要进展。

**备份 API 的创意用法**：malkia 描述了使用 SQLite 的备份 API 进行变更检测的方案，展示了 SQLite API 的多功能性。
