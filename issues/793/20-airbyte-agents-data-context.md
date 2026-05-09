---
layout: article
title: "Airbyte Agents：跨多数据源给 Agent 提供上下文"
issue: 793
number: 20
category: show_hn
original_url: "https://news.ycombinator.com/item?id=48023496"
hn_url: "https://news.ycombinator.com/item?id=48023496"
date: 2026-05-08
---

## 文章摘要

Airbyte 联合创始人兼 CEO Michel Tricot 在 HN 上 Show 出 **Airbyte Agents**——他们把多年做 ETL 连接器（700+ 数据源）的积累，转向给 AI Agent 用的"上下文层"。Tricot 的核心论点：当前 agent 失败的根因不是推理弱，而是**在能开始推理之前，根本不知道哪里有什么数据**。"agent 通常需要先发现什么是相关的，才能开始推理。"

Airbyte Agents 不是又一个 MCP 套壳的薄包装，而是一个**为 agentic search 优化的数据索引**：Airbyte 现成的复制连接器把 700+ 数据源的内容预先抽进一个 Context Store，agent 在调用上游系统前可以先在 Context Store 里做结构化搜索；当确实需要写入或拿实时数据时，再回退到原系统直接读写。文章里的真实案例：让 agent 回答"哪些客户处于流失风险"——直接走原始 API 需要 47 步且大多失败；走 Airbyte Agents 的预索引后步骤大幅压缩。Token 成本对比也很直接：相比社区版 MCP，Zendesk 节省最多 90%，Gong 节省最多 80%，Linear 节省最多 75%；Salesforce 因为原生 SOQL 已经够好，只省 16%。

技术架构上，Airbyte Agents 提供 REST API、SDK 和 MCP Server 三套接口；动作动词不仅有 ETL 的"复制"，还包括 get/set/list/update/upload 等。凭据管理走单点 OAuth，避免给每个 agent 配几十组密钥。后端把 filter 谓词下推为 SQL 在云端执行；目前对外暴露 JSON 谓词而非 raw SQL，作者解释是为了避免被锁死在某个 SQL 方言上。社区里关于这个架构的讨论非常激烈：核心分歧是"**预索引 vs. 实时查询**"以及"**该不该让 agent 直接写 SQL**"。

## HN 评论精华

- **"这其实就是 MCP gateway"**（**swyx**）：前 Airbyte 员工的视角——产品本质是在 MCP 层之下补足"AI 工程师不懂数据工程"这道鸿沟。AI 应用对数据有大量需求，但 AI 工程师普遍缺 ETL/索引知识，Airbyte 的连接器积淀正好填补。

- **"把数据放到 agent 面前还不够"**（**jessewmc**）：他指出真正的难题在 *what you index, and how you signpost*——索引哪些字段、用什么元数据让 agent 能"看懂"，这是个上下文工程问题。**aaronsteers**（Airbyte 团队）回应说他们利用了已有的连接器元数据来给字段打标签，并允许用 SDK 自定义 tool。

- **"为什么不直接给 agent SQL？"**（**andai**）：直接拷问——SQL 是数据领域最锋利的工具，干嘛兜个 JSON 谓词？作者回应当下用 JSON 是为了 SQL dialect 兼容性，未来可能开放。**sho（Gentility.ai）** 进一步主张"give agents the sharpest tools you can"，他们的做法是把 API 数据同步进 DuckDB，让 agent 直接 SQL 查。这条评论代表了**"DuckDB + 直查"派**的反对路线。

- **"恭喜你做了个 ETL，叫它 agent"**（匿名调侃）：HN 经典戳穿——这个产品的"agent"标签，本质就是 ETL 加一层语义检索 API。但有人反驳说**正因为 agent 时代才让大家重新意识到 ETL 是基础设施**。

- **跨系统实体匹配**：好几位评论者指出真正的难题不是单一数据源——是 Salesforce 的 customer、Zendesk 的 ticket、Stripe 的 subscriber 怎么对得上？这正是 Airbyte 已经积累的 schema mapping 能力的延伸价值。

- **API 收费墙的隐忧**：评论区也提到一个商业风险——SaaS 厂商（Notion、Slack、Atlassian）正在加速给 API 加 agent 收费墙，Airbyte Agents 作为代理商如何应对反爬反 agent 的趋势，会是产品长期能不能成立的关键。
