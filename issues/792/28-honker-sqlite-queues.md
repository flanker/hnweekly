---
layout: article
title: "Honker：在你的 SQLite 文件里塞进耐久队列、流、Pub/Sub 与 Cron 调度器"
issue: 792
number: 28
category: data
original_url: "https://honker.dev/"
hn_url: "https://news.ycombinator.com/item?id=47963316"
date: 2026-05-01
---

## 文章摘要

Honker 是一款新颖的轻量级中间件，作者 russellthehippo 把传统上要靠 Redis、RabbitMQ、Kafka、cron 守护进程拼出来的几件事——耐久队列、流（Streams）、发布/订阅（Pub/Sub）、cron 风格调度器——全部塞进一个 SQLite 文件里。理念非常直接：对于很多"小到不需要 Kafka，但又比纯内存队列更严肃"的项目来说，多引入一个独立服务往往是过度工程；如果数据库本身就在那儿，就让它顺便承担消息基础设施。

Honker 的核心卖点：

- **单文件部署**：一个 `.db` 文件包含队列、流、订阅、定时任务等所有状态，备份、复制、迁移与日常 SQLite 文件没有区别。
- **事务一致性**：写消息和写业务数据可以放在同一个 SQLite 事务里，避免传统外置队列在"DB 提交成功但消息发送失败"或反之的角落 bug。这点对幂等性敏感、状态机驱动的系统尤其有价值。
- **API 完整**：除了 push/pop 的队列语义，还提供 append-only 的流（带 offset/cursor）、多订阅者扇出的 pub/sub、以及 cron 表达式驱动的定时任务调度——基本覆盖了一个中型产品 90% 的异步需求。
- **轻量轮询模型**：消费者用约 1 ms 间隔的轻量 SELECT 来感知新消息，省去了平台相关的 inotify/kqueue 实现成本。作者在 HN 上承认初期为了跨平台简洁性才选择这种实现，未来计划增加 push 通道。

它定位在那个"队列方案的尴尬中段"——比内存队列要严肃，但远没到需要 Kafka 这种重量级中间件的规模；尤其适合单机服务、SaaS 边车、桌面应用、CLI 工具乃至嵌入式场景。结合 Litestream 之类的复制方案，还能拥抱 follower 容灾。

## HN 评论精华

- **tptacek** 直接挑战了项目的市场定位：作者一边宣称"避免内核文件监听器的复杂度"，一边又用每毫秒一次的轮询 SELECT 替代它——这个对比并不诚实，性能特性也未必更好。轮询本身是 trade-off，不能包装成"更轻"。

- **codedokode** 把 1ms 轮询的成本算了笔细账：每秒约 1000 次 SELECT，每次 ~3µs，意味着 ~3ms CPU/秒/进程，相当于 0.3% CPU 浪费在每条等待线程上。在移动端或电池受限场景，这还会阻止 CPU 进入低功耗状态，实际功耗放大效应可能远大于这个数字。

- **paulddraper** 的态度更尖锐：他暗讽这种"事务+消息一体化"的承诺与轮询代价之间不太对得上账，开发者本可以用条件变量+互斥量的传统 push 机制实现得更优雅。

- **ncruces** 给出了一个更内行的方案：SQLite 自带的 `wal_hook` API 允许在 WAL 写入时触发回调，可以把"每毫秒轮询"换成事件驱动通知。但他也指出局限——WAL hook 默认只对同一连接的更改生效，要做跨进程通知仍需额外工作。

- **wmanley** 补充了一个工程界已经验证的取巧路径：直接对 WAL 文件做 `inotify`，比应用级轮询更省 CPU，也比 hook 方案更易跨进程。

- **jallmann** 建议回归经典：condvar + mutex 在同进程内最干净；跨进程则可结合 SHM 信号量。

- **russellthehippo（作者）** 在线回应坦诚承认轮询不是终态：原始设计就考虑过 inotify/kqueue，只是为初版避开平台差异选择了轮询；roadmap 已经把 push 通知列为高优先项。

- **koito17** 提出反例支持：k3s（Kubernetes 的轻量发行版）的 kine 后端就用 SQLite 轮询实现 watch 语义，已在大量生产环境跑通，证明该模式可行。

- 周边方案讨论也很热烈：Elixir 生态的 **Oban** 现在原生支持 SQLite，**Graphile Worker** 在 PostgreSQL 上提供类似抽象，**Litestack**（Rails）则把多种基础设施合并进 SQLite。Honker 的差异化在于跨语言可用 + 内置 cron + 流语义，但要在这条赛道占住位置，作者还需要把轮询带来的疑虑一一化解。
