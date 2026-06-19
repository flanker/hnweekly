---
layout: article
title: "Datasette Apps：在 Datasette 内托管自定义 HTML 应用"
issue: 798
number: 22
category: show_hn
original_url: "https://simonwillison.net/2026/Jun/18/datasette-apps/"
hn_url: "https://news.ycombinator.com/item?id=48593731"
date: 2026-06-19
---

## 文章摘要

Datasette Apps 是运行在 Datasette 实例上、被严格沙箱隔离的 iframe 内的独立 HTML + JavaScript 应用。它让开发者能够构建自定义界面，在使用 Datasette 传统 JSON API 能力的同时，访问持久化的关系型数据库。

这些应用通过客户端 JavaScript 对 Datasette 数据库执行只读 SQL 查询，并可通过预先配置好的存储查询（stored queries）获得可选的写入能力。正如文章所述，它们是「运行在严格受限 `<iframe>` 沙箱中的自包含 HTML+JavaScript 应用」。

安全架构采用了多层防御：iframe 沙箱阻止访问 DOM、窃取 cookie 或操纵 localStorage；内容安全策略（CSP）头限制对外部网络的请求和资源加载；MessageChannel 传输确保页面跳转后命令通道随即关闭；权限控制将基础的应用创建权限与危险的 CSP 配置权限相分离。一次 AI 安全审查曾发现一个严重漏洞：无特权用户可诱骗管理员访问恶意应用从而窃取数据，修复方式是将 CSP 域名白名单限制为仅受信任的工作人员可用。

主要特性还包括：可见的日志记录（捕获 SQL 查询与 CSP 违规以便调试）、通过管理员审定的存储查询实现安全的数据库写入、面向 AI 的友好设计（提供可复制的提示词用于 LLM 辅助生成应用），以及让 Claude 能够以对话方式创建和编辑应用的 Agent 集成。这一模式把 Claude Artifacts 式的交互能力与持久化数据访问结合起来，可在已认证的 Datasette 环境内构建更丰富的数据探索、可视化与操作界面。

## HN 评论精华

- **fsuts**：为不熟悉的读者补充了 Datasette 的定义，称它是「一个用于探索和发布数据的工具」，面向记者、策展人和研究者。
- **pietz**：分享了自己类似的项目 caipi.ai，让 Agent 通过 MCP 编辑、配合浏览器中的交互界面来构建数据驱动的应用。
- **anitil**：谈到自己以前用 Datasette 的 JSON 接口，认为这种方式比把应用和数据分散到多个系统中更能让二者保持在一起。
- **simonw**（作者）：回应对文件系统存储的关切，表示计划支持把应用以 Git 版本管理的方式存到磁盘上，而不仅仅存在数据库里。
- **euroderf**：好奇为何至今没有框架能简单地用 SQLite 表、控件和实时更新来填充 HTML，认为 WASM 版 SQLite 有望被广泛采用。
- **joren-**：提到自己构建的 Pihka，同样用 SQLite 把数据与展现保持在一起，并生成分面搜索界面和客户端数据层。
