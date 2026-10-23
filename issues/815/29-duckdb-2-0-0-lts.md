---
layout: article
title: "DuckDB 2.0.0 LTS「肉桂水鸭」发布"
issue: 815
number: 29
category: data
original_url: "https://duckdb.org/2026/10/21/announcing-duckdb-200"
hn_url: "https://news.ycombinator.com/item?id=50181136"
date: 2026-10-23
---

## 文章摘要

DuckDB 团队于 10 月 21 日发布 2.0.0 LTS 版本，代号「Cinnamon Teal」（肉桂水鸭，一种分布在美洲西部、羽色红棕的鸭子）。官方称这是迄今最大的一次功能发布，自 1.5 以来积累了将近 2 万次提交。如果说 1.0 的主题是稳定和向后兼容，2.0 就首先是一个功能版本。2.0 是长期支持版，至少支持到 2027 年 10 月；上一个 LTS 版本 1.4 支持到 2026 年 11 月 17 日；为了让用户平稳过渡，1.5 的支持期也延长到 2027 年 3 月，相当于也成了 LTS。CLI 和 Python 客户端已经可用，R、Rust 等客户端和部分扩展要晚几天。

**DuckDB 也能当服务器了。** DuckDB 从诞生起就是进程内数据库。5 月的 1.5.3 引入了 Quack 协议和 quack 扩展，让任何 DuckDB 进程都能对外提供数据库服务。2.0 带来了打磨过的 Quack，性能和稳定性大幅提升，并新增 `CONNECT` 语句，一条命令就能挂载并切换到远程数据库：服务端调用 `quack_serve` 并设置令牌，客户端 `ATTACH 'quack:localhost'` 后 `CONNECT` 即可直接查询。服务器默认只监听本机，要对外提供服务，官方建议放在终止 TLS 的反向代理后面。`CONNECT` 不限于 Quack，也能指向 PostgreSQL、MySQL 等远程库，新的远程下推优化器会把查询直接发给远端执行，而不是把整张表拉过来。CLI 还新增了 `-serve` 和 `-connect` 参数，一行命令就能起服务端或连上去。

**性能。** 2.0 为 Parquet、CSV 和 DuckDB 原生存储格式引入了异步 I/O，I/O 层与查询处理层独立扩展，在后台发起读取并保持多个请求同时在途，远程读取的并行度大大提高，在 S3 等对象存储上的查询明显加快。

**VARIANT 成为一等公民。** VARIANT 类型用来存放 JSON 这类半结构化、无模式的数据，同时保留列式执行的性能，可以理解为「加强版 JSON」：每行的数据形状可以不同，但它不是文本格式。1.5 已经引入 VARIANT，2.0 则做了完整集成：自动识别数据中的公共结构并「切碎」存储（shredding），压缩好、执行快，从存储层的切碎执行到 Parquet 的读写全链路打通，并配有一组 `variant_*` 函数。

**CLI 的智能体模式。** Claude Code、Codex、Cursor 等编码智能体越来越多地替用户驱动 DuckDB CLI。2.0 的 CLI 能检测自己是否被智能体调用，并切换到智能体模式：输出紧凑的 Markdown 表格而不是带填充的框线表格，明确标出结果是否被截断，及早终止失控的查询，以 JSON 格式报错，耗时较长的查询会先报告预估成本，让智能体决定要不要等。在 TPC-H 实验中，模型需要读的 DuckDB 输出减少了 59%。可以用 `-agent` 手动开启，或用 `-no-agent` 关闭。

**全新的 SQL 解析器。** DuckDB 一直用从 PostgreSQL 派生的解析器，它由 YACC/Bison 生成 LALR(1) 解析器，随着 DuckDB 的 SQL 方言不断扩充，一点小改动都可能与现有规则冲突，产生移进/归约冲突。2.0 默认改用自研的基于 PEG 的可扩展解析器（1.5 时可选启用），PEG 按书写顺序尝试候选规则，新增语法不会再意外破坏其他规则，Python 3.9 也是出于同样原因改用 PEG。扩展现在可以直接挂接语法本身，未来会出现提供全新 SQL 语法的扩展；错误信息也带有精确的源码位置。需要时可以通过 legacy_parser 扩展切回旧解析器。

**扩展生态。** 新的 C API v2 能力强得多，把文件系统、表函数等组件暴露给扩展，目前仍在征求反馈。组织还可以注册和托管自己的可信扩展仓库：一个仓库由名称、URL 前缀和一个或多个用于签名的 RSA 公钥组成，前缀可以是本地路径、HTTPS、S3 等，创建时 DuckDB 会拉取并固定公钥，打印 SHA-256 指纹供带外核对，这项功能语法仍可能变化。核心扩展方面，DuckLake 发布 1.1，Iceberg 扩展朝功能完整推进，Delta 扩展支持建表、分区和时间旅行。

**SQL 新特性。** 2.0 支持触发器：BEFORE 和 AFTER、按行和按语句触发、通过 `REFERENCING OLD/NEW TABLE` 引用变更前后的数据、每个事件多个触发器等，部分组合暂不支持。典型用途是审计表；官方还演示了用插入和删除两个触发器配合 `MERGE INTO` 增量维护按国籍统计平均年龄的表，称这让 DuckDB 离路线图上一直挂着的物化视图又近了一步。方言方面，新增 `$x` 简写代替 `getvariable('x')`，可修改数据的 CTE 让 INSERT、UPDATE、DELETE、COPY 能作为管道步骤，`NEAREST` 连接把 top-k 相似度搜索变成连接子句，便于向量和嵌入场景，支持嵌套模式、JSON 修改函数、SQL 标准的 `FETCH FIRST`，并明确了多行匹配时 `MERGE` 和 `UPDATE ... FROM` 的语义。

**迁移与内部变化。** 破坏性变更不多，最可能出问题的是存储格式：2.0 之前的版本无法读取 2.0 创建的数据库文件，但 2.0 仍能读写旧文件，旧文件也仍可被旧版本读取。官方建议先在影子系统上测试，关键业务可以等 11 月的第一个补丁版本。编译需要 C++17（此前长期停留在 C++11），发布构建默认改用 Clang。时区、日历和排序规则不再依赖 ICU 库，改为自行实现，时区数据直接从 IANA 数据库生成并压缩到约 45 KB，行为不变且更快。旧的单箭头 lambda 语法 `x -> x + 1` 默认禁用，改用 Python 风格的 `lambda x: x + 1`，2.1 将彻底移除；把全部源码打包成单个头文件和单个源文件的 amalgamation 构建被弃用，因为体积已大到不实用；macOS x86_64 逐步淘汰，2.1 起不再支持。

## HN 评论精华

本帖 457 分、62 条评论，气氛几乎一边倒地正面，主要是用户分享实际使用经验、讨论服务器模式和异步 I/O，以及大家都拿 DuckDB 做什么。

**高核数机器上的性能提升。** RobinL 说 2.0 在 EC2 的高核数机器上快了很多，不只是从 S3 读数据，计算本身也是：以前大约到 64 个 vCPU 就不再提速，现在能在 128 到 192 个 vCPU 上跑满并真正转化为更短的完成时间。这让他们的开源记录链接工具 Splink 能在 10 分钟内处理 10 亿条记录、100 亿次记录比对，据他所知是同类中最快的。dicey 问是否还有人用 Spark，RobinL 说仍用 Spark 的人多半是受平台限制（比如在 Databricks 上难以在大机器上跑 DuckDB）或者更熟悉它，而不是因为它更快，对能自由选择环境的新用户，他们总是推荐 DuckDB。gopalv 解释异步 I/O 通常会顺带推高 CPU 占用，因为瓶颈从磁盘移到了 CPU 和内存带宽，扫描速度提高后其他算子单位时间里也有更多活干。RobinL 说核数超过某个点后，增加 CPU 反而会因并行开销让总耗时变长；gopalv 猜测可能是 AWS Nitro 在所有核心同时忙碌时导致较高的 CPU steal，建议用 `taskset` 空出一个核，32 核以上他都这么做，还能保证 SSH 响应顺畅。

**服务器模式与触发器。** paoliniluis 认为能以客户端-服务器架构运行是颠覆性的，会取代很多要靠大量服务器支撑的数据栈；bambax 补充这个「服务器」本质上仍是进程内的，所以依然轻量简单；jtbaker 已经在想把它放在 HTTP 服务和负载均衡后面横向扩展，甚至在 Cloud Run 上缩容到零。datadrivenangel 对触发器最兴奋，说 DuckDB 真正接近「盒子里的数据仓库」了，zazuke 说在源数据库不支持 CDC 时触发器仍然很好用。nicoritschel 已经在生产环境用 alpha 版做一个小组织的集中分析服务器，他写了一个扩展让 DuckDB 支持 Postgres 线协议和 Arrow Flight，因为目前支持 Quack 的客户端还很少；paoliniluis 分享了一个（vibe coding 写的）支持 Quack 的 Metabase 驱动，nicoritschel 提醒随 2.0 发布的 Quack v3 与 v1 不兼容。

**异步 I/O 的小插曲。** fragebogen 问异步 I/O 是否也覆盖 DuckDB 自己的格式，adrian_b 引用文档说暂时没有；实现这项功能的 DuckDB 开发者 pdet 现身说原生格式也支持。adrian_b 回应文档里还写着「将来实现」，还发现发布公告的那句话似乎是后来才补上「以及 DuckDB 原生存储格式」的。pletnes 问异步 I/O 对 CSV 和 Parquet 有什么好处，jtbaker 和 skeeter2020 解释主要在有网络延迟、一次查询要读很多文件（比如用通配符匹配一批 Parquet）时，工作线程不再阻塞等数据，带宽利用率更高。

**大家用 DuckDB 做什么。** mikkelam 说很想用但在公司找不到场景，引来一串回答：tcdent 用它分析 Rust 性能分析器输出的 Parquet 文件；skeeter2020 说如果用户的数据只有几 GB 甚至几 MB，可以直接把全部数据交给他们本地查询，绕开行级权限等安全难题；gopalv 借鉴 Google Capacitor 的数据排序技巧，把两三百万行塞进 16 到 20 MB，在浏览器里用 duckdb-wasm 查询，用户自带 CPU，3000 人同时在线也不需要算力；llimllib 提醒在 duckdb-wasm 中想用 HTTP 范围请求，需要关闭 `builtin_httpfs`，否则可能拉下大量数据，tomjakubowski 说这正好解决了他这周在 DuckLake 上排查的性能问题。chatmasta 让 Claude 定时工作流把仓库数据拉进嵌入了 DuckDB 的仪表盘；thewisenerd 每天把云厂商几十万行的成本导出从 CSV 转成 Parquet 做报表；AlfeG 在 .NET 单体应用里嵌入 DuckDB，因为 MSSQL 跑那些查询太慢；Nihilartikel 在 DBeaver 里用 DuckDB 把磁盘上的 CSV 和 Excel 文件当表来连接查询。jiggawatts 说他上周用 2.0 在一台中等虚拟机上不到两小时就把 7 TB 的 Azure NDJSON 日志压缩成 Parquet，只剩原来的 2.5%，然后做成一个「冻结的 DuckLake」，把签名的 blob URL 直接发给安全分析师，不需要服务器也不需要 Splunk，唯一遗憾是不支持 KQL；nicoritschel 说有了新的 PEG 解析器，这正适合做成扩展。

**能当事务数据库吗。** skybrian 问在小项目里拿 DuckDB 当主数据库跑事务会有多糟。netcraft 说只要单写入者就能走很远；dbettin 引用官方文章称 DuckDB 从第一天起就是带完整 MVCC 和事务隔离的多连接数据库，在不少负载上足以和 PostgreSQL 竞争；Nnnes 找到官方基准：批量传输比 PostgreSQL 快约 30 倍，8 线程以内的小写入略快，但之后 PostgreSQL 继续随线程扩展，DuckDB 则趋于平稳。aynyc 附上并发文档链接，强调限制是单写入者且单读取者，throwawaythekey 认为这与 DuckDB 的设计初衷相反，只适合玩玩，steveshreeve 则说他们已经在这么做了。
