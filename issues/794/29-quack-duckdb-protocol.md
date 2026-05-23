---
layout: article
title: "Quack：DuckDB 的客户端-服务端协议"
issue: 794
number: 29
category: data
original_url: "https://duckdb.org/2026/05/12/quack-remote-protocol"
hn_url: "https://news.ycombinator.com/item?id=48111765"
date: 2026-05-22
---

## 文章摘要

DuckDB 一直以"嵌入进程内（in-process）"为卖点——数据库就在你的 Python / Node / 浏览器 Wasm 进程里跑，没有网络开销。但这套模型有个硬伤：**多个进程不能并发写同一份数据库文件**。当你要把可观测性数据集中写入一个文件、同时还想让多个仪表盘并发查询时，就卡死了。**Quack** 就是 DuckDB 团队对这个问题的官方回答——**"DuckDB 实例之间可以用 Quack 远程协议互相对话了"**。

设计上有几个有意思的选择。**协议直接架在 HTTP 上**——不是 TCP 自定义二进制，而是普通 HTTP，意味着可以白嫖现成的反向代理、防火墙、负载均衡器、认证系统。最关键的是，**浏览器里的 DuckDB-Wasm 可以直接连到远程 DuckDB 服务端**，省去了一整层 gateway。序列化格式没用 **Arrow Flight SQL**（虽然这是大数据圈的默认选项），而是用了一套基于 DuckDB 内部 primitives 的私有格式 `application/duckdb`——理由是不想被外部标准绑死、自己的格式可以快速迭代。

**性能上 Quack 的优势体现在两处**。第一是单次小查询：Arrow Flight 强制两次往返（先拿 schema 再拿数据），Quack 只要一次请求-响应，**延迟减半**。第二是批量传输：把 6000 万行结果搬走，Quack 用了 **4.94 秒**，Arrow Flight 用了 **17.4 秒**，PostgreSQL 用了 **158 秒**。小写入并发 8 线程时 Quack 跑到 5434 TPS，超过 PostgreSQL；继续往上加线程则被 DuckDB 自身的并发插入上限卡住。

**安全默认值**也走的是"先能用再开放"路线：随机生成的认证 token、默认只 bind 到 localhost、SSL 走外层反向代理。整套设计的哲学是把 DuckDB 从"嵌入式分析引擎"扩展到"轻量级数据基础设施"——既保留原来的本地速度，又能接入观测、共享、并发等场景，不需要切到 ClickHouse 或 Postgres。

## HN 评论精华

- **aduffy**（赶巧需求）：上周刚在抱怨这个问题——用 Deno + DuckDB 接传感器数据，想用 `duckdb -ui` 看数据就得停掉 server。Quack 完美解决。顺便表白："DuckDB 是我 2025/26 年最爱的技术"，已经渗进 LLM 工作流、数据分析、ETL 全套流程。

- **steve_adams_86**：详细解释怎么用 DuckDB 当**多源数据胶水**——把 Sentry、AWS CloudWatch、GitHub 的数据全 dump 到同一个 DuckDB 实例，用纯 SQL 跨源 join 排查故障时序。其他工具能做到同样的事，但都要配 Docker、装客户端、学 DSL，"只需要 SQL"这点让他几乎再也不开别的工具。

- **simlevesque**（困惑派）：DuckDB 现在到底想做什么？SQLite 的边界很清晰，但 DuckDB 一会儿是本地引擎、一会儿是 DuckLake、一会儿又有 Quack 服务端协议，路线图越来越散。**wenc** 反驳：把 DuckDB 类比成 SQLite 看就懂了——SQLite 也是从手机扩展到 iot、浏览器、桌面 app。DuckDB 是"驱动现代数据栈的引擎"，做出这些扩展是顺理成章的，**核心还是那个本地超快的查询引擎**，CLI 和 Python 库照样用得舒服。

- **rglover**：直接吐"卧槽这正好解决了我们内部应用框架"——之前犹豫 DuckDB 怎么做水平扩展，现在答案有了。顺手夸了一句 "Quack" 这个协议名取得"很 DuckDB"。

- **smithclay**：在做基于 parquet 的可观测性存储项目，一直对 Apache Iceberg 的易用性有意见，但又想拥抱开放格式。Quack 让 DuckLake 路线对他变得很有吸引力，**可能能替代 Mimir 或 Clickstack**，运营复杂度大幅下降。

- **philipallstar**（一句话点题）：如果 SQLite 哪天也加客户端-服务端协议，大家也一样会问 "SQLite 到底想干嘛"——但其实没什么好奇怪的，**只是工具的覆盖面在扩张**。
