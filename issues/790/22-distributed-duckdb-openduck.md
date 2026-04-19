---
layout: article
title: "OpenDuck：分布式的 DuckDB 实例"
issue: 790
number: 22
category: data
original_url: "https://github.com/citguru/openduck"
hn_url: "https://news.ycombinator.com/item?id=47761997"
date: 2026-04-17
---

## 文章摘要

OpenDuck 是一个受 MotherDuck 架构启发的开源项目，目标是为 DuckDB 带来"差分存储、混合（双端）执行以及透明的远程数据库"能力。用户可以通过类似 `ATTACH 'openduck:mydb'` 这样简洁的语法无缝挂载远程数据库，让分布式的数据访问体验与本地查询几乎无异。

在存储层面，OpenDuck 实现了差分存储架构：系统使用追加式的不可变"封存层"（sealed layers）保存数据到对象存储，并通过 PostgreSQL 维护元数据。这种设计支持基于快照的一致性读取、单一序列化写入通道，以及多个读取方的并发访问。

在执行层面，OpenDuck 提供混合执行能力——一个查询可以部分在本地机器执行，部分在远程 worker 执行。网关会将执行计划切分，将算子标记为 `LOCAL` 或 `REMOTE`，并通过桥接算子管理两端边界。这样做的好处是只需在网络上传输中间结果，最大限度降低网络开销。远程表作为一等公民被纳入 DuckDB 的本地目录（catalog），可以参与与本地表的 JOIN、CTE 和优化器操作。

协议层面，OpenDuck 采用极简的 gRPC 实现配合 Arrow IPC 批处理。这种开放性避免了供应商锁定——任何支持 gRPC 并返回 Arrow 数据的服务都可以作为兼容后端。

架构上由几个关键组件组成：网关负责身份认证、worker 注册、路由和混合计划切分；worker 是内嵌的 DuckDB 实例，提供远程执行能力；元数据仓库用 PostgreSQL 存储元数据，封存层则保存在对象存储中；扩展是 DuckDB 的 C++ 扩展，实现了 StorageExtension 和 Catalog 接口。项目主要使用 Rust（62.5%）和 C++（30.5%）开发，采用 MIT 许可证，目前在 GitHub 上拥有 450 星和 18 个 fork。

## HN 评论精华

1. **解决 DuckDB 的独占锁问题**：讨论中最核心的痛点是 DuckDB 的独占锁机制——多进程无法并发写入。一位评论者指出："你不能从多个进程同时写入它。如果一个进程以写模式打开数据库，其他进程就完全无法修改它。" OpenDuck 用差分存储加追加层加快照隔离的方案，实现了多读取方并行、单个网关序列化写入的模式。

2. **OpenDuck 与 DuckLake 的差异**：有评论者询问与 DuckLake 的区别。项目作者回应，DuckLake 处理的是湖仓层级的并发问题，而 OpenDuck 工作在不同的层次。他解释："一旦你需要回退到 DuckDB 自身的计算引擎去处理 DuckLake 尚不支持的功能时，你又回到了单一 .duckdb 文件和独占锁的世界。"

3. **与 Raft 方案的对比**：另一位开发者分享了他们基于 OpenRaft 的分布式方案——做的是全量复制。他们的优先级是"让每个节点都能独立提供读取服务，实现零网络延迟"，而不是查询联邦。这代表了分布式 DuckDB 的另一种主流选择。

4. **生产就绪度的质疑**：也有人对成熟度表达了怀疑。一位分析者认为该项目"相当新，距离生产可用还很远"。批评者更直白地称其为"氛围编程（vibe coded）追云趋势的典型产物"——生态中充斥着未经充分测试的概念验证。

5. **DuckDB 生态的复杂性蔓延**：有评论者担忧复杂度的扩散。他观察到："DuckDB 如此严重地依赖其扩展机制"，在保持核心轻量的同时，正面临失去 SQLite 式简洁性的风险。
