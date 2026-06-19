---
layout: article
title: "TimescaleDB 是如何压缩时序数据的"
issue: 798
number: 30
category: data
original_url: "https://roszigit.com/en/blog/timescaledb-compression-hypercore"
hn_url: "https://news.ycombinator.com/item?id=48544451"
date: 2026-06-19
---

## 文章摘要

TimescaleDB 通过一个名为 **hypercore** 的行列混合引擎，实现了最高 98% 的压缩率。与 PostgreSQL 处理单个大字段的 TOAST 机制不同，hypercore 针对的是时序数据的"跨行模式"，挖掘通用压缩算法无法利用的结构性规律。

其工作方式是：新数据先落入传统的行式 chunk 以保证写入速度，较旧的 chunk 再自动转为列式压缩格式。约 1000 行被归为一批，每批变成一行包含各列数组的压缩行，从而支持按列操作和选择性列访问。压缩算法按列类型自适应选择：

- **数值/时间戳列**：delta 编码、delta-of-delta 与 simple-8b 打包。delta-of-delta 对规整间隔会产生大量零值，存储大幅缩小。
- **浮点测量值**：基于 XOR 的压缩（受 Gorilla 启发），利用相邻值的前导/尾随零。
- **重复列**：游程编码（RLE），值只存一次并附重复次数。
- **JSONB/字符串**：字典压缩，必要时回退到 TOAST。

例如温度读数 `72.5, 72.7, 72.4` 会变成 `72.5, +0.2, -0.3`，体积骤减。两个关键配置是 **segmentby**（指定按哪一列的值对整批分组，如 machine_id，其值每批只出现一次，规划器可借元数据跳过无关批次）和 **orderby**（批内排序，通常为 `time DESC`，时间有序能最大化 delta 编码效果）。一般建议每个 segment 含 100–10,000 行。性能上，查询通常因 I/O 减少而加速（文中一例达到 28 倍提速、42 倍压缩比），但对压缩 chunk 的点查和 UPDATE/DELETE 可能因解压而变慢。

## HN 评论精华

**gopalv**：认为"压缩对查询性能的影响"那一节最值得关注——数据库的本职是查找、聚合、更新数据，存储只是权衡的体现；任何能加速过滤拒绝或扫描速率的压缩才算更优，仅用 IO 换 CPU 的则未必。

**tudorg**：分享自己也在做一个时序 PG 扩展（deltax），并列举诸多加速技巧：为每个 segment 保留 max/min/sum、distinct 数、布隆过滤器等元数据，许多常见查询无需解压列即可直接基于元数据回答。

**blackoil**：指出 Facebook 的 Gorilla 早有此思路——值存为 delta，时间存为 delta of delta。

**self_awareness**：提醒压缩功能用的不是 Apache 许可证，许多发行版包管理器装的 timescaledb 只支持 Apache 许可，因此很可能无法使用压缩，需手动安装。

**frollogaston**：提出这项特性对非时序数据同样有用——只要某个被分组列重复度极高、以致普通索引变慢的场景都适用，类比 Spanner 的 interleaved 表动机。

**robocat**：吐槽标题里的"最高 98%"——凡用"up to"的标题大多是水文，并戏言想当标题审核的"仁慈独裁者"。
