---
layout: article
title: "全新 Mac Studio：M5 Max 与 M5 Ultra"
issue: 807
number: 13
category: featured
original_url: "https://www.apple.com/newsroom/2026/08/apple-introduces-new-mac-studio-with-m5-max-and-m5-ultra/"
hn_url: "https://news.ycombinator.com/item?id=49433316"
date: 2026-08-28
---

## 文章摘要

苹果于 2026 年 8 月 25 日发布新款 Mac Studio，搭载 M5 Max 和全新 M5 Ultra 芯片，官方定位是「端侧 AI 与极限专业工作流的终极桌面机」。硬件主管 Johny Srouji 称这是「有史以来最强大的 Mac」，关键在于把 Neural Accelerator（神经加速器）直接集成进每一个 GPU 核心，从而大幅加速矩阵乘法。

规格方面：**M5 Max** 版本为 18 核 CPU（6 个 super core + 12 个性能核）、最高 40 核 GPU、最高 128GB 统一内存、614GB/s 内存带宽；**M5 Ultra** 版本为最高 36 核 CPU（12 super + 24 性能核）、最高 80 核 GPU（苹果史上最强 GPU，首次为 Ultra 芯片带来 Neural Accelerator）、最高 512GB 统一内存、1.2TB/s 内存带宽（比上代高 50%）。苹果宣称 M5 Ultra 的峰值 AI 算力是 M3 Ultra 的 4.3 倍、M1 Ultra 的 9.8 倍；多线程性能比 M3 Ultra 高 1.3 倍，图形性能提升 1.8 倍。

苹果这次几乎把整篇新闻稿的重心押在了本地大模型上：512GB 统一内存加 1.2TB/s 带宽意味着「完全在设备上运行巨型模型、隐私完整，不必数 token，也不用担心云成本上涨」。更值得注意的是**多机集群**：借助 Thunderbolt 5 和内置的 RDMA（远程直接内存访问）支持，多台 Mac Studio 可以组成共享内存池，四台集群的推理速度可达单机的 3 倍。软件侧，macOS 引入了名为 **Core AI** 的全新框架，用于在 Apple silicon 上构建、运行和部署模型，配合开源的 MLX 框架和 Xcode 组成端到端的 AI 开发平台。

其他升级包括：基于 PCIe Gen 6 的新一代 SSD 架构，存储性能翻倍；最多 6 个 Thunderbolt 5 端口（单口 120Gb/s）；首次通过苹果自研 N1 芯片带来 Wi-Fi 7 和蓝牙 6；支持通过 USB-C 做 genlock 同步；最多驱动 8 台显示器或 4 台 5K 120Hz 的 Studio Display XDR；M5 Ultra 的媒体引擎编解码模块数量是 M5 Max 的两倍，可同时播放 33 路 8K ProRes 422 30fps 视频。系统预装即将发布的 macOS 27「Golden Gate」，带来 Siri AI 和一系列 Apple Intelligence 功能。

**价格与上市**：M5 Max 版起价 2499 美元（教育版 2299 美元），M5 Ultra 版起价 5499 美元（教育版 5099 美元），8 月 25 日起在 30 个国家和地区接受预订，9 月 22 日开始发货，**512GB 内存配置要到 10 月底才供货**。苹果同时低调更新了搭载基础版 M6 的 Mac mini（最高仅 32GB 内存、170GB/s 带宽）。

## HN 评论精华

823 分、554 条评论。讨论几乎全部围绕两件事：**内存太贵**，以及**桌面机是不是该重新取代笔记本**。真正谈视频剪辑、3D 渲染等传统专业工作流的声音反倒稀少——这条产品线在 HN 眼里已经彻底变成了「本地跑大模型的机器」。

**内存价格是最大痛点。** blints 算账：256GB 要一万美元，512GB 大概得翻倍。marcuskaz 指出 256GB 选配加价就要 4000 美元。mythz 说「164GB 内存升级收 4000 美元实在离谱」，打算先买台 M6 Mac mini 过渡，「基本上放弃 AI 主权的野心，先骑着补贴过的云端 LLM 价格熬两年」。nico_h 直言「等不及泡沫破了」。epolanski 认为多收 4000 美元「就是在薅用户羊毛，因为苹果知道会有很多人买单」。也有反方：dist-epoch 对比 NVIDIA RTX 6000（96GB / 1.7TB/s）售价 13000 美元，认为「256GB / 1.2TB/s 的 Mac 竞争力极强，肯定会到处缺货」；seanmcdirmid 甚至觉得「在今天疯狂的 DRAM 市场里，一万美元买 256GB Ultra 便宜到不真实」。SwellJoe 则冷静指出，这只说明「在跟 AI 专用机比的时候苹果税不存在」，但仍打算等「RAMpocalypse（内存末日）」结束再买。

**为什么没有 1TB？** gizajob 觉得「不给挥霍者或 VC 烧钱的人留个 1TB 选项很奇怪」。tristor 的分析最有代表性：他已有 128GB 的 M5 Max MBP，512GB 只能多跑「几个」有意思的模型，真正的分水岭在 1TB——那才够 4bit 量化跑 1T 参数以上的模型，「512GB 卡在『刚好不够』的位置，正是最令人沮丧的点」。alberth 给出一个精巧的推测：Ultra 由 16 颗基础芯片拼合，而今天同时发布的基础版 M6 上限仍是 32GB，32×16=512GB，所以 512GB 可能会是 Ultra 长期的天花板。dannyw 认为根本原因是 NAND/DRAM 不够分：768GB 的内存够造近 100 台 iPhone 17。petercooper 补充苹果确实在供应吃紧，「订 128GB 的 MBP 要等六周以上」。wmf 冷冷地说：这些「规则」都是人为的，苹果只是不想给。

**最长的一条子线程是「Mac Studio + 瘦客户端」。** joshstrange 提问「下一台电脑是不是该买 Studio 而不是一台永远插着扩展坞的 MBP」，引出了几十条真实配置分享。共识出奇一致：**Tailscale 把家里的 Studio 变成随处可达的算力，笔记本退化成终端。** appplication、herpdyderp、emp_、geophile 等人都在用这套方案；znpy 直接说「第一次我的个人笔记本上没有任何开发工具，只装了 ghostty 和 openvpn」。super_mario 提供了热设计的实证：满配 M3 Max MBP 跑本地 LLM 会过热、电池因高温衰减且风扇吵，换成 M4 Max Mac Studio 后「gpt-oss 120b 整天跑都没有噪音」。try-working 一句话概括这波风向：「笔记本干不了 agentic engineering，会烫得要死、电池瞬间见底。」sanderjd 说得更本质：「钟摆摆回了桌面机——过去没有什么有用的计算量是笔记本装不下的，现在有了。」

反对意见也不少：ThouYS 用过 Mac mini 后劝人三思，「MacBook 是一个太完整的包裹：好音箱、好键盘、好屏幕、指纹识别，要凑齐这些得一堆外设」。spockx 抱怨「想要好屏幕就必须买贵 CPU」。jmalicki 和 chocrates 都问：为什么不用便宜的 Linux 工作站配一台 Mac 笔记本？epolanski 表示自己 2022 年攒的 Ryzen 台式机性能更强、价格只有 Ultra 的三分之一。Aurornis 提出了一个常被忽略的技术反驳：Mac 大内存看着诱人，但 **prefill（预填充）速度比真 GPU 慢太多**，「除非你能让它后台跑 24 小时不着急，否则我现在再也不在大内存 Mac 上跑大模型了」。kristianp 也认为苹果宣传的「一台 Ultra 给办公室当 LLM 服务器」有误导性，token/s 撑不起商业场景。

**性能估算**：GodelNumbering 给出了较靠谱的推算——1.2TB/s 来自两颗 M5 Max（各 614GB/s）通过 4.4TB/s 的片间总线互联，跑非量化的 Deepseek V4 flash 大约能有 1000+ tokens/s 的 prefill 和 50+ tokens/s 的生成，「这已经相当可用，接近云端水平」。

**对苹果本身的吐槽。** nalekberov 说「苹果成了 AI 淘金热里的新铲子贩子」，担心接下来是软件变差加硬件更难维修。ghostly_s 敏锐地注意到：新闻稿里以前满是碾压竞品的性能图表，现在只剩桌面截图，「通篇没有一句和竞争对手比较的性能声明」。osmukka 数出「up to（最高）」一词出现了 46 次，读得「几乎生理性难受」。mannanj 觉得这类新闻稿本身就像「AI slop」。speedping 打趣标题里有个 em dash，「不知道是不是 AI 写的」。drnick1 老调重弹：为什么不做一台标准 ATX 塔式机？jdeaton 只留下五个字：「可惜跑不了 Linux。」lvl155 则说，如果苹果允许一等公民级的 Linux 支持，这会是绝佳的家用服务器。

**时机。** FinnLobsien 和 jbverschoor 都点出这次发布的政治意味：John Ternus 一周后接任 CEO，而他此前正是硬件工程 SVP，「我不讨厌苹果重新变成一家做好电脑的公司，而不是拼命推订阅」。stephen_cagle 半开玩笑：如果规格属实，17000 美元买一台 512GB / 1.2TB/s 的机器对小型办公室是笔好买卖，「Anthropic 的 IPO（传闻在十月）来得再快也不嫌早」。kylehotchkiss 说得更直白：「我已经用 ollama 跑了超过一百万条 prompt，现在准备本地上大模型。我一分钱都不想再多给 Dario 了。」calini 的总结获得了不少共鸣：「为什么我只有两个肾。」
