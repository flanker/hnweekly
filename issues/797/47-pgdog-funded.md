---
layout: article
title: "PgDog 完成融资，即将走进你身边的数据库"
issue: 797
number: 47
category: startup_news
original_url: "https://pgdog.dev/blog/our-funding-announcement"
hn_url: "https://news.ycombinator.com/item?id=48476466"
date: 2026-06-12
---

## 文章摘要

PgDog 是一个位于 PostgreSQL 前端的代理层，旨在让 Postgres 实现水平扩展，从而免去为了规模而迁移到 MongoDB、DynamoDB 等其他数据库的需要。使用方式简单：在现有 Postgres 前部署一个 Docker 镜像，再修改数据库连接串即可。

PgDog 的主要能力包括连接池、负载均衡，以及跨多个实例的数据库分片（sharding），可部署在本地、自有云账户或开发机上。其团队由有 Instacart 高速增长期 Postgres 扩展经验的基础设施工程师组成。

公司宣布完成 550 万美元融资，投资方包括 Basis Set、YC、Pioneer Fund 等。创始人 Lev Kokotov 表示，这笔资金为产品提供了"数年的现金跑道"用于扩展。

目前 PgDog 在多个生产部署中每秒处理超过 200 万次查询，已分片处理超过 20TB 数据，Docker 拉取量达 140 万次，并提供带 SLA 支持的付费企业版。公司计划每周发布，并正在开发面向 AWS 的企业级特性。

## HN 评论精华

**eikenberry**（541 赞）：Postgres 的主要痛点其实不是扩展，而是高可用。即便每分钟处理 10 万以上事务，手动故障切换流程依然繁琐易错。

**r7n**：DynamoDB 能在键空间上横向扩展写入，而 Postgres 即便加了分片层也做不到，这一根本架构差异对写密集型负载很关键。

**doctorpangloss**：质疑仅靠负载均衡是否构成真正的高可用——PgDog 文档中提到"数据库需由外部管理"来完成副本提升，说明它并非完整的 HA 方案。

**ahachete**：担心 PgDog 基于取模的分片分布，认为"唯一的扩容方式就是整体重新分片"，并指出 Citus 用虚拟分片可在无需全量重分片的情况下迁移。

**tomtomtom777**：对"每分钟数十万杂货配送订单"的说法存疑，指出 Amazon 每分钟也只有约 2 万订单，该数字可能指数据库操作而非真实订单。

**levkk**（创始人）：承认取模分布的局限，承诺"很快"加入 rendezvous 哈希，并指出与 Postgres 分区的兼容性可帮助用户逐步迁移。
