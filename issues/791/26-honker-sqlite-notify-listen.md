---
layout: article
title: "Honker：在 SQLite 上实现 Postgres NOTIFY/LISTEN 语义"
issue: 791
number: 26
category: data
original_url: "https://github.com/russellromney/honker"
hn_url: "https://news.ycombinator.com/item?id=47874647"
date: 2026-04-27
---

## 文章摘要

Honker 是一个 SQLite 扩展，把 Postgres 风格的 `NOTIFY`/`LISTEN` 语义带到了 SQLite 世界。它在单一 SQLite 文件之上提供了"持久化的发布订阅、任务队列和事件流，无需客户端轮询，也无需独立的守护进程或消息代理"。换句话说，原本要靠 Redis、RabbitMQ、Celery 才能实现的能力，现在只需要一个 SQLite 文件。

它的核心价值在于"事务一致性"：业务写入和入队操作可以在同一个 SQLite 事务里原子提交。如果业务事务回滚，对应的任务、事件、通知也会一起回滚——这就是经典的"事务性 outbox"模式被进一步简化到极致。功能层面，Honker 提供了三类原语：跨进程的轻量级通知（ephemeral notifications）、带消费者偏移量的持久事件流（durable streams）、以及至少一次（at-least-once）语义的工作队列。其工作队列支持指数退避重试、优先级、延迟执行、死信表、任务超时与过期；此外还内置了具名分布式锁、限流器、带 leader 选举的 cron 风格周期任务调度，以及可选的任务结果持久化。

技术亮点在于"唤醒机制"：传统轮询会带来延迟和资源浪费，Honker 利用 SQLite 自身提供的 `PRAGMA data_version`——一个会随每次 commit 自增的内部计数器——以 1 毫秒间隔检查。这样实现了跨进程"个位数毫秒"延迟的事件传递，而无需应用层做主动轮询。队列操作还使用 partial index 把工作集保持在很小的体量，让 claim 操作不受历史数据增长影响。Honker 同时为 Python、Node.js、Rust、Go、Ruby、Bun、Elixir 提供了绑定，所有语言共享同一个 SQLite 扩展和兼容的 schema。

## HN 评论精华

- **作者 russellthehippo** 解释了核心动机：Honker 在 SQLite 上加了"跨进程 NOTIFY/LISTEN"，提供个位数毫秒延迟的"推式"事件投递，而不需要单独跑一个 daemon，所有状态都活在 SQLite 文件里。

- **andersmurphy** 指出最受益的场景是"单线程应用 + 多进程并发"，尤其是 Python、Ruby 这类只能靠多进程做并发的语言。作者也认同这正是 Honker 的甜蜜点：不轮询就能做到 1ms 反应速度，同时与业务写入保持原子性。

- **ncruces** 提出了一个非常关键的技术建议：与其用 `stat()` 检查文件，不如直接用 `PRAGMA data_version`，这是 SQLite 内置的 commit 计数器，单调递增，免疫时钟漂移，并能正确处理 WAL 截断和事务回滚。这条建议显然命中了核心，作者很快表示会采用。

- **zbentley** 进一步在文件监控的边缘场景上提出了周到的反馈：建议同时跟踪 inode 和设备号来应对"文件被替换"，并防御系统时钟漂移。作者回应将采用 data_version 轮询 + 周期性 stat 检查的"组合"方案。

- **tuo-lei** 一句话点出了 Honker 的真正卖点：相比于独立 IPC 系统，"业务数据原子提交"才是关键——它从根本上消灭了"通知发出去了，但事务回滚了"这一类经典 bug。

- **ncruces** 也提醒了适用边界：如果你打算把它用在分布式场景，并涉及跨节点的 claim/ack，就需要分布式锁，那时 Honker 在规模上将不再合适。换句话说，它最闪光的地方是"单机或共享存储下的轻量化基础设施"。
