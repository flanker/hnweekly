---
layout: article
title: "23 种 EC2 实例上的 PostgreSQL 性能与成本对比"
issue: 801
number: 35
category: data
original_url: "https://postgres.saneengineer.com"
hn_url: "https://news.ycombinator.com/item?id=48816900"
date: 2026-07-17
---

## 文章摘要

作者 Andrei（HN 用户名 anivan_）做这个基准测试的初衷，是不满于人们常被大厂案例和流行书籍"带偏"、把后端系统设计得过度复杂。他倡导"精简架构（lean architecture）"，并把这个 PostgreSQL 选型工具作为其"数字花园"中的一件作品发布出来。

测试在 AWS us-east-1 上针对 PostgreSQL 17 展开，围绕 23 种 EC2 实例类型、搭配不同磁盘选项（gp3-baseline 与 gp3-max）展开，合计约 52 种配置组合。工作负载模拟"90% 读 / 10% 写"、32 个并发连接，数据集初始规模 1 GB，测量每秒请求数（RPS）、延迟分位数和成本效率。关键结论包括：**m8g.large**（基于 Graviton 的 ARM 实例）是"满足 33,000 RPS 目标的最便宜选项"，约 82 美元/月即可跑出 45,515 RPS，是综合性价比之王；在原始效率上，**c8i.large** 以 52,203 RPS 领先；若需要更高吞吐，m8g.xlarge（约 147 美元/月、70,871 RPS）和 m8g.2xlarge（约 278 美元/月、81,451 RPS）表现强劲。整体来看，基于 Graviton 的 ARM 实例在成本指标上普遍优于 x86-64；52 种配置中有 30 种越过了 33,000 RPS 门槛，而 Graviton 实例频繁出现在实惠区间。此外，小型的 t 系列（t3、t4g）尽管便宜，但 RPS 不足 6,000，不适合生产级 PostgreSQL 负载；一个反直觉的发现是，gp3-max 磁盘配置虽然提升了吞吐，却有时反而降低了"每美元 RPS"的效率，说明在磁盘上砸钱存在边际递减。

## HN 评论精华

作者本人（anivan_）在评论区积极回应，讨论几乎变成了一份"下一步该测什么"的众筹清单。

- **ballislife30** 希望看到"同一 EC2 实例类型上 Aurora PostgreSQL 与自建 PostgreSQL 的对比"，**toredash** 进一步想看与 RDS 的对比。作者表示这些都容易加入，并计划把 RDS 及其各种特性作为下一步。

- **nijave** 提供了扎实的技术补充：他在 Azure 上做过小规模测试，发现磁盘延迟的影响远大于最大 IOPS；并提醒 vCPU 其实是线程，所以 ARM64 上是 4 个 SMT1 核，而 Intel/AMD 上是 2 个 SMT2 核。作者据此指出一个方法论要点：huge pages 在 RDS 上默认"开启"，而他测的自建默认是 "try"（等同于关闭），会另行说明并单独评测。

- **mattlong** 强烈建议加入 Optimized Reads 实例类型（如 r8gd、m8gd）——它们带本地 NVMe SSD 作为网络磁盘前的缓存，对于"数据集远大于内存的读密集负载"是巨大的胜利。**Rafuino** 两次提醒测试遗漏了 AMD 实例（M8a、R8a、C8a 等），并指出近期多数 AMD 实例已关闭 SMT，即 1 核对 1 vCPU；作者承认为控制首发范围做了裁剪，会在下一版补上。

- **TurdF3rguson** 反映了一个可用性问题：RPS 里的 "r" 到底指什么（行？记录？请求？）。作者澄清是"每秒请求数"，并接受了"加信息徽标"的建议。这位用户还追问"请求是否等同于查询"，并分享自己"一台 8 美元 VPS 上的 80GB 库跑 10 QPS"的经验，点出性能强烈依赖具体查询。

- **handfuloflight** 提问：如果写入变得重得多（比如记录 agent 操作），设计该如何变化？作者回应，重写场景有很多基于 LSM 树的专用数据库，但他的观点是应先摸清 Postgres 这类"易用型数据库"的极限——"人们很容易低估 Postgres 能做到的程度"，只有确实不够用时再转向 LSM 树方案。**crudgen** 和 **sysguru2046** 分别希望扩展到 Azure/GCP 以及 Supabase 对比。
