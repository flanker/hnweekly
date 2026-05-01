---
layout: article
title: "Show HN：一个 Karpathy 风格的 LLM Wiki，由你的 agent 们维护（Markdown + Git）"
issue: 792
number: 18
category: show_hn
original_url: "https://github.com/nex-crm/wuphf"
hn_url: "https://news.ycombinator.com/item?id=47899844"
date: 2026-05-01
---

## 文章摘要

[WUPHF](https://github.com/nex-crm/wuphf)（项目名取自 *The Office* 里 Ryan 的同名"未来通讯应用"梗）由作者 **najmuzzaman** 开发，定位是一个**面向 AI agent 团队的协作办公环境**，核心组件是一个由多个 agent 共同维护、以 Markdown + Git 为底层"事实之源"（source of truth）的 wiki。它对外宣称的灵感来自 Andrej Karpathy 多次提及的"LLM-native 知识基底（LLM-native knowledge substrate）"构想 —— 即一个 agent 既能读、也能写的活体知识层，让上下文跨 session 累积，而不是每天早上重新粘贴一次。

**为什么走 Markdown + Git 而不是 Postgres + pgvector + Neo4j + Kafka**：作者明确表示这是**故意往回退一步**，看看在不上向量库、不上图数据库、不上消息队列的前提下，纯 Markdown + Git 这种"file-over-app、可读、`git clone` 即可带走"的方案能撑到哪。整个 wiki 物理存储在 `~/.wuphf/wiki/`，全部走标准 Unix 工具链可读（`cat`、`grep`、`git log`）。

**整体数据架构**：
- 每个 agent 拥有私有 notebook：`agents/{slug}/notebook/.md`
- 共享团队 wiki 在 `team/`
- 实体级别的事实日志：`team/entities/{kind}-{slug}.facts.jsonl`，append-only JSONL；每攒够 N 条事实，由一个 synthesis worker 调用 LLM 重写出一份"实体简报（brief）"
- synthesis 提交在 git 历史中以一个独立身份 **"Pam the Archivist"** 落库，与人/agent 的提交区分开，溯源清晰
- 检索层：**Bleve（BM25）+ SQLite**。SQLite 存事实、实体、边、redirect、supersedes 等结构化元数据。**目前不上向量库**，作者设了一道发布门槛 —— 在内部 500 artifacts × 50 queries 的基准上，单 BM25 召回@20 必须 ≥ 85%；如果某类查询掉到 85% 以下，就 fallback 到 sqlite-vec
- 每条 fact ID 决定性可重算（包含句子 offset），canonical slug 一旦分配就不再改名，merge 通过 redirect stub 完成；rebuild 是逻辑等价而非字节等价

**Draft → Wiki 推升机制**：notebook 里写的内容是私有草稿，必须经过 review（agent 自审或人审）才能 promote 到 canonical wiki，并自动加 back-link。底层是一个小型状态机，驱动条目的过期与自动归档。这一层是整个系统对抗"agent 写得太多 / 写出 confidently wrong 内容"问题的核心防线 —— 没经过 promotion 的内容不进入团队检索。

**质量与一致性工具**：
- `[[Wikilinks]]` 支持，断链以红色高亮
- 每日 lint cron：扫描 contradictions、stale entries、broken wikilinks
- `/lookup` slash command 与一个 MCP tool：短查询走 BM25，叙事性问题走 cited-answer 检索环路

**与已有方案对比的独特点**：
1. 不是 Mem / Letta / MemGPT 那种基于向量 + RAG 的 agent 记忆系统
2. 不是 Obsidian + 插件 —— 因为 Obsidian 是单用户编辑器，没有"agent A 起草、agent B 推升、团队批准"这种状态机概念，且需要一层 MCP surface 给 runtime 调用
3. 不是 CLAUDE.md / AGENTS.md 那种单个静态文件 —— wuphf 关注**多 agent 之间的事实合并、重命名追溯、引用链**

**安装**：`npx wuphf@latest`，MIT 协议，自托管，自带 keys。运行时支持 Claude Code、Codex、OpenClaw、本地 LLM via OpenCode。HN 帖里给出的 5 分钟 asciinema demo 演示了"录入 5 条 fact → 触发 synthesis → 调用用户 LLM CLI → Pam 身份提交 git" 的完整链路。

**作者声明的局限**：召回调优仍在进行；synthesis 质量被 agent 观察质量限制（"垃圾输入 → 垃圾简报"，lint 只能减轻不能消除）；目前是单办公室作用域，没有跨办公室联邦。

## HN 评论精华

- **mellosouls** 第一时间贴出了 Karpathy 那条引发讨论的原 Twitter 链接（[archive 镜像](https://xcancel.com/karpathy/status/2039805659525644595)），算是给整个项目的"理论出处"做了脚注。

- **dhruv3006**：质疑 Markdown 在"durability（耐久性）"上到底强在哪。**left-struck** 的回答简洁有力："Markdown 文件极度开放、易找软件、简单、广泛使用 —— 这几条加起来几乎能保证它在很远的未来仍然可读可用。" **dhruv3006**："那确实，Markdown 在未来的分发上会很有优势。"

- **najmuzzaman（作者本人）回应"为什么不用 Obsidian Vault + Plugin"**：给出了两条结构性理由 —— (1) Obsidian 是单用户编辑器，没有"agent A drafted、agent B promoted、team approved"这种状态机概念；插件可以模拟，但 source of truth 必须是 agent 直接对话的进程，而不是 vault 文件。(2) Agent 需要一个 MCP surface，`/lookup`、`entity_fact_record`、`notebook_write`、`team_wiki_promote` 是 MCP tool，由 agent runtime 直接调用；Obsidian 插件 API 面向人类用户和 Electron app，等于要你重写一遍 MCP 层。但他承认兼容并存可以："你完全可以让 Obsidian 把 `~/.wuphf/wiki/` 当成 vault 来读，已经有 Reddit 上的用户这么干了，Obsidian 当 reader、wuphf 当 writer。"

- **mncharity**（TiddlyWiki 原作者 jermolene 在子评论中现身确认身份）：抛出一个有趣的对照视角 —— TiddlyWiki 这个"用单个 HTML 文件实现可自我修改 wiki"的项目已经存在 20+ 年，社区也在探索 LLM 工具链。jermolene 还在做一个叫 [twillm](https://github.com/Jermolene/twillm) 的项目，使用 TiddlyWiki 的 Node.js 配置，把 tiddler 存为独立文件，可以直接和 Claude Code 这类工具同时工作在一个 Markdown vault 上。他指出 TiddlyWiki 有几个对 Karpathy LLM-Wiki 模式特别契合的优势：(a) **计算视图**取代物化的索引文件 —— Karpathy 的方案依赖一个 `index.md`、需要 LLM 自己保持同步，"这正是 LLM 不擅长的事，跨 session 的 staleness 会越来越糟"。TiddlyWiki 的 view 是渲染期实时计算的 filter expression，比如"标签为 concept 的条目按评分排序"，永远不会过期。(b) **frontmatter 即可查询的结构** —— Obsidian 把 YAML frontmatter 渲染成顶部一个盒子；TiddlyWiki 把它提升为一等公民字段，可 filter / sort / aggregate。(c) LLM 不仅写内容，还能写 wikitext tiddler 形成小的交互式 live view（dashboard、glossary、index）。

- **renan_warmling**：提出系统性短板 —— "缺少快照（snapshots）和每次文件迭代的 data enrichment。代码出 bug 时 agent 应该能 rollback 并生成解释 rollback 原因的 enrichment、形成新快照。另外 opinion weight、规则一致性、agent 间冲突避免都没解决。最后是 **temporal vs atemporal memory** 的区分缺失 —— 业务规则、软件行为、安全策略、agent 治理这些东西不应该和会话级临时记忆混在一起。"

- **johntash**：尖锐地指出 LLM-wiki 类项目的通病 —— "怎么让 LLM 别写得太多？我自己造过几套类似系统，全都太容易被 LLM 写出一堆东西，最后系统越大越难用。" 他举了个例子：让 LLM 自己造知识 wiki、给链接让它去研究、distill 成具体页面 + 来源引用，"看起来很美，直到你真的去读它写的内容。"

- **coldtea（与子评论 voting 思路）**："一个实操方法是把 capture 层和 promotion 层分开。Agent 可以自由起草，但要进入 trusted 状态需要人审。一些团队用投票机制 —— 多个 agent 独立总结同一来源，只在它们收敛时才 promote。'confidently wrong' 问题会随时间恶化，因为坏条目会被其他 agent 引用，最后你的知识库就装满了自信的 BS。"

- **manfre**：分享了自己写的 LLM-wiki 实践 —— Claude 插件 + Obsidian Web Clipper + qmd 搜索，主要用来记住自己访问过的随机网页（[博客链接](https://manfre.me/posts/2026/04/build-llm-wiki-obsidian/)）。说明 LLM-wiki 已经成为一个小生态，不只是 wuphf 一家。

- **psteinweber**：提了一个很多 agent 协作工具忽视的需求 —— **多人类用户支持**："像 paperclip 这种工具不支持多人，但对我们这种小型、agent 辅助的团队来说，'多个 marketing 角色的 agent 互相对话、明确定义的 human-in-the-loop 决策'恰恰是最重要的特性。"

- **gbram**：顺手安利了类似产品 [hivemindai.dev](https://hivemindai.dev) —— "面向多团队的 agent 协调层，跨用户/团队共享，只捕获关键决策和动作，不存中间所有杂项。"

- **kaoD**：从一个邻近赛道进来 —— "看起来很像 [Hurl](https://hurl.dev/)，区别是？"（这条评论其实是从 najmuzzaman 提到的另一项目 Voiden 衍生出来的支线讨论，但也反映了 HN 用户对"基于 markdown 的工作流"普遍敏感。）

- **hyperionultra / spiderfarmer / wiseowise**：一条偏题但反复出现的支线 —— 对 Karpathy 本人的"信徒文化"提出怀疑。**mirekrusin** 反讽回应："这就像因为一个音乐家狂热于乐器而讨厌他。" 这条侧支讨论提醒读者，"Karpathy-style"标签本身在 HN 上既是号召力也是争议点。
