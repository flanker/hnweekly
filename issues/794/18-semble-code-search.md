---
layout: article
title: "Show HN：Semble——给 Agent 用的代码搜索，省 98% 的 token"
issue: 794
number: 18
category: show_hn
original_url: "https://github.com/MinishLab/semble"
hn_url: "https://news.ycombinator.com/item?id=48169874"
date: 2026-05-22
---

## 文章摘要

**Semble** 是 MinishLab 团队开源的一个**给 AI agent 用的代码搜索库**。卖点直接写在 README 第一句：**比 grep + read 少用 98% 的 token**。它的基准显示，要在一份大型代码库上达到 85% 的召回率，grep+read 需要约 100k token，而 Semble 用 2,000 token 就能达到 94%。

它不需要 API key、不需要 GPU、不需要外部服务，**纯 CPU 本地跑**。一份普通仓库的索引时间约 250 毫秒，单次查询约 1.5 毫秒——比传统的代码专用 Transformer 模型快 218 倍，但在 NDCG@10 指标上能达到 0.854，相当于它们 99% 的质量。

**架构是混合检索（hybrid retrieval）。**两条路并行：(1) **语义检索**——用 Model2Vec 框架的 potion-code-16M 静态嵌入做向量相似度；(2) **词法检索**——用 BM25 在标识符和 API 名称上做关键词匹配。两组结果通过 **Reciprocal Rank Fusion (RRF)** 融合，再叠一层针对代码的重排信号——查询是符号就给词法路径加权、查询是自然语言就保持均衡；定义比引用排得更高；标识符做词干化（camelCase 和 snake_case 互相匹配）；同一文件多个 chunk 命中给整体加分；测试桩、legacy 兼容层、空声明则被降权。

部署上 Semble 既可以作为 **MCP 服务器**接入 Claude Code、Cursor 等 agent 平台，也可以作为 CLI 给 bash 脚本或子 agent 调用。整个项目用 Python 写、跑在 CPU 上、模型只有 16M 参数——本质是一个**轻量、纯本地、不依赖任何外部服务**的代码语义索引器。

## HN 评论精华

- **freakynit**（最被点赞的反驳）：贴出一组亲测结果，结论是**这种工具反而会让 agent 变笨**——任务是追踪某个中型项目里的写入和搜索路径，三组对照：(1) 用 codebase-memory-mcp：85k/4.4k token；(2) 自己的极简方案：67k/3.2k；(3) 什么都不用：80k/3.2k。Semble 类工具反而最贵。他给的"极简方案"很有意思：在 AGENTS.md 里只写一行"先读 PROJECT.md"；PROJECT.md 里手写一份项目地图——简介 + 关键文件一句话描述 + 结尾让 LLM 在改动后自行更新。**人工写的 80 行胜过所有花哨索引。**

- **jerezzprime**：道出了所有这类工具的根本困境——"模型被 RL 训练得**严重偏好 grep**，对其他形式的结果不信任，会不断重试或重读，**所有 token 节省都被这个不信任循环吃光了**。"他建议 Semble 团队做的不是基准测试，而是去 Claude Code / Copilot CLI 上**禁用 grep 只留 Semble**，跑端到端 agent benchmark。

- **esperent**（独立做了 eval）：用 Pi + GPT 5.5 测了 RTK on/off + headroom on/off 四种组合，3 轮就停下来了——"**两个都关掉效果最好。**"上下文用量确实降了，但需要的对话回合数升了，**总成本反而更高**。他对整个领域的态度是："这种工具的人很多在炫 retrieval 质量，但那不重要。重要的是——**agent 用了它以后能不能更便宜更快地完成同样质量的工作？**"

- **aadishv**（最严谨的对比测）：在 browser-use/browsercode 仓库上跑了同一个问题，强制只用 Semble vs 只用 rg+fd——结果：**不用 Semble 反而稍便宜**（10.9% ctx / $0.144 vs 9.8% / $0.172）。回答质量大致相当。

- **boyter**（同行）：他在做 [cs](https://github.com/boyter/cs) ——一个**不建索引**的"更聪明的 grep"，性能优先，加入代码结构感知和排名。已计划用 Semble 做对照基准。他猜测 cs 更适合"authenticate --only-declarations"这种精确符号查询，Semble 更适合"auth 是怎么工作的"这种自然语言查询。

- **jelder**：抛出更老派的观点——**LSP 早就比 grep 强很多了**，对人对 LLM 都是。问题是 Claude 把 LSP 当二等公民对待，得在 prompt 里专门提醒"用 LSP 不要用 grep"才肯切换。

- **andai**（小项目派）：在小代码库上 Claude 经常花一堆时间到处搜，**直接把整个目录 dump 进上下文反而最便宜**。他做了一个启动钩子直接 dump 全树，跳过"在黑暗里乱摸"环节。

- **kurtextrem**（领域综述）：链接了 entire.io 的研究——"**单纯把搜索做快收益有限，把搜索结果排序做好才能让 agent 早点找到对的代码**"。同时列了一长串相邻工具：Morph WarpGrep（有免费层）、codemogger、cs、colGREP、mgrep（mixedbread）、osgrep、codedb。这个赛道现在拥挤得几乎每天有新选手。

- **wrxd / nextaccountic**：另外两个值得关注的方向——**maki.sh** 用真正的语法解析器代替 grep 来抽 chunk，能更省 token；**colgrep** 走完全不同的色彩通道索引路线。

整体来看，HN 的共识不是"Semble 不好"，而是"**所有这类 retrieval 工具都需要拿出真正的 end-to-end agent benchmark，否则 grep 的简单路径很难被打败**"。
