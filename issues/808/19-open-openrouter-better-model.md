---
layout: article
title: "我们做了一个开源版 OpenRouter，还能把使用流量变成更好的模型"
issue: 808
number: 19
category: show_hn
original_url: "https://github.com/experientiallabs/experiential"
hn_url: "https://news.ycombinator.com/item?id=49471407"
date: 2026-09-04
---

## 文章摘要

Experiential 是 Experiential Labs 开源的一个 LLM 网关（gateway）与路由器，定位是"开源版 OpenRouter"，项目地址在 GitHub 上的 experientiallabs/experiential。作者在 Show HN 自述中把它的价值主张概括为三条：第一，通过一个 OpenAI 兼容的 API 同时访问托管模型、BYOK（自带密钥）模型和本地自托管模型；第二，控制哪些用户和 agent 可以用哪些模型、用于什么场景、能花多少钱；第三，把生产流量变成一个为质量、速度、成本优化过的自定义路由器，甚至训练出一个属于你自己的模型。

技术上，网关是 Rust 原生实现、面向高并发设计的，并且把各家模型和厂商之间那些恼人的配置差异（流式格式、工具调用、模型参数、限流、以及各不相同的错误行为）都抹平了。作者给出的性能数字是：BYOK 请求下网关本身增加的延迟低于 1 毫秒，由 Experiential 提供厂商密钥时低于 2 毫秒。它覆盖了所有主流推理厂商，1000+ 模型列表每天由一个 codex agent 自动开 PR 刷新。

上手方式很简单：pip 安装后运行 exp，首次启动会有一个配置向导，走一遍厂商、模型、推理强度（reasoning effort）的选择，持久化每个厂商连接，然后给出公开别名、身份和默认 50 美元命令预算，最后打印一次性密钥。之后就可以用标准的 OpenAI SDK 或 Anthropic Messages API 调用。也有托管版本 platform.experientiallabs.ai，同样是零加价（no markup）。

最有意思的是"从流量中优化"这一环。做法是：先用 OpenTelemetry 采集现有 agent 的标准化 trace，从中挖掘出有代表性的真实任务；再用文本世界模型（text world models）为各个候选模型模拟 rollout；用 LLM judge 打分；最后在 prompt 的 embedding 之上拟合一个最近邻分类器，来决定每个请求该走哪个模型。作者称这通常能在成本/质量维度上画出比单一模型更好的帕累托曲线，但坦承"并不完美"。基于同样的模拟，系统还能给出缓存命中优化建议、新模型建议，甚至用 Tinker 微调一个你自己拥有的开源模型。仓库明确写了商业模式方向：开源可自部署，托管版零加价，赚钱靠企业授权和托管。

## HN 评论精华

**缓存是最尖锐的技术质疑。** 首条高赞评论来自 **Areibman**：坚持用单一模型的一大好处就是省下缓存输入 token 的钱，如果在一堆模型之间来回切，性能也许上去了，成本可能失控。作者 **SilenN** 的回答是"诀窍是很少切换，或者只在任务边界切换"，并且承认路由的结论往往就是"这一个模型本来就在这个任务的帕累托前沿上，那就一直用它"。**cameronh90** 顺势反问：那还不如根本不要网关来切模型，直接让 harness 自己决定子 agent 用什么模型。SilenN 回应说自动帮你算出子 agent 该用哪个模型、并随新模型发布持续更新，是另一条路。**akshay_akula** 也把缓存列为"换模型前必须先回答的问题"。**try-working** 分享了自己造路由器的经验：一般每个领域池子里只该放两个模型。

**关于遥测和"是否真开源"的争执最激烈。** **rdslw** 直接开火，说商业模式是"经典的先养用户再 rug-pull"，证据是第一句话就高调宣称开源，却把默认开启的遥测"怯懦地藏在 README 最后一段"。SilenN 反驳说 PostHog 只是开源仓库的使用分析、可以自己审计，并说对方"要么读不懂要么是恶意的"。**aHumbleUser** 随即指出：你 GitHub README 上写的就是默认开启。SilenN 只能把原文贴出来——README 确实写着匿名聚合的 PostHog 遥测默认启用，只是不含 prompt、trace、路径、模型名、凭证等内容。这一轮交锋里作者的说法（"默认关闭"）和文档不一致，是本贴最实在的一处槽点。

**竞品与定位。** **cheema33** 直接问和 LiteLLM 有何不同，**kfallah15** 给的差异化是"从流量做路由和模型优化"，SilenN 补充还有托管市场而不只是 BYOK。**mongrelion** 列了一串要拿来横评的同类项目（GoModel、bifrost、LiteLLM），GoModel 作者 **santiago-pl** 现身推荐自己维护的可复现基准测试。**nejch** 的感慨颇有代表性：希望开源社区把 vLLM Semantic Router 这类有研究和产业背书的项目打磨扎实，"2026 年之于模型路由器，就像 2025 年之于 agent harness"。

**croemer** 提了个务实的命名意见：标题里的"open OpenRouter"乍看像打字错误，OpenRouter 是别人的品牌，不如说"我们做了个类似 OpenRouter 的东西"。**0xbadcafebee** 阴阳了一句"你一周前才开始做？期待三周后回来看你 10 亿美元退出"，**tyre** 查了下第一个 PR 是 6 月 24 日、实际两个月；rdslw 借题发挥说小团队两个月就能复刻，恰恰说明这类产品在 AI 时代的价值大幅降低，"我们这个群体得把价值判断的逻辑调到后 AI 时代"。另外 **jakswa** 发现 GitHub 上那张 UI 截图对应的界面并不在开源仓库里、只有托管版才有，这是他"第一个尴尬的发现"。**swthbht** 问是否也路由推理强度，SilenN 给了个有趣的观察：很多时候 Opus 5 低推理强度约等于高推理强度。
