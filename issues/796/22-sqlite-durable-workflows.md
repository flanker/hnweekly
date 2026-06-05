---
layout: article
title: "持久化工作流，你只需要 SQLite 就够了"
issue: 796
number: 22
category: data
original_url: "https://obeli.sk/blog/sqlite-is-all-you-need-for-durable-workflows/"
hn_url: "https://news.ycombinator.com/item?id=48326802"
date: 2026-06-05
---

## 文章摘要

Obelisk 提出了一个观点：对于许多持久化工作流（durable workflow）系统来说，单凭 SQLite 就足够了。这是对 DBOS 那句"持久化执行你只需要 Postgres"的进一步延伸。其核心洞见在于把"持久性"这一关注点从"基础设施复杂度"中剥离出来。

在架构上，系统由多个 Obelisk worker 各自写入本地的 SQLite 数据库，再由 Litestream 异步地把变更复制到兼容 S3 的对象存储。观察者可以随时取回这些数据库进行检视，从而同时实现了备份和调试能力。

技术思路上，持久性来自被持久化的工作流状态，而非昂贵的基础设施。Obelisk 通过执行日志来记录进度，能从历史中回放工作流，并支持 activity 重试。这种模型让"工作流状态保留下来且易于检视"，同时让计算资源保持"廉价且可随时丢弃"。

主要优势包括：简单（没有网络跳转、没有控制平面、没有运维开销）、成本低（本地数据库文件免去了独立的数据库服务）、可移植（Litestream 支持异步备份到对象存储）、隔离性好（每个 agent 是一个小而自包含的单元，提供更好的故障边界）。

文章也诚实地指出了重要的权衡：Litestream 的复制是异步的，这意味着如果系统在同步完成前就崩溃，存在丢数据的可能。这种方案适合实验性的 AI 工作流，但缺乏共享网络数据库那样的高可用性。当需要更广泛的可扩展性或同步持久性时，Postgres 仍是更优选择。作者的建议是：从简单的 SQLite 起步，除非部署需求超出了单数据库能承载的范围。

## HN 评论精华

**关于"持久性"的核心辩论**最为热烈。`orf` 一针见血：文中那句"Litestream 复制是异步的，如果 SQLite 卷在写入被复制前消失，恢复会丢掉最新的本地写入，但这对许多 AI 和实验性工作流没关系"——换言之，"SQLite 并不是你需要的全部，除非你只是在做实验、根本不在乎持久性；而真要持久性，你还得加上 Litestream + 对象存储"。他还补充说，主数据库通常和运行临时工作流 pod 的 kubernetes 节点处于完全不同的故障类别。

`0cf8612b2e1e` 则提出了对称的观点：Postgres 默认也不会免费做同步复制；要么你有持久化存储要么没有，SQLite 和 Postgres 都能保证提交的本地持久性（`PRAGMA synchronous = full` / WAL 给出严格持久性），要做分布式持久性，无论用另一个 Postgres 节点还是对象存储，都是外部依赖。`paulddraper` 调侃道："没有持久性的持久化工作流，那叫分布式工作流。"

**对 Litestream 生产可用性的担忧**：`gwking` 分享了真实教训——他用 0.3.x 多年没问题，但升级到 0.5.x 时遇到磁盘用量失控的问题，差点导致生产宕机，最终选择退役 Litestream 换成"更笨但更可靠"的方案。`dilyevsky` 也半开玩笑："只要不崩溃、Litestream 没有那种每隔一个版本就出一次的数据损坏 bug，它就是持久的。"

**AI 时代的趋同模式**：`Xcelerate` 和 `zrail` 都表示自己独立想到了类似做法——让 agent 先根据规格设计 DAG，每一步把状态存进 SQLite，迭代时只需改一两步再重跑。`Xcelerate` 感慨大家正不约而同地收敛到相似的 agent 编排模式上。`sgloutnikov` 指出 DBOS 其实也支持 SQLite，原型阶段默认就用它，并贴出了一份 awesome-workflow-engines 列表，说明"自己造一个工作流引擎"是多么常见的需求。
