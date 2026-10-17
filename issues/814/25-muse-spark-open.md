---
layout: article
title: "Meta 发布 Muse Spark Open"
issue: 814
number: 25
category: data
original_url: "https://research.meta.ai/blog/muse-spark-open"
hn_url: "https://news.ycombinator.com/item?id=50107379"
date: 2026-10-16
---

## 文章摘要

10 月 15 日，Meta 在研究博客上发布了 Muse Spark Open 的权重。这是一篇篇幅不长的产品公告，标题直接给它定了位：「迄今为止能力最强的美国开放权重模型」。

**模型规格。** Muse Spark Open 是原生多模态的混合专家（MoE）模型，总参数 8160 亿，每个 token 激活 420 亿，上下文窗口 100 万 token。Meta 说它主要面向编程和智能体任务，擅长长周期任务、复杂软件工程和多步推理。基准测试部分只放了一张图表，对比对象是 Inkling、Nemotron 3 Ultra、Beam、Mistral 4 Large、Qwen3.8-2.4T-A95B、GLM 5.3 和 Kimi K3，覆盖智能体、知识与推理、编程、工具辅助多模态几类评测，详细方法另有一份评测报告。值得注意的是，对比表里全是开放权重模型，没有 OpenAI、Anthropic 等闭源前沿模型。

**主打卖点：两台 Mac Studio 就能跑。** 公告用了大量篇幅讲 Apple Silicon 适配。Meta 称借助最新版 MLX 框架，Muse Spark Open 是同级别中第一个能在桌面上运行的模型，所需硬件是两台搭载 M5 Ultra 的 Mac Studio。原理是苹果的统一内存架构：每台 M5 Ultra 的集成 GPU 最多可用 512 GB 内存，两台加起来能装下整个模型；芯片 1.2 TB/s 的内存带宽足以读取每个 token 只需要用到的那一小部分参数（这正是 MoE 稀疏激活的好处）。于是两台 Mac Studio 合起来就像一台拥有 1 TB 统一内存的「超级计算机」。Meta 说他们完全在本机上测试了一系列智能体工作流：用基于该模型的 Muse Code 给开源 Mastodon 客户端 Ice Cubes 提交真实的 iOS 修改（修 bug、加功能、完整构建），以及端到端操控 DaVinci Resolve，让模型逐帧审看素材、挑出最精彩的片段并裁成竖屏。

**分发渠道铺得很广。** 托管 API 方面有 Baseten、Fireworks AI、Groq、Modal、Nebius、OpenRouter 和 Together AI；权重可在 Amazon SageMaker AI、Google Cloud Model Garden、Hugging Face 和 Microsoft Foundry 下载。推理引擎首日支持 SGLang、vLLM、TensorRT-LLM、Hugging Face Transformers、Apple MLX、llama.cpp 和 Unsloth，并针对 AMD、苹果、英特尔和英伟达芯片做了优化。Meta 还随模型在 SGLang 里提供了可自动调优的 Helion GPU 内核，在英伟达和 AMD GPU 上带来 1.03 到 3.95 倍的内核级加速，在 AMD MI350 上端到端提速 3%。想进一步微调的开发者可以用 PyTorch 的 TorchTitan。

**安全与许可。** Meta 表示，开放权重有其特殊风险，因此对这个版本额外做了安全后训练，并按照「Meta 超级智能扩展框架」评估了最严重的几类风险，结果达到了发布门槛，详情见模型卡，完整评测报告稍后公布。许可证是「Muse 模型许可证」，带有「有限的商业条款」，Meta 的说法是这能让许多开发者免费使用。公告把这次发布放进 Meta「把个人超级智能带给每个人」的叙事里，开头一句是「我们相信未来属于每个人，包括在家里运行先进模型的自由」。

## HN 评论精华

这个帖子拿到 901 分、460 多条评论，是本期最热的条目之一。讨论主线有五条：「每个人」到底是谁、许可证算不算开放、实际性能与中国模型的对比、Meta 的商业动机，以及「美国最强」这个限定语本身。

**「未来属于每个人」，前提是你买得起两台 Mac Studio。** 这是被嘲讽最多的一句话。mcphage 算了一笔账：一台 256 GB 内存的 M5 Ultra Mac Studio 大约 1.1 万美元，512 GB 版本还没开卖。joshstrange 估计整套下来约 3 万美元，ryanschaefer 的估计是 5 万。devy 指出差不多的钱可以买一块 H100，带宽是 Mac Studio 的三倍多，还追问 Meta 是怎么拿到尚未发售的 512 GB 机型的；dannyw 解释苹果一向会在严格条件下给开发者提前供货，bigyabai 补充说 PyTorch 本来就是 Meta 的。sorenjan 借《发展受阻》的台词挖苦：在年薪 25 万美元以上的人堆里待久了，就会觉得桌上摆两台 512 GB 的 M5 很正常，「一根香蕉而已，能多少钱？10 美元？」markstos、octomind、brisket_bronson 都在不同角度重复了这个吐槽。tidbeck 和 sourcecodeplz 提到推理速度大约 20 token/秒；GeekyBear 引用苹果机器学习研究博客的说法，四台 Mac Studio 通过雷雳 RDMA 组网能跑到每秒 36 个 token 以上。felineflock 列出 Unsloth 量化版本的体积：最低的 1 比特量化 180 GB，Q4 版本 510 GB，「依然超出普通发烧友的能力」。

也有人为这一点辩护。bahmboo 认为 Meta 真正做了在多个平台上测试和编写部署文档的工作，抬高了开放权重发布的标准，不是「扔个文件让社区自己琢磨」，对专业市场来说两三万美元的硬件不算什么。TomGarden 说这是他第一次看到大实验室明确宣布为消费级硬件（虽然很贵）做优化，希望以后能有针对 128 GB Mac 的模型。adrian_b 则认为大模型哪怕只能从 SSD 流式读取权重、每秒一个 token 也有用，可以白天用小模型快速迭代，晚上让大模型跑一夜检查结果。

**这到底算不算「开放」。** popinman322 指出许可证要求大型推理服务商单独找 Meta 签授权，这会削弱公共服务商之间的价格竞争，就像之前 Kimi K3 那样。isityettime、PunchTornado、tintor、ShinyLeftPad 等人坚持：禁止部分商用的许可证就不是开源，顶多算「权重可得」。seizethecheese 去读了许可证原文：只有当你把模型作为「模型即服务」（通过 API 等方式向第三方提供模型输出）出售，且公司及关联方 12 个月总收入超过 2000 万美元时，才需要通知 Meta 并另签协议；内部使用、研究、以及把模型作为自家产品功能的一部分都不受限制。fhd2 和 ithkuil 据此认为条款「没那么苛刻」，SSLy 说这跟 GLM 的条款差不多，reilly3000 称之为「有点开放洗白，但对训练方和公众都算公平」。dannyw 还提出一个正面理由：OpenRouter 上不少服务商跑的是降级或配置错误的模型，单独授权有助于保证质量。cosmojg 则从美国版权法角度提出，模型权重是自动优化过程的产物，未必受版权保护，由此引发了一长串关于「权重是不是事实」的法律争论，overfeed 等人反驳说权重凝结了大量人类的设计与判断。adrian_b 实际下载时发现，Hugging Face 上是需要填写单位和职位、等待 Meta 审批的「门控模型」，他的申请大约十分钟获批。

**跑分不如预期，被中国模型压着。** GodelNumbering 引用 Artificial Analysis 的数据：Muse Spark Open 的智能指数为 39，与 DeepSeek V4.1 Flash、Mimo 2.5 Flash 相当，落后于 GLM 5.3 Flash 和 Qwen3.8-Flash-Next。onlyrealcuzzo 接话说它和 DeepSeek V4.1 Flash 差不多，运行成本却高出约 10 倍。bel8 贴出对比：Muse Spark 1.3 为 45 分，Open 版只有 39 分。a11r 说表现不如那两个能在单块 B300 上跑的中国模型，Meta 在开放权重领域还得追赶。redox99 批评它用的是过时的注意力机制，100 万 token 时光 KV 缓存就要 200 GB 以上，称其「非常令人失望」。halJordan 和 sharktheone 提到扎克伯格此前承诺要开源 Muse Spark 1.2，结果给的是这个，算是「货不对板」。mmaunder 认为对比表里没有 DeepSeek 4.1 Flash 是「巨大的危险信号」。AustinDev 在自己的本地基准上测了，首 token 延迟、吞吐和并发都不如 GLM 5.3 Flash，得分也更低。eknkc 也觉得它比 GLM 5.3「笨」，但在 fireworks.ai 上速度很快。tintor 提到 Meta 过去有模型被曝刷榜，所以已不再信任 Meta 的任何跑分。

正面评价也有。jjcm 做了「图片转 HTML」测试，认为它是他见过最好的开放模型之一，虽然没完全按设计稿来，但抓住了神韵。scirob 发现在德语基准上它目前是第一名，所以「西方最强」对西方语言用户确实有意义。simonw 照例放出了不同推理档位下的「骑自行车的鹈鹕」SVG，有人调侃 Muse 模型显然没有针对鹈鹕刷过榜，也有人回一句「它们也不怎么好就是了」。

**「美国最强」这个限定语。** sorenjan 觉得很讽刺：一家顶级美国科技公司在宣传里只敢说「美国最好」而不是「世界最好」，因为它落后于中国。sergdigon 注意到这已成趋势，一周前 Mistral 的说法也是「显著超越任何美国或欧洲开发的开放权重模型」。zkrhass_ 的总结是，Meta 和 Mistral 意识到不必再打败 DeepSeek，只要做「我被允许使用的模型里最好的那个」就能赢得客户。dcchambers 认为这确实满足了一个细分需求：拒绝使用中国模型、又想自建推理的美国公司。Ntrails 不理解为什么自己部署的开放权重模型还要看产地，反正它不会「打电话回家」；mattmaroon 猜测 Meta 身处诉讼射程内，大概没有做蒸馏，等于被「道德」束缚了手脚，aaronharnly 回应说，认为蒸馏别人的模型不道德、拿受版权保护的材料训练却完全没问题，这种立场正是前沿实验室一直在鼓吹的。

**Meta 的商业动机。** swalsh 认为扎克伯格当年 Llama 的判断正越来越被证实：对智能收租的市场并没有出现，只有掌握最前沿模型的几周里能收租，过了一定能力门槛后价格才是决定因素；他还断言 Muse「非常有黏性」，Meta 会赢下消费级 AI。这句话招来大量反驳：nater5000 说 Muse 出现的时间太短，根本谈不上黏性；casualscience 说它不过是套了 Meta 品牌的 OpenClaw，AI 助手没有社交网络那种网络效应；有人直接质疑他是 Meta 的托。binlog 反问：如果对智能收租的市场没出现，OpenAI 和 Anthropic 合计约 1000 亿美元的收入从哪来？由此又引出一场关于 Anthropic 是否真的盈利的争论。2001zhaozhao 提出一个更细的观点：所有个人 AI 智能体都有黏性，因为价值沉淀在难以导出的记忆里，但 Muse 缺少网络效应，朋友都用它并不构成你也用它的理由。solyin 则指出 Muse 应用目前登顶 iOS 下载榜，超过 ChatGPT、TikTok 和 Claude，在 HN 的圈子之外，不讨厌 Facebook 的普通人觉得它很好用。至于为什么要免费放出权重，评论里的猜测包括：防止某一家独占数据、赢家通吃（onlyrealcuzzo），搅乱领先者的利润率（baron3dl），以及「气一气 Dario」、接住被 Anthropic 疏远的用户（hypfer）。

最后，也有不少人单纯表示欢迎：karahime 很高兴 Meta 重新全力投入开放权重，dcchambers 认为开放模型是确保所有人公平获得 AI 的唯一途径；但 binlog 讽刺说，科技圈一年 364 天都在骂扎克伯格，等他拿出点亮闪闪的东西，又争先恐后排队去领。
