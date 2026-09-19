---
layout: article
title: "WalShadow：从物理 WAL 实现亚秒级 Postgres 到 ClickHouse 复制"
issue: 810
number: 31
category: data
original_url: "https://clickhouse.com/blog/introducing-walshadow"
hn_url: "https://news.ycombinator.com/item?id=49660791"
date: 2026-09-18
---

## 文章摘要

ClickHouse 发布了开源复制引擎 WalShadow，它绕开了业界标准的 Postgres 逻辑复制路线，直接消费 Postgres 物理 WAL 流，把数据同步到 ClickHouse。官方基准中，Postgres 提交的事务约 200 毫秒后即可在 ClickHouse 中查询到，持续吞吐达到每秒 28.9 万行，基本跟上了源库自身的写入速率。

传统 CDC 方案依赖 `wal_level = logical`，需要在源库内部运行逻辑解码插件、维护复制槽，再经由 Kafka 或 JSON 中转写入目标库。WalShadow 走的是另一条路：它订阅的是 Postgres 为物理备库和崩溃恢复生成的同一份 WAL 流，把解码工作整体挪到源库之外。作者把整个链路拆成四个阶段：第一，追踪活跃 schema——过滤出目录（catalog）相关的 WAL 记录，回放到一个「影子」Postgres 实例中，该实例只保存 schema，不存数据，用于维持最新的表、列和类型目录；第二，并行解码数据变更——heap 记录被分发给一组 Rust 编写的解码器，不经过影子实例；第三，构建 ClickHouse 原生数据块——批处理器按表把解码后的行组装成完整的 native block，避免中间格式转换带来的保真度损失；第四，并行插入——独立的插入器池并发写入多个块，使解码与插入可以各自独立扩展。

由于块是并行处理的，到达顺序可能乱序。WalShadow 的做法是给每一行附上源端的 WAL 位置 `_lsn`，让 ClickHouse 按主键保留最新版本；对必须严格有序的操作（schema 变更、TRUNCATE）则插入屏障，等待此前所有数据落盘后再执行。它支持完整的复制生命周期：初始全量加载、持续复制、schema 演进（ADD/RENAME/DROP COLUMN、CREATE TABLE）、重启恢复以及计划内的源端切换。

对比对象是 ClickHouse 自家收购来的 PeerDB（ClickPipes 背后的引擎）：同样 8 vCPU 的 c8i.2xlarge 机器上，PeerDB 的提交可见延迟约 10 秒、吞吐约每秒 12 万行，WalShadow 分别是约 200 毫秒和 28.9 万行，延迟大约低 50 倍。作为参照，原生 Postgres 物理备库的基线约为 50 毫秒。

一个关键限制是：物理 WAL 是 WalShadow 架构的前提，而绝大多数托管 Postgres 服务并不向用户暴露物理 WAL。因此它目前只能用于自托管 Postgres 或 ClickHouse 自家的 Managed Postgres（后者以私有预览形式提供托管版）。作者把这件事放在更大的叙事里：从收购 PeerDB、在 ClickPipes 中推出 Postgres CDC，到推出基于本地 NVMe 的 ClickHouse Managed Postgres，目标都是把 Postgres 的事务能力与 ClickHouse 的分析能力拼成一套统一的数据栈。

## HN 评论精华

这条帖子讨论不多（十来条），但几乎全是硬核技术问答，作者 saisrirampur 亲自逐条回复。

- **vmsp** 抓住了最有意思的一点：他们没有用 `wal_level = logical`——那是多年来做 Postgres CDC 的「友好」方式——而是直奔 `wal_level = replica`，而后者除了 Postgres 自身的复制之外几乎没人在上面搭过东西。他还补了一句颇有时代感的判断：这种需要逐层啃协议细节的活儿，"如果没有编程 agent，大概根本不会有人去实现"，因为过去这属于巨大的时间黑洞。

- **DenisM** 质疑这套做法是否脆弱：复制实现在不同 Postgres 版本之间总会有变化。作者承认确实需要投入维护成本，但认为换来亚秒延迟和更低运维负担是值得的；他预期现有命令的 WAL 格式在版本间会相对稳定，新增命令则需要额外适配。他还提到 Postgres 社区内部也有把逻辑解码从主进程解耦出来的类似工作，团队打算把 WalShadow 的经验反馈给上游。

- **rgbrgb** 问能否用在 Supabase 托管的 Postgres 上——他们公司目前用轮询方式把 Postgres、日志、银行数据、Slack 都镜像进 ClickHouse，效率很低但胜在只需一个只读连接串。作者明确回答不行：WalShadow 需要直接访问物理 WAL，多数托管服务不开放。**ZiiS** 补充推荐了 Supabase Pipelines 作为折中方案，「没这么快，但比轮询强」。

- **phroas** 问到了 TOAST（大字段外部存储）如何处理，这在 CDC 里历来是痛点：未变更的 TOAST 字段是否需要与「上次见到的状态」合并才能还原整行。开发者 **__s** 坦率承认 "toast is a pita"（TOAST 很烦人），并透露正在做三种模式：禁用；把 TOAST 分片存在 ClickHouse 里再拉回来解析；以及存在影子目录里——延迟比走 ClickHouse 低，但要额外占磁盘。

- **__s** 还提到自己那边目前仍要求 `wal_level=logical`，希望未来能放宽，同时认同走物理 WAL 这条路「复杂度成本极高」。
