---
layout: article
title: "Show HN：Llmspeed，买硬件之前先感受本地大模型跑得有多快"
issue: 815
number: 16
category: show_hn
original_url: "https://llmspeed.dev/"
hn_url: "https://news.ycombinator.com/item?id=50127979"
date: 2026-10-23
---

## 文章摘要

想在本地跑大模型的人，几乎都会在买硬件前纠结同一个问题：花几千甚至上万美元买一块显卡或一台 Mac，到底能换来多快的体验？网上的跑分通常只给出一个"每秒多少 token"的数字，而这个数字对大多数人来说很抽象，很难想象 20 tok/s 和 60 tok/s 在实际使用中差多少。llmspeed.dev 就是为了解决这个问题而做的小工具：选一款 GPU 或 Mac，再选一个模型，页面就会按预测的速度把回答一个字一个字地"流"出来，连第一个 token 出现之前的等待时间也一并模拟，让你直观地"感受"这套配置用起来是什么样子。

网站目前覆盖 30 款硬件和 15 个热门模型。硬件从老一代的 RTX 3060、3090、Tesla V100、AMD MI50，到 RTX 5090、RTX PRO 6000、DGX Spark、AMD Strix Halo，再到 Mac Studio M5 Ultra 512GB、MacBook Pro M5 Max，以及纯 CPU 和云端 H100、H200；模型包括 Qwen 3.6/3.8 系列、GLM-5.3、Gemma 4、GPT-OSS、DeepSeek V4 Flash、Llama 3.1 8B 等，后端覆盖 CUDA、Metal、ROCm、Vulkan 和 CPU。其他功能包括：

- **赛跑模式**：两套配置并排对比，比如 RTX 5090 对 Mac Studio。项目首页的演示视频里，Mac Studio M5 Ultra 512GB 在 GLM-5.3 Flash（Q8）上流畅输出，而 5090 根本装不下这个模型。
- **内存适配检查**：显示模型在各种量化精度下能否完全装入显存、是否需要部分卸载到系统内存，或者干脆跑不起来。
- **可分享链接**：任何配置或对比都能生成链接。

根据 GitHub 上的 README，每个估算值都标注了可信程度：**实测**（在这套硬件、模型和量化下真实跑过）、**推导**（根据相似配置的实测数据插值），以及**推算**（例如尚未发布的硬件，只能根据规格计算）。实测数据来自 r/LocalLLaMA、localscore.ai、llama.cpp 讨论区等社区。没有实测数据时，网站退回到第一性原理：生成速度受内存带宽限制，每生成一个 token 都要把激活权重读一遍，混合专家（MoE）模型只算激活参数；提示词处理（prefill）受算力限制；两者都使用按后端校准的效率系数，并随上下文增长而变慢。部分卸载采用混合模型，显存部分按 GPU 带宽、溢出部分按系统内存的有效带宽计算。内存估算则计入权重、KV 缓存和运行时开销，Mac 的统一内存按约 75% 可用、独立显卡按约 93% 可用。

项目以 MIT 协议开源，用 Vite、Preact 和 TypeScript 构建，是纯静态网站。作者欢迎大家用 `llama-bench` 跑出实测结果，以 PR 的形式提交到基准数据表中。README 也坦承了局限：目前只支持单设备，不模拟多卡；驱动、推理引擎版本、flash attention、功耗限制和散热都可能让实际数字偏差 10% 到 30%。

## HN 评论精华

本帖 330 分、80 多条评论。大家普遍认可这个点子，但评论区很快变成了"你的数字不准"的反馈大会，以及本地大模型玩家交流配置心得的场所。作者 albinstman 在评论区积极回应，并在讨论期间上线了新的模型和硬件数据。

**数字准不准？** 这是最集中的批评。pil0u 说他的 M2 Ultra 128GB 实际跑得比网站给 M3 Ultra 512GB 的数字快将近一倍。binary132 在 4090 上用 Qwen 3.8 27B Q4 加三路草稿推测解码能跑到约 80 tok/s，而模拟器只显示二十几。kgeist 的 5090 在小上下文下有 60 到 70 tok/s，网站说 46，加上 n-gram 推测还能破 100，"有很多可以调的旋钮，网站都忽略了"。也有人觉得偏乐观，adrian17 说 5060 Ti 上 Gemma 26B-A4B 给出的峰值不太可信，还指出 KV 缓存的估算比 Sebastian Raschka 网站上的数据小了约 6 倍。tom_alexander 很喜欢这个工具，因为它解决了两个问题：很难判断砸一万多美元买硬件是否值得，以及为了测速把每个模型每种量化都下载一遍对 Hugging Face 这样的免费服务"不太礼貌"；但他指出网站说 9070 XT 跑不了 FP16 的 27B 模型，实际上 llama.cpp 可以把部分层放在 CPU 上跑，而且提示说"没有任何量化能装下"，切换到 Q8 后提示却消失了。bitexploder 批评得最不客气，说他在 RTX 4080 上跑着网站标为"跑不了"的模型，称其为"纯粹的垃圾"。

**软件进步太快，快照会很快过时。** layoric、Grombobulous 都提到推理引擎 Strata 显示出还有大量性能没被榨干，layoric 用两块 16GB 的 A4000 加一台老 X99 双路至强，跑 Qwen 3.8 Flash Next 的 IQ3 量化能达到 65 到 75 tok/s；bitexploder 说他用两块 2017 年的显卡跑到 200 多。bozdemir 在企业里为数百名工程师用 H200 搭建推理服务，他指出整个运行框架（甚至 NVIDIA 驱动版本）都会影响速度，建议标注每个配置使用的库及版本，albinstman 回应说基准数据源通常不包含这些信息。

**真实负载与"感觉"。** friendly_chap 认为对于智能体编程，并行 prefill 的能力远比输出速度重要。Jabdoa2 在公司用 vLLM 的经验是：网上的基准往往能被调参超越，但真实的智能体编程负载通常是 6 万到 14 万 token 的提示加几千 token 输出，解码吞吐只有基准的一半到三分之一，而且更多取决于缓存感知路由和 KV 缓存管理。layer8 提醒，模拟假设的是单个对话，一旦并行处理多个任务，硬件需求就会上升。kinduff 注意到 OpenAI 和 Meta 会用大量动画和淡入效果来掩盖延迟。amelius 希望工具也能让人感受模型的质量，并问为什么推理工具不给 prefill 阶段加个进度条。

**功能请求。** 呼声最高的是多卡支持：gitowiec 希望能选同构或异构多卡、PCIe 代数、NVLink 和通道拆分，Perenti、arcfour、sleight42、Tepix、mirekrusin 也都提到双卡、双 Strix Halo、双 Spark 这类常见配置；albinstman 表示希望将来支持，但要在受欢迎程度和高质量基准数据的可得性之间权衡。其他请求包括加入老显卡（hbogert 的 2017 年 RX580 8GB 也能跑 Qwen 9B）、骁龙 X Elite、NVFP4 量化、搜索硬件、自定义 tok/s 等。kristopolous 和 montroser 则建议干脆在云上租一台试几分钟，花费不到 0.5 美元。

**老硬件玩家的配置交流。** lopotubez 用一台 9 年前、GTX 1070 Max-Q 8GB 的笔记本，借助 llama.cpp 的 Turboquant 分支跑 4 位量化的 Qwen 3.5-35B-A3B 微调版，131k 上下文下仍有 16 到 19 tok/s，他说任何前沿模型都会告诉你这台机器只能跑垃圾 9B 模型，这完全不对，还贴出了完整配置。OtomotO 在 RX 6700 XT 上跑同一模型时让它分析整个项目却原地打转，PaulKeeble、trucks-refinish、montroser 指出是上下文只设了 16k，智能体工作至少需要 64k，可以把 KV 缓存量化到 8 位来腾出空间。PaulKeeble 还认为激活参数少的 MoE 模型在本地硬件上性价比远高于稠密模型，内存将是 MoE 的主要瓶颈。montroser 提醒硬件价格正剧烈波动，3090 半年内涨了一倍，vast.ai 的租价仅 9 月就涨了约 50%。voxleone 则预言大模型会带来本地计算的复兴。
