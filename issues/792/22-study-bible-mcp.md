---
layout: article
title: "Show HN：Study Bible MCP —— 学术级希腊语/希伯来语词典与形态分析"
issue: 792
number: 22
category: show_hn
original_url: "https://github.com/djayatillake/studybible-mcp"
hn_url: "https://news.ycombinator.com/item?id=47949856"
date: 2026-05-01
---

## 文章摘要

Study Bible MCP 是由 djayatillake 构建的一个 Model Context Protocol 服务器，把学术级别的圣经研究工具暴露给 Claude、ChatGPT、Cursor、Windsurf、Cline 等支持 MCP 的 AI 助手。它的核心定位不是"在聊天里查经文"，而是把神学院级别的语言学和释经学资源结构化地交给 LLM，让模型在回答时基于原文文本而非检索到的二手解释。

资源层非常硬核。希腊语侧集成了三部主要词典：完整的 Liddell-Scott-Jones（LSJ）古希腊语词典，覆盖整个古典语料；Abbott-Smith 专注新约希腊语的词典；以及 Strong's concordance。希伯来语侧提供完整未删节的 Brown-Driver-Briggs（BDB）词典。每个条目包含定义、词源、出现频次、神学意义。文本层接入了 TAGNT（带形态学标签的希腊语新约）和 TAHOT（带形态学标签的希伯来语旧约），所有词都附带 Strong's 编号与形态分析——动词时态、语态、语气、人称、性数格等都可以逐词查询。再上层是经节级的英译与学术注释。

服务器一共暴露 18 个工具，可走远端 SSE，也可走本地 stdio，无需 API key、无需下载语料库。作者还把 Gordon Fee 与 Douglas Stuart 的经典释经学著作《How to Read the Bible for All Its Worth》中的解释原则编码进了 prompt 框架里——也就是说在使用过程中模型会被引导先看历史背景、文学体裁、作者意图，避免"断章取义（proof-texting）"和过度寓意化（allegorization）。这种对释经学的"显式约束"是普通 LLM 圣经助手最缺的部分。

## HN 评论精华

由于 HN 评论页抓取受限，下面的整理基于可获得的讨论片段以及对项目本身的延伸分析。

- **正面评价**：有评论者反馈"它的回答在神学上是站得住脚的，因为它从原文出发并走过一套明确的释经流程"，这正是作者刻意设计的差异化——不是直接让 LLM"自由发挥"，而是把方法论硬编码进工具调用顺序。

- **关于"上下文"**：另一位用户表示日常请它讲解某节经文时，"它总会把这一节放回所在章节的上下文里看"，这是规避"prooftext"问题的具体表现。

- **使用场景**：多位用户每周用它备讲道、回答神学问题，并在自己的信仰社区里互相推荐。这与传统 BibleWorks/Logos 这类付费桌面软件的用户路径接近，但门槛被压到了"装一个 MCP server 给 Claude Desktop"。

- **作者的愿景**：原作者写到"我希望每个基督徒都能用上这套工具，以一种健全的方式探索圣经"，强调访问平等——把过去只有神学院学生和牧者能用的语言学资源开放出来。

- **数据来源讨论**：有评论者询问 Strong's concordance 等词典是怎么集成的，是否有进一步扩展的空间。这指向一个本质问题——这类工具的长期价值取决于底层语料库（LSJ、BDB、TAGNT、TAHOT 等）的覆盖与版权状态，而非 LLM 本身。

- **延伸分析**：从工程视角看，这是 MCP 协议在"领域知识 + 强方法论"场景下一个很合适的范式——LLM 提供自然语言理解和推理，MCP 服务端提供专业、可信、可被引用的数据通道。比起 RAG 直接喂文档，"调用 18 个结构化工具"显然能让回答可追溯、可复核，这对宗教文本这种对解释立场极其敏感的领域格外重要。

- **潜在质疑**：把特定的释经学框架（Fee/Stuart 的福音派立场）写进系统 prompt 也带来一个问题——它在事实上为模型选定了一种解释传统，对希望中立或采用其他释经传统（天主教、东正教、犹太教或学术批判派）的用户来说会有内置偏向。这是后续可改进的方向，例如把"释经传统"做成可切换的 profile。
