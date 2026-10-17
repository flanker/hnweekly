---
layout: article
title: "Muse Wonder：Meta 面向个人与专业 AI 智能体的前沿模型"
issue: 814
number: 24
category: data
original_url: "https://research.meta.ai/blog/muse-wonder"
hn_url: "https://news.ycombinator.com/item?id=50059115"
date: 2026-10-16
---

## 文章摘要

10 月 12 日，Meta 在其 AI 研究博客上发布了 Muse Wonder 系列模型，包括 Muse Wonder 和 Muse Wonder Full 两个版本，称它们是 Meta 迄今最强的模型，在个人和专业智能体任务上达到业界领先水平，是其"扩展阶梯"上的下一级，也是迈向"个人超级智能"的一大步。

**发布方式。** Muse Wonder 当天即可通过 Meta Model API 和编程工具 Muse Code 使用；Muse Wonder Full 只向 Muse Code 订阅用户开放，没有放进 API。Muse Wonder 之后会为消费级产品 Muse 提供支持，先向订阅用户推出，还将用于生成工件（artifact）和其他需要更高智能的流程。Meta 还计划以 Muse Wonder 为基础，开发可以提供给所有 Muse 用户的专用模型。

**背景。** 文章回顾说，14 个月前 Meta 开始从零彻底重组 AI 工作，从基于第一性原理重建技术栈，到如今推进前沿。发布 Muse Spark 时，Meta 阐述了"可预测、严谨扩展"的方法，这次的发布在预训练和强化学习两方面延续了那套工作。两款模型在 Meta 位于俄亥俄州、吉瓦级规模的 Prometheus 数据中心训练。

**基准成绩。** 文章给出了一张完整的对比表，比较对象是 Claude Fable 5.1、GPT-6 Astra 和 Claude Opus 5.5（均为 max 设置）。部分数据如下（依次为 Wonder Full / Wonder / Fable 5.1 / Astra / Opus 5.5）：

- Meta 内部的"个人超级智能指数"：59.1 / 54.3 / 49.9 / 54.5 / 55.2
- Harvey LAB-AA（法律智能体）：13.1 / 10.0 / 6.4 / 8.6 / 4.2
- Vals Finance Agent V2（金融分析）：59.1 / 56.6 / 58.9 / 53.5 / 58.6
- AutomationBench-AA（端到端业务流程）：45.4 / 39.1 / 32.1 / 41.6 / 41.2
- MemoryBench（记忆上下文）：85.5 / 85.6 / 79.0 / 81.4 / 81.5
- DeepSWE 1.1（长周期编程）：79.0 / 78.8 / 67.4 / 74.1 / 74.2
- SWE-Marathon（超长周期工程）：57.5（xhigh）/ 53.1 / 45.6 / 42.5 / 46.3

但也不是全面领先：HealthBench Pro、Terminal-Bench 4.0、JobBench、OSWorld V2.1 上 Opus 5.5 最高，GDP.pdf 上 GPT-6 Astra 最高，ProgramBench（从零编程）上 Opus 5.5 以 70.5 大幅领先 Wonder Full 的 59.0。在 Artificial Analysis 综合智能指数上，Opus 5.5 以 58 排第一，Wonder Full 以 56 排第二，Fable 5.1、GPT-6 Astra、Gemini 4 Argon 均为 53，Wonder 为 52。博客的柱状图还用箭头标出了 Meta 自家模型一年来的进步：Muse Spark 31、Spark 1.1 34、Spark 1.2 40、Spark 1.3 48，再到 Wonder 52 和 Wonder Full 56。

**四项核心能力。**

- **领域专长**：把专业知识用于工作和日常生活中的问题，从读懂租约到理解财务或健康信息，评测涵盖法律、金融、健康。
- **主动记忆（Active Memory）**：Meta 认为可靠的记忆是实用个人智能体最关键的一步，模型需要跨对话记住关于你的重要背景，并主动检索与当前任务相关的细节。
- **主动专注（Active Focus）**：为一个困难目标持续努力，尝试不同方法、突破障碍、根据新信息调整，必要时请求澄清，并在采取重要行动前先确认。用 FocusBench 和 SWE-Marathon 评估。
- **前沿编程**：编程把以上能力汇于一体，要跟踪需求、跨代码库修改、反复测试和修订。两个模型都在多样化的长时任务上训练过。

博客附带了几个演示：根据可定制的图片输入构建一个可玩的 3D 厨房游戏、在浏览器和多个应用间工作，根据个人偏好制定周六跑步计划、根据个人 Instagram 上的真实照片设计并订购定制周边、按照一系列复杂匹配要求安排婚礼座位表等。

**能力的泛化。** 文章专门讨论了"刷榜"（benchmaxxing）和"大模型味"（big model smell）的说法：两个基准分数相近的模型，一个可能比另一个能应对广得多的陌生任务。Meta 在扩大测试时计算的同时，对比了模型在后训练重点任务（分布内）和未专门训练的任务（分布外）上的表现。结果是，Muse Spark 1.3 和 Muse Wonder Full 在分布外任务上随计算量提升的幅度相似，但在同等分布内表现下，Wonder Full 在陌生任务上的成绩比 Spark 1.3 的趋势线预测得更好。Meta 将其归因于更大、更强的基座模型：基座越强，后训练的收益越能迁移到陌生任务上。这对个人智能体尤其重要，因为目标、工具和约束的组合千变万化，训练时不可能全部预见。

**安全。** 个人智能体代表用户行事，可能接触消息、日历、私人文档，因此需要谨慎处理隐私信息并抵御滥用。Meta 称 Muse Wonder 系列在对抗性指令的鲁棒性和隐私信息处理上都处于领先，对 CBRN（化学、生物、放射性和核）和网络滥用提供了迄今最强的防护，同时按照"Meta 超级智能扩展框架"保留前沿网络能力供防御用途。Meta 称这是其迄今对齐程度最高的模型，完整的安全评估报告将稍后发布。

## HN 评论精华

本帖 671 分、370 多条评论。讨论的焦点包括：扎克伯格的高价挖人策略是否终于见效、模型是否刷榜、"超级智能"这个用词、多家前沿实验室并存是好是坏、Meta 的隐私记录，以及一批早期用户的实测反馈。

**扎克伯格的豪赌见效了？** 217 开玩笑说"抱歉扎克，我不知道你这么能打"，在 AA 指数上仅次于 Opus 5.5 已经相当不错。Cookingboy 回顾：当初扎克伯格花数十亿美元收购 Scale、让二十多岁的 Alexandr Wang 掌管 Meta 超级智能实验室、挤走图灵奖得主 Yann LeCun、开出九位数薪酬从其他实验室挖人，人人都在嘲笑，华尔街也痛恨 Meta 的资本开支；如今看来，在一个并非全新的产品领域里，扎克伯格再次证明了自己是出色的执行者。他把"执行者"写成了"executioner"（刽子手），引来一串调侃。overfeed 反讽说相信 Muse 会和扎克伯格开创的元宇宙一样伟大，随后展开一场关于元宇宙算不算失败的争论：avarun、heaney-555 说 Quest 头显卖得跟 Xbox 差不多、Ray-Ban Meta 眼镜的销量比其他所有智能眼镜加起来还多一个数量级，overfeed 回应"黑莓当年也卖得很好"。关于 LeCun，redox99 说留着一个讨厌 LLM、预言总在几个月内被证伪的人才是蠢，hashmap 反驳：让一个靠低薪雇人标注数据起家的人取代卷积神经网络的发明者，并不值得称道。

**刷榜疑虑。** clueless 问刷了多少榜，phoghed、xynelius 认为可以默认每个模型都在刷榜，毕竟基准本该代表常见任务。samseaborn 指出博文专门有一节讨论如何衡量刷榜，phoghed 讽刺道"在所有科技 CEO 中，扎克伯格是绝不会骗我的那一个"。cvhc 吐槽博客页面的鼠标特效让散光和飞蚊症患者很难受，ollin 给了一行在控制台隐藏 canvas 的代码。user43928、droidjj、dcchambers 指出一个关键细节：主页面上领先的成绩来自只对订阅用户开放的 Wonder Full，API 里只有 Wonder。

**实测反馈好坏参半。** droidjj 贴出价格：每百万输入 token 4 美元、缓存输入 0.2 美元、输出 20 美元。Jcampuzano2 注意到这和 Meta 以往低价策略不同，已经比 Sol 贵、与 Opus 持平，可能意味着 Meta 自认已进入前沿。jjcm 做了图片转 HTML 的测试，认为 Wonder（非 Full）明显不如 Opus 5.5，大致介于 Sonnet 5.5 和 Haiku 5.5 之间，猜测视觉能力还不成熟。ekojs 用它做 React 前端打磨、Node 后端审查和 Rust 小改动，认为"智力有了，但品味和判断力还没到"，还在一个简单改动上陷入审查和修复的死循环。himata4113 认为它在真实使用中仍落后一代；chriddyp 则在数据分析测试中发现它与 Opus 5.5 同级，比 Muse Spark 1.3 快一倍多、贵一倍左右。simonw 照例让它画骑自行车的鹈鹕，结果"相当不错"；wxce 则说该停止鹈鹕测试了，这是典型的古德哈特定律。er4hn 讲了用 Muse（大概是 Spark 1.3）全权安排西雅图周末游的经历：它推荐了已经倒闭的餐馆，弄错了奶酪店开门时间，还惊慌地提醒他航班就在今天，其实那是第二天的每日航班。hugh4life 担心低价订阅会被收紧：他用 15 美元的 Muse Code 套餐搭配 Spark 1.3 能用出约 1500 美元的额度，可用 Wonder 跑两个小提示就花掉了每周额度的 4%；chrisco255 补充，Alexandr Wang 回应说用量过高是个 bug，已修复并重置额度。sergiotapia 也说 5 美元套餐几句问答就用掉了 14% 的周额度。

**"超级智能"这个词。** chenster、khalic 对"超级智能"一词很反感，认为这是在讨好特朗普。ipsum2、frozenseven、laidoffamazon、arjie 纠正：Meta 超级智能实验室 2025 年 6 月就成立了，Bostrom 的《超级智能》2014 年就出版了，要说也是特朗普借用了扎克伯格的说法。stratos123 总结：Meta 用的不是特朗普要求企业采用的那个"表忠心"的流行词，而是 Facebook 几年前为营销编出来的流行词，两者都和这个词的本义毫不相干。

**前沿多几家是好是坏？** atleastoptimal 认为前沿公司越多越好，否则一家公司实现递归自我改进后会造成极端的权力集中；freakynit 唱反调，认为一家独大时其他公司为自保反而会站在普通人一边。socializer 认为这些巨头在"AI 将管理社会"这一核心理念上是一致的，"王座坐得下不止一个人"；atleastoptimal 举出马斯克离开 OpenAI、Amodei 出走创立 Anthropic、Ilya 两度出走，说明精英间的怨恨和竞争远比合作更常见。tekacs 观察到现状是"总有一家领先，但每隔几周换一家"，这对公众来说是一种有用的不稳定平衡。讨论还岔到了 Anthropic 的模型福利立场：hintymad 引用《纽约时报》的报道，aesthesia 引用 Opus 5.5 模型卡中"我们不确定 Claude 是否应受到直接的道德考量，但认为这是现实的可能性"的表述，staticman2 则反复质问：如果真这么认为，删除模型为何不算不道德。GodelNumbering 指出递归自我改进的支持者大大低估了验证难题：判断 N+1 版本是否优于 N 版本，只能靠人类或 N 版本本身，而两者都超出了能力范围。

**开放权重与商业模式。** PcChip、cardboard9926 等人问是否会开放权重，kennywinker 提醒 Meta 9 月发布 Muse Spark 时就承诺过开放权重，至今未兑现。swalsh 认为扎克伯格当初的 Llama 论点正被一周周验证：智能的"租金市场"不会出现，持久的收益属于掌握稀缺资产（算力、专有数据、网络效应）的一方，开放权重反而能让这些资产更值钱。Starlevel004、saisundar 认为 Meta 追上来的一大原因是数据：它以近乎免费的价格提供模型、换取用户同意用于训练，单是 Opencode 上一周就有近 40 万亿 token 流经 Muse Spark 1.3 的"贡献者"版本；Muse 也是继 ChatGPT 之后最大的消费级发布。

**隐私信任问题。** nerevarthelame 指出 Meta 自己的内部指标显示约 3% 的个人信息处理违规率，对于要让普通人把私事交给智能体的产品来说太高了；kraf 讽刺说"把个人数据交给 Meta"本身就是这项基准的定义。pmuse 报告在登录 Muse Code 时看到了一些陌生开发者的账号，引发一阵恐慌，后来他排查发现与 Safari 的 Wipr 2 内容拦截扩展有关，并向 Meta 道歉；nmfisher 反问为何要道歉，如果原因真是浏览器指纹，那恰恰是 Meta 的问题。throwyawayyyy 说他刚想退订某个 Meta 产品的营销邮件，结果页面报错"不支持的请求方法：post"，"没有哪家公司比它更不配获得使用 Muse 所需的那种信任"。此外，arecsu、murermader 认为厨房游戏演示完全照搬了《胡闹厨房》（Overcooked），而且在 Firefox 上漏洞百出；daemonologist、overfeed 拿命名开涮："Glimmer、Spark、Wonder，下一个是什么，最终是切伦科夫辐射吗？"InsideOutSanta 接了一句"Muse 恶魔核心"。
