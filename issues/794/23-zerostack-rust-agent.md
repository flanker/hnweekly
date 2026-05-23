---
layout: article
title: "Zerostack——8MB 内存的 Unix 风格 Rust 编码 agent"
issue: 794
number: 23
category: code
original_url: "https://crates.io/crates/zerostack/1.0.0"
hn_url: "https://news.ycombinator.com/item?id=48164287"
date: 2026-05-22
---

## 文章摘要

**Zerostack** 是 gi-dellav 用纯 Rust 写的一个**极简编码 agent**——核心定位是 Pi 和 OpenCode 的精神继承者，但在资源占用上做到极致。代码量约 9000 行，单二进制 **8.9MB**，**空闲会话约 10MB RAM**，工具调用时 CPU 占用约 1.5%（相比之下 JS 系 agent 占 20%+，长时间运行的 OpenCode 会泄漏到 6GB）。**比 JS 替代品小约 30 倍**。

**架构上**是典型的 Unix 哲学组合：
- **LLM 集成层**：多提供商支持——OpenRouter / OpenAI / Anthropic / Gemini / Ollama
- **权限系统**：四档可配置模式，从最严格到 yolo
- **会话管理**：持久化对话存储，自动 compaction
- **TUI**：Crossterm 实现，支持 Markdown 渲染和鼠标
- **可选特性**：MCP 支持、Git worktrees 集成（按任务建分支）、ACP 协议（接入 Zed 等编辑器）、Exa 联网搜索、loop 系统做长 horizon 迭代任务

设计上有几个被作者特别强调的点。一是 **"doom-loop detection"**——检测 agent 反复重复破坏性操作时自动打断，避免 agent 把 repo 自己删了。二是 **prompts 系统**——通过 slash command 在运行时切换 "code / plan / review / debug" 等模式，不用维护一套套独立 skill。三是发布节奏惊人——HN 帖时是 1.0.0，本周已经迭代到 1.3.0。许可证 GPL-3.0。

安装一行——`cargo install zerostack`，可选 `--features acp` 接入 Zed。

## HN 评论精华

- **noodletheworld**（最被点赞的元评论）：抛出一个时代之问——"**Agent harness 是新时代的 web framework 吗？**"——人人想写一个、上手简单、做到生产级却极难、赛道上铺满半成品。但他也补充："**这一个做得挺好——work 得起来，而且 raison d'être 表达得很清楚。**"

- **slopinthebag**（中肯的赛道观察）：他认为编码 agent 不难——"就是 TUI + 工具 + agent loop"——**真正难的是支撑各种 provider 的怪癖**。有意思的是这个赛道在主动做实验——有人塞一堆工具、有人只给一个 sandboxed Python 解释器让 agent 自己写脚本、有人极简只用 bash。他个人想要的是**让用户掌舵更多、agent 自动化更少**的 harness——"少一点 agent，多一点 augmentation"——但他没找到，可能得自己写。

- **rullopat**（直击痛点的反问）：**"我懂特定场景需要小内存，但**——对一个绝大部分时间都在调 LLM 然后等回应的软件来说，性能优化的意义到底在哪？"**这是个根本性的问题——agent 的瓶颈是 LLM API 延迟和模型本身，不是 Rust 还是 Node。

- **throwa356262**（反方）：直接打 rullopat 的脸——"**Claude Code 在低端笔记本上吃几个 GB 内存真的很烦**，8MB 这个数字让我开心。"老款 MacBook / Chromebook / Linux 上跑 agent 的人完全感同身受。

- **360MustangScope**（同病相怜）：今天本来正要开始自己写一个 Rust agent——"**OpenCode 在大项目上慢慢泄漏到 6GB 然后越变越慢真的让人抓狂**。"Zerostack 直接救了他周末。

- **wkcheng**（早期用户反馈）：试用后觉得确实快。但报告了两个兼容性问题——(1) Azure 上的 GPT-5.5 用 OpenAI 兼容接口失败，因为 `max_tokens` 已被替换为 `max_completion_tokens`；(2) 没法传 custom headers，所以 DeepSeek 的 `reasoning_effort` 参数发不出去。

- **frio**（同道中人）：在自己业余写一个类似项目——目的有二：深入理解 agent + 学 Rust。他特别想保留 Pi 的"**自我变异、自动生成新工具**"能力——他不喜欢给 agent 开 bash（即使开 edit + cargo run 也等价于 RCE）；他的解法是**让 agent 在遇到没工具的情况时按需生成新工具**，而不是给它一个万能 bash。

- **parhamn**（更极端的极简派）：分享了自己 200 行写的 [nano](https://github.com/pnegahdar/nano)——repl、session、approvals 全有。他抛出了一个让赛道老兵思考的话："**模型越聪明，harness 越无关紧要**（除了 DX 之外）。"

- **zbyforgotp**（理论派）：指出一个深层问题——"我们不信任 LLM 执行，所以加用户审批；但任务分解需要 code 和 prompt **共递归**，这意味着审批应该能在任意深度被发起。"他建议参考 Qubes OS 的虚拟机间剪贴板协议，给递归 agent 设计一个**审批传递协议**。

- **sergiotapia / hiAndrewQuinn**（实操党）：呼吁这个赛道需要正经的 **agent benchmark**——claude code / codex / opencode / pi / zerostack 横向对比。同时 hiAndrewQuinn 报告自己把代码扔给 DeepSeek v4 Flash 跑了一遍 audit，没发现可疑代码——9000 行 Rust 小到一次能审完。

- **arjie / tontinton / khimaros**：评论区还冒出了好几个**自己写编码 agent 的人**——arjie 用 Claude Code 加 Dirac 行哈希做了一个、tontinton 有 [maki](https://github.com/tontinton/maki)（MIT 协议，跟 Zerostack 几乎同形态）、khimaros 的 [airun](https://github.com/khimaros/airun) 是无 TUI 的可管道版本，能识别 Claude Code / Pi / OpenCode 的 skill 和 prompt 模板。这条 HN 帖几乎变成了**Rust 写编码 agent 的同好认识大会**。
