---
layout: article
title: "Redis 作者新作：用 ds4 在本地运行大模型"
issue: 813
number: 26
category: data
original_url: "https://dwarfstar.sh/"
hn_url: "https://news.ycombinator.com/item?id=49936575"
date: 2026-10-09
---

## 文章摘要

提交的链接 dwarfstar.sh 是 DwarfStar 4（简称 ds4）的社区网站，页脚写明这是「围绕 Salvatore Sanfilippo（antirez）项目的社区站点」，并不是官方主页。本文结合该网站和 GitHub 仓库 antirez/ds4 的 README 整理。

**它是什么。** ds4 是一个用 C 写的推理引擎，范围刻意收得很窄，目标是在人们自己买得起的硬件上跑好少数几个顶级开源权重大模型。最初只做 DeepSeek V4 Flash，现在支持 DeepSeek V4 / V4.1 Flash（含实验性视觉模型）、DeepSeek V4 PRO、GLM 5.2/5.3 与 GLM 5.3 Flash，以及 Qwen3.8 Flash Next。它不是通用的 GGUF 运行器，只能用项目自己产出的 GGUF 文件，模型加载、提示词渲染、工具调用、KV 状态、HTTP 服务和编程智能体都在一起集成测试。README 说模型支持是「机会主义」的：哪个开源模型最适合 128 GB 笔记本或 256/512 GB 工作站就跟进哪个，有更好的替代品时旧模型可能会被移除。

**主要技术选择。** 第一是非对称 2-bit 量化：对 MoE 模型的路由专家做激进压缩，共享的关键路径保持高精度，网站的说法是「压缩了，但没切掉脑子」，这样 2840 亿参数的 DeepSeek V4 Flash 也能放进高内存机器。第二是把 KV 缓存当成磁盘上的正式数据：长前缀按提示词的 SHA1 哈希存到 SSD，重启后能直接恢复，不用重新 prefill。第三是一个引擎、三种用法：`./ds4` 交互式命令行，`./ds4-server` 提供兼容 OpenAI 和 Anthropic 风格的本地 API（Claude Code、Codex CLI、OpenCode、Pi 都能接），`./ds4-agent` 是原生编程智能体，直接用模型的原生工具格式，会话可以保存、切换、恢复。此外还有 SSD 流式加载（内存不够时从 SSD 读权重）、张量并行（两台 128 GB Mac 用 RDMA 互联跑 4-bit 模型）、流水线并行、会话批处理、投机解码（MTP/DSpark）、可调思考强度和「方向引导」（directional steering）等功能。

**硬件与性能。** 主力平台是 96 GB 以上内存的 Apple Silicon Mac（Metal），也支持 NVIDIA DGX Spark 和一般的 CUDA 机器（包括 Ada Lovelace 架构的 L40S 多卡，8 卡 16 会话总生成速度约 126 tok/s），以及 AMD Strix Halo（ROCm）。Qwen3.8 的 Q2 版本主权重约 41.7 GiB，是 64 GB Mac 的入门选择。网站列出的基准里，M5 Max 128 GB 跑 V4 Flash Q2 时，2K 上下文 prefill 约 790 tok/s、生成约 39 tok/s；64K 上下文生成约 28 tok/s。

**AI 开发声明和发布理念。** README 坦白说这个软件是在 AI 编程智能体的大量协助下开发的，人负责想法、测试和调试，并且特意致谢 llama.cpp 和 GGML：ds4 不链接 GGML，但部分量化格式和内核代码是在 MIT 许可下沿用或改写的。antirez 还写了他对 AI 时代软件发布方式的看法：用户可以让编程智能体按自己的硬件深度修改软件，所以项目更应该是覆盖主流场景的「可运行模板」，不必试图支持每一种配置。他鼓励用户用编程智能体来了解和定制这个项目。

## HN 评论精华

**网站本身挨了不少批评**

- **twoodfin** 直接给出 GitHub 地址，说仓库页面更适合 HN 读者。**Almondsetat** 说网站「纯属 slop」，建议管理员把链接换成原仓库。**timmytokyo** 列举了网站标题里的营销腔，比如「Compressed, not lobotomized」「Dense, resident, yours」「Local frontier inference, narrow on purpose」，认为这是典型的 AI 生成网站：三段式排比、「是 X 不是 Y」句式、小号全大写字、过度分节、花哨动画。**aeve890** 拿 antirez 本人朴素的博客做对比，觉得很讽刺。
- **Flere-Imsaho** 持不同意见：虽然 HN 用户不喜欢这种 80 年代复古风加动画的网站，但这一页确实把他需要的信息都讲清了。**LoganDark** 回应说问题不在字体和动画，而是 LLM 生成的落地页都长一个样，只讲项目是什么，不讲用起来什么感觉。ds4 本身比这个页面有意思得多。

**和 llama.cpp 等运行器有什么区别**

- **HoldOnAMinute**、**locknitpicker** 问 ds4 和 llama.cpp、ollama 有什么不同。**pydry** 的第一反应是「llama.cpp 的 vibe coding 仿制品」。**simonw** 解释说，通用运行器要支持所有模型，很容易配错，结果工具调用不正常或者性能不理想；ds4 只支持少数精选模型，但每个都支持得很好，说白了就是「默认值更好」。有人追问，多花功夫调参的话其他运行器能不能达到同样效果，simonw 回答「能」。
- **jminnl** 补充说，硬件组合太多，没法维护一套统一的「好默认值」。光是 llama.cpp 的 GPU 组合、批大小、线程数、张量切分、缓存和投机解码选项排列下来，就能让人花一天以上做基准测试，理想情况是运行器第一次加载模型时自己找出最佳配置。他也认为 llama.cpp 团队得到的认可远远不够。
- **ttoinou** 从 DeepSeek V4 Flash 刚支持时就在用，说这是他在 M5 Max 128 GB 上用过「最好的启动器」，现在用 Qwen3.8 Flash Next 跑了一周多，速度快、上下文很长。他举了一个细节：ds4-agent 只追加、从不改写消息历史，所以 KV 缓存前缀可以一直复用。
- **doctorpangloss** 认为量化这么狠的 DeepSeek V4 模型效果不好。**c0rruptbytes** 说自己的经验相反，ds4 的量化版本在基准上胜过 unsloth 的量化版本，并附上了测试链接。

**为什么用 C 而不是 Rust**

- **GTP**、**simoiacos** 说 antirez 公开谈过：他觉得 Rust 不够顺手，而且 LLM 写 C 写得更好，因为训练语料里有大量高质量的 C 代码；只有安全关键的代码才值得用 Rust。**zozbot234** 不同意：C 程序的安全性和正确性是全局属性，而 LLM 恰恰不擅长推理全局属性，Rust 用局部语法强制模块化，反而对 LLM 更稳妥。**Aeolos** 认为 Rust 完全能写这种底层代码，SIMD、无别名和多线程正确性保证都有，zlib-rs 就比 C 版本快。

**对 antirez 本人的争议**

- **jeffbee** 说「来自 Redis 作者」对他来说反而是个警告。**za_creature** 翻出 Jepsen 对 Redis 的测试文章和 antirez 当年的回应，认为他当年本可以在内存数据库上做 Raft，却自己土法搞了一套复制协议还坚持己见。**r_b_m_k** 提到 Martin Kleppmann 和 antirez 关于 RedLock 的争论，觉得 antirez 对分布式系统过于自信。
- **antirez 本人**回复说，Redis 能流行十多年靠的正是这些设计选择：当时很多系统和公司认为不可线性化就没有用，结果都失败了，所以被批评的地方恰恰是他职业生涯里做得比较好的决定。他还说外界的敌意也和他对 AI 的积极态度有关，并提到 GPT 6 Astra 能轻松写出在真实系统模型假设下可验证 RedLock 安全性的 TLA+ 模型。**jacquesm** 力挺他，说自己审查过的系统里大约 30% 都装着 Redis。

**生态和衍生项目**

- **neomantra** 维护了一个把 ds4 编成共享库的分支，并做了 Go 绑定 ds4go，带 TUI、工具库和 Homebrew 安装。他还花一个周日下午在云上的 AMD MI325X 上调通了 ROCm 构建。他的看法比较冷静：Qwen3.8 在 64/96 GB 上能跑，但现实中最好有 128 GB；本地模型还是不如云端大模型聪明、快，他不建议现在就拿本地模型替代 Opus 这类模型写代码，但 ds4 跑的模型知识面广、思考能力强，他在尝试用它做高层思考，再给更擅长写代码的模型生成提示词。
- **simoiacos** 受 DwarfStar 启发，为 32 GB 内存的 Intel Xe-LP 笔记本写了推理引擎 Xenolith，目前只支持量化版 Gemma-4。**aziis98** 在 Intel Ultra 7 255H 的核显上跑到约 22 tok/s。**ABS** 提到 AMD 在 6 月给 antirez 寄了一台 Strix Halo。
- **westoque** 引用 README 里「软件更应该是覆盖主流场景的可运行模板」这句话，认为这很能代表 AI 时代开源软件的构建方式。**darkwater** 感叹 128 GB 内存的 M5 Max MacBook Pro 现在要 7800 欧元。**trvz** 回了一句：没在 2500 欧时买 128 GB 的 Strix Halo，只能怪自己。
