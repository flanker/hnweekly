---
layout: article
title: "DuckDB 内幕：它为什么这么快？"
issue: 798
number: 29
category: data
original_url: "https://www.greybeam.ai/blog/duckdb-internals-part-1"
hn_url: "https://news.ycombinator.com/item?id=48553388"
date: 2026-06-19
---

## 文章摘要

本文是深入剖析 DuckDB 架构与性能的系列第一篇。DuckDB 从 2019 年的一个研究项目，成长为如今应用最广泛的数据库之一，从 notebook 到 SaaS 平台的嵌入式分析都能见到它的身影。其速度优势主要来自几方面：

- **进程内执行**：DuckDB 是库而非服务器，省去了序列化和网络开销。传统数据库要把结果编码进线缆协议、经 TCP 传输、再在客户端解码；DuckDB 直接运行在客户端进程内，对 pandas DataFrame 等数据结构实现"零拷贝"访问。
- **列式存储 + Zone Map**：数据按列组织，查询只需读取相关列。每列被切分为最多 122,880 行的 row group，每个 row group 携带一份记录最小/最大值与空值计数的 zone map，使 DuckDB 在谓词不匹配时能整组跳过，大幅削减 I/O。
- **全面的查询优化**：DuckDB 实现了约 33 个优化 pass，包括谓词下推、子查询去关联化（把相关子查询转为 join）、join-filter 下推（把哈希表构建期的运行时过滤器推回扫描端），以及用动态规划求最优连接顺序。

在执行层面，优化器生成逻辑计划后转为物理算子，并组织成"流水线"——数据流式穿过一系列算子；GROUP BY、ORDER BY 这类"流水线断点"需读完全部输入才能产出，形成天然边界。多线程执行中的 sink 分三阶段（sink/combine/finalize），各线程使用独立的本地状态以避免锁竞争。存储层方面，DuckDB 数据库是采用 256KB 带校验和块的单文件；同时也能凭借列式组织与 min/max 统计高效查询 Parquet 文件，并通过"嗅探器"自动识别 CSV 的方言、列类型与表头。

## HN 评论精华

**anitil**：盛赞 DuckDB 让生活轻松太多——一句 `select * from 'data.json'` 就很美妙；通常一个项目要么擅长小问题、要么擅长大问题，而它两头都行。

**0xferruccio**：分享实际用法——用 DuckDB 分析公司每位工程师上传到 S3 的 Claude Code 会话数据，借此发现开发体验的短板并用清晰指标佐证改进影响；从没碰过 DuckDB 的同事都惊讶于它如此易用。

**mcv**：感慨"是不是一切都在变成列式"——Parquet、Arrow 都是列式，如今 DuckDB 也靠列式取胜，直言这一趋势既令人着迷又需要时间消化。

**holografix / jdw64 等**：追问既然有 Python + Pandas，为何 DuckDB 还这么火，普遍答案是更好的性能加上 SQL 接口。

**willtemperley**：指出一个重要警告——在无法动态链接的场景（如 App Store），DuckDB 不是好选择，因为静态链接扩展非常困难；这一点上 Arrow C++ 更有优势，二者其实可互补共存。

**tobyhinloopen / codingbear**：表示自己是被 LLM"带"着用上 DuckDB 的——vibe coding 项目大量用它处理 JSON(L) 文件，配合 Claude Code 切分各类表格数据极为顺手。
