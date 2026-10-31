---
layout: article
title: "分布式 SQLite 的十二年：共识、备份与合并解决不同问题"
issue: 816
number: 28
category: data
original_url: "https://philipotoole.com/twelve-years-of-distributed-sqlite/"
hn_url: "https://news.ycombinator.com/item?id=50217500"
date: 2026-10-30
---

## 文章摘要

rqlite 作者 Philip O’Toole 从自己的 2014 年经历回顾分布式 SQLite。他最初需要在集群中保存少量关系数据，于是在 SQLite 前加入 Raft 日志，让写入经领导者和多数节点确认。由此形成的系统选择偏向一致性：网络分区中的少数节点不能继续接受写入。

文章把后来的方案分为几条路线。dqlite 更偏向嵌入产品的复制库；Litestream 把 WAL 流式备份到对象存储，重点是单机故障后的恢复；支持合并的扩展则面向多写入者与本地优先场景。它们名字里都有 SQLite，所回答的问题却不同：是否允许等待恢复、是否要求多数确认、是否接受副本暂时分歧，都会改变选择。作者也回顾了从开源组件走向托管平台的商业变化。文章是维护者的历史视角，而非完整产品基准；最有用的部分是先明确故障和一致性需求，再比较技术名称。

## HN 评论精华

**控制面是一个具体场景。** joostdecock 分享把 rqlite 用于可观测性产品控制面的经历；qaq 也表示控制层使用满意。它们是部署经验，而不是所有数据负载都适合的证明。

**备份与持续可用不同。** gizzlon 认为自己的 SQLite 加 Litestream 已经足够，同时承认接受某些极端情况下的数据损失。这样的容忍条件，正是比较不同方案时需要先说明的前提。

**还需要更多可比证据。** a3nz3g 询问性能基准，Kinrany 希望把嵌入、集群、备份和可选合并整合起来。关于 D1 的可靠性和文章文风也有争论，但零散体验不能据此推导整个产品的普遍结论。
