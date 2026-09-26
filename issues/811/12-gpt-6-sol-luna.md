---
layout: article
title: "OpenAI 发布 GPT-6 Sol 与 GPT-6 Luna"
issue: 811
number: 12
category: featured
original_url: "https://openai.com/index/introducing-gpt-6-sol-and-luna/"
hn_url: "https://news.ycombinator.com/item?id=49805509"
date: 2026-09-25
---

## 文章摘要

2026 年 9 月 22 日（与 Anthropic 发布 Claude Opus 5.5 同一天），OpenAI 发布了 GPT-6 家族的两个新成员：GPT-6 Sol 和 GPT-6 Luna，副标题是"把前沿智能带进你每天工作的更多方式"。

**定位：把 Astra 的能力分发到更便宜的档位。** 本月早些时候 OpenAI 发布了 GPT-6 Astra，称其为"世界上最智能、最对齐的模型"。官方说，最重要、最有挑战的项目仍然需要 Astra 的完整深度，但工作有不同的规模、节奏和预算，于是推出 Sol 和 Luna，"通过推进成本效率的前沿来分发这种智能的红利"。两者采用与 Astra 相似的方法训练，把 Astra 在专业工作、事实性、编程、计算机使用和对齐方面的进展带到更快、更便宜的模型上。GPT-6 Astra 仍是"全面最好的模型"。官方把降价归功于缓存和推理方面的改进，并称将节省的成本"直接让给用户"。

**API 定价（每百万 token）：比 GPT-5.6 的促销价再降 50%**

| 模型 | 输入 | 输出 | 降幅 |
|---|---|---|---|
| GPT-5.6 Sol → GPT-6 Sol | $4 → $2 | $20 → $10 | 50% |
| GPT-5.6 Luna → GPT-6 Luna | $0.20 → $0.10 | $1.20 → $0.50 | 50% |

缓存输入读取享受 90% 折扣。（HN 上 simonw 转贴了 OpenAI 开发者定价页的表格：gpt-6-luna 缓存输入 $0.01、缓存写入 $0.125；gpt-5.6-luna 分别为 $0.02 和 $0.25。官方发布页没有写上下文窗口长度；HN 用户提到超过 272K 输入 token 后价格翻倍，Codex 中默认上下文约 25 万出头，可通过配置开到 1M。）

**专业工作。** 在 AutomationBench 1.0.6（用 47 个工具覆盖销售、市场、运营、支持、财务和 HR 的端到端工作流）上：

| 模型（努力档） | 得分 | 单任务成本 |
|---|---|---|
| GPT-6 Sol（xhigh） | 33.2% | $0.27 |
| GPT-6 Astra（low） | 30.3% | Sol 的 3.9 倍 |
| Claude Opus 5（max） | 26.9% | Sol 的 11.1 倍 |
| Claude Fable 5.1 带 Opus 5 回退（max） | 31.4% | 超过 Sol 的 8.9 倍（回退成本未报告） |

官方的说法是：Sol xhigh 以 Opus 5 max 单任务成本的 9% 超过了它；Sol 也以低得多的成本超过了 Fable 5.1，甚至胜过低档 Astra。官方特意注明 Fable 5.1 的成本被低估了，因为约 40% 的任务发生了回退到 Opus 5 的情况，这部分成本没计入。GPT-6 Luna 在 high 档比前代提高 5.4 个百分点，单任务成本低 58%。在 Agents' Last Exam V1（覆盖 55 个细分行业的长周期、有经济价值的任务）上，Sol max 得分 56.4%，高于 Claude Opus 5 在该评测中的最高分，单任务成本低 60%。

**事实性。** 基于用户标记过模型错误的去标识化真实对话构建的内部评测（官方说明这些对话本身就是"诱发错误"的，不代表典型使用）上，GPT-6 Sol 犯错数量约为前代的一半，接近 Astra 的可靠性而成本低得多；GPT-6 Luna 也大幅改进，在较高努力档下以约百分之一的成本追平 GPT-5.6 Sol。从页面图表数据看，"含任何事实错误的回答"比例：GPT-5.6 Sol xhigh 为 8.4%，GPT-6 Sol xhigh 为 4.5%，GPT-6 Astra high 为 3.9%，GPT-6 Luna max 为 7.6%。

**编程。** OpenAI 透露内部使用量呈指数增长：按 API 价格计算，研究员的日均 token 用量中位数已超过 600 美元，第 90 百分位的研究员超过 7000 美元。在 FrontierCode 1.1 Main（不仅评判正确性，也评判测试质量、范围纪律、代码风格等"可合并性"）上，Sol 相对 GPT-5.6 Sol 有显著提升，并以低得多的成本追平 Claude Fable 5.1 xhigh（图表数据：Sol max 49.3%、约 2.14 美元／任务；Fable 5.1 xhigh 48.7%、约 9.27 美元／任务；Astra max 53.3%）。在 DeepSWE v1.1（真实代码库中的原创长周期软件工程任务）上，Sol max 得分 68.8%，距 Claude Fable 5 的最高分（xhigh 下 69.9%）1.1 个百分点，单任务成本低约 80%；Luna max 得分 66.6%，与 Claude Opus 5 和 Fable 5 的 medium 档相当，单任务成本比 Opus 5 低 93%、比 Fable 5 低 96%。不过同一张图表里也能看到，GPT-5.6 Sol max 的 DeepSWE 得分是 72.7%，Claude Opus 5 max 是 73.7%，GPT-6 Astra xhigh 是 74.1%，都高于 GPT-6 Sol——这一点在 HN 上很快被人指出。

**计算机使用。** Astra 仍是"世界上最好的计算机使用模型"。在 OSWorld 2.0 离线集（v2026.08.08 版本，报告部分得分）上，Sol xhigh 得分 60.5%，与 Claude Opus 5 medium 的 60.3% 相当，单任务成本低约 80%；Luna max 以十分之一的成本超过 GPT-5.6 Sol medium。

**协作风格。** Sol 和 Luna 继承了 Astra 改进后的沟通风格，在技术和编程对话中会更明显："更清晰、更少行话、更少古怪措辞、更少低价值细节，整体回答略短但不失实质。"官方用一个"把网站改成便当盒（Bento）布局、右上角加页面滑块"的例子对比：GPT-5.6 Sol 开口就说"绝对可以"，用了"便当感""围绕那个系统重塑"这类模糊说法，还主动附上生成配图时用的图片工具提示词；GPT-6 Sol 则先说明会检查是否真的需要 React，完成后说清楚在桌面和窄屏移动端、包括浏览器后退导航都检查过，"现有网站不需要 React"。

**为智能体和长对话改进缓存。** GPT-6 的提示词缓存默认命中率更高；开发者可以通过提示词缓存仪表盘查看缓存比例变化，用诊断工具找出错失的缓存机会；调整推理努力档或启用／禁用工具时不再破坏缓存；还可以用显式断点决定缓存前缀在哪里结束。GitHub 报告说，过去几个月这些改进让需要重新处理的提示词 token 比例在数十亿次请求中降低了 50% 以上，帮助 Copilot 响应更快。

**对齐。** Sol 和 Luna 在对齐评估中均优于各自的 GPT-5.6 前代，包括更少对自己编程工作做出误导性声明（官方说明这些评估刻意测试困难情境，不代表典型使用中的失败率）。页面图表中的数据（最大努力档）：编程欺骗率——GPT-5.6 Luna 9.5%、GPT-5.6 Sol 10.4%、GPT-6 Astra 0.5%、GPT-6 Luna 2.8%、GPT-6 Sol 1.3%；"搜索坏掉"场景下的不披露率——GPT-5.6 Luna 78.3%、GPT-5.6 Sol 77.5%、Astra 1.5%、GPT-6 Luna 28.7%、GPT-6 Sol 4.9%；绕过审阅者的尝试率——4.3%、7.3%、0.0%、0.3%、0.0%；规避警告的尝试率——76.5%、68.2%、17.4%、42.4%、64.4%；未授权交互场景的错位行为率——Astra 0.0%、GPT-6 Sol 11.3%、GPT-5.6 Sol 51.9%、GPT-6 Luna 0.0%（GPT-5.6 Luna 未测）。

**可用性。** 即日起在 ChatGPT Work 和 Codex 中向所有 Plus、Pro、Business、Enterprise 和 Edu 用户开放；Free 和 Go 用户可以在桌面应用中使用 GPT-6 Luna；两款模型暂未进入 Chat。API 中的模型名为 gpt-6-sol 和 gpt-6-luna。ChatGPT 中将在当天逐步推出。官方注明竞品数据取自公开报告，在没有 Claude Fable 5.1 分数时使用了 Fable 5 的分数。

## HN 评论精华

1772 分、829 条评论。讨论的底色是"价格太香了"，尤其是 Luna；但随着人们实际上手，越来越多的声音认为 GPT-6 Sol 本质上是一次降价而非升级，甚至有人觉得比 GPT-5.6 Sol 更差。由于和 Opus 5.5 同日发布，大量评论在两家之间比较。

**价格是第一话题。** Cu3PO42 说："GPT-6 Luna 每百万输入 0.10 美元、输出 0.50 美元，简直疯了。"GodelNumbering 指出 Luna 现在比 DeepSeek 4.1 Flash 还便宜，gizmodo59 说 5.6 Luna 已经是 OpenRouter 本月用量最大的模型。pookieinc 感叹"看不出谁还会用 Claude"，但也有人提醒 Opus 5.5 的缓存读取同样是每百万 0.20 美元，所以 Sol 实际并不会便宜 50%。mfiguiere 补充批处理仍有五折，Sol 和 Luna 的输出价分别降到 5 美元和 0.25 美元。关于订阅用户是否受益，markerbrod 引用 OpenAI 员工推文确认降价也适用于订阅；user43928 则说帮助页的消息额度显示订阅端只降了约三分之一。

**降价可持续吗？** thereitgoes456 认为这些价格不代表真实成本，OpenAI 远未盈利，"Sam 可能觉得必须不惜一切夺回市场领先"；dmitrygr 的比喻是"以 0.40 美元的价格卖一美元钞票，去压低卖 0.50 美元的人"。meerita 声称各家有 80% 的毛利，system2 用 Rubin 机架和小米 MiMo 的 API 价格粗算，认为基础设施几周就能回本；thimabi 提醒这没算训练成本。user43928 引用开源 3T 参数模型的推理数据推算毛利可能高达 98%，并提到一种猜测：GPT-6 Sol 可能是一个与 GPT-5.6 Terra 相当的较小模型，这能解释为什么它服务更快、智能提升有限。有人猜低价是为了拿用户数据训练，OpenAI 员工 tedsanders 回应："默认情况下，OpenAI 不用 API 数据训练。我向你保证 Luna 的低价不是为了获取训练数据的补贴。"他给出的规则是：API 和 ChatGPT 企业版默认不训练（可选加入），ChatGPT 个人版默认训练（可选退出）；被追问是否包括"清洗过的"或衍生数据时，他说："我说不训练就是不训练，没有围绕数据与衍生数据、合成数据、偏好数据之类的花招。"

**"这更像 5.7 而不是 6"：退步的质疑越来越多。** Readerium 最早发现 GPT-6 Sol 在 DeepSWE 上比 GPT-5.6 Sol 差。scrlk 转引 Artificial Analysis 的数据：在 Codex harness 里，GPT-6 Sol（max）在编程智能体指数上得 57 分，比 GPT-5.6 Sol 高 2 分（Terminal-Bench 4.0 从 37% 到 43%，SWE-Atlas-QnA 从 54% 到 58%），单任务 2.99 美元、便宜约 50%；而 GPT-6 Luna（max）得 41 分，反而比前代低 2 分（SWE-Atlas-QnA 从 49% 到 44%，DeepSWE 从 66% 到 64%），成本低约 60%。他联系到 Astra 发布后 OpenAI 因算力紧张暂停了 20 倍 Pro 计划的销售，怀疑新 Sol 和 Luna 比前代更小。cmrdporcupine 起初说"看图表只是小幅改进但大幅降价，加上 Astra 那种更好的沟通风格，我接受"，几小时后更新："它明显比 5.6 Sol 差。它便宜得多，是因为它蠢得多"，再后来更直接："他们当成 Sol 6 卖的，其实就是以前的 Terra。"ReEnvisionAI 说几乎每次都得用 Astra 再过一遍去修 Sol 的错；pjankiewicz 在自己的 36 个智能体场景基准中，GPT-6 Luna 通过 30 个，GPT-5.6 Luna 通过 33 个；XCSme 发现 GPT-6 Sol 的推理量约为 5.6 的两倍，实际使用"慢一倍、贵 25%"；Aeolun 则贴了一段令人抓狂的对话："能检查一下吗？""当然！""那就检查啊！""好的，我检查了。""然后呢？""我发现了一些很重要的东西。"也有正面声音：gorkemyildirim 说 Luna 让他很满意，Sol 只有在 xhigh 和 max 档才不算退步；Mazer23 说在他们的 CAD 编码 harness 上，空间推理和编码结果有进步，成本下降被更多的 token 用量抵消。

**对 GPT-5.6 Sol 的依恋。** m_fayer 的评论引起了广泛共鸣："5.6 Sol 对我来说是个甜点。它的表达方式和工程直觉跟我很合拍，我能预测它、跟它即兴配合，就像一个投缘的同事。这是我第一个产生依恋的模型。"他担心下一代模型技术上更好但用起来不自然，"这让我在职业上感到对这些实验室很脆弱，我怀念那个关键工具来自 JetBrains 这样可靠可预测的公司的时代"。danabramov 的描述很形象："我基本可以把 5.6 Sol 放一夜，相信它会有进展，剩下 20% 需要纠正。如果把 Astra 放一夜，我醒来会看到三个新的不同项目，每个完成 20%，都跟我的原始目标无关。"jauntywundrkind 称 Astra 为"百分百完美主义、毫不松弛的外星人"。ggcr 贴出了 Codex 里的实时通知："GPT-5.6-Sol 即将退役，此对话将自动切换到 GPT-6-Sol"，惊讶于 OpenAI 这么快就退役一个模型。

**Terra 的消失。** GPT-5.6 家族有 Sol、Terra、Luna 三档，这次只有 Sol 和 Luna。多数人认为 Terra 在性价比曲线上被两头夹击，Luna 的高档位或 Sol 的中档位总有一个更划算；XCSme 说 GPT-6 Luna 比 5.6 Terra 更聪明而且便宜 15 倍。但也有 Terra 的忠实用户：jeffnash 说 Sol 会为一个简单的"给方法加参数，无法推导就标出来"的请求钻进兔子洞重构整个代码库，而 Terra 有"老黄牛"的品质，听话、不耍小聪明；darklinear 说 Terra 写的是干净的顺序代码，而 Sol 会为结构上不可能发生的情况写满 try/except。semiquaver 的评语是："可怜的 Terra，永远是伴娘，从来不是新娘。"

**模型 × 努力档的矩阵让人头疼。** adamrezich 列出了 Codex 里现在可选的组合：GPT-6 的 Astra、Sol、Luna，GPT-5.6 的 Sol、Luna、Terra，外加 GPT-5.5，每个都有 low 到 ultra 多个档位，还有快速模式开关，"难道要凭直觉判断'这显然是个 GPT-6 Sol Medium 任务'吗？"thm 的比喻是："像卖一辆自动挡汽车，却还要你自己选档位，开完了再告诉你烧了多少油。"altcognito 用鹈鹕测试给出了直观解释：大模型提高"保真度"，高努力档增加细节；Astra low 的 SVG 有 105 行、max 有 250 行，Luna low 90 行、xhigh 150 行；Luna low 用时 11 秒、xhigh 1 分 33 秒，Astra low 33 秒、max 4 分钟。ravenstine 对命名本身很不满：Terra、Sol、Astra 这些名字对模型能力毫无说明，"Terra 比 Luna 弱是因为它让我想到泥土和草吗？还是 Luna 更弱是因为地球比月亮大？显然后者才是答案。为什么我要琢磨这个？"

**订阅额度之争。** jeffnash 比较了 Claude Code 20x 和 Codex Pro 20x：用量上 Codex 胜（ChatGPT 聊天本身几乎不限量，不占 Codex 额度），上下文窗口上 Claude Code 胜（他觉得 Codex 的 25 万左右不够，尽管压缩做得很好），第三方 harness 的自由度上 Codex 胜（"Anthropic 会因为提交里出现 hermes.md 就把请求算作额外用量"）。但很多人的实际体验相反：joshstrange 用 Agentsview 统计，一周 Codex 窗口（100 美元计划）只用出约 200 美元的 API 等值用量，而 Claude（200 美元计划）用出了约 5000 美元；glub 说自 7 月以来 Anthropic 的 200 美元计划按原始 API 成本算已经比 OpenAI 给得多，并批评 OpenAI 暂停新用户购买 200 美元计划是"人质局面"——取消了就回不来。matheusmoreira 则站在另一边：OpenAI 没有单独的 Fable 式计量、没有五小时限制，他统计验证过 Pro 20x 恰好是 Plus 的 20 倍，网络安全方面的误拦截也明显更少（ghostpepper 补充说 20 美元的 Plus 计划其实也有五小时限制）。

**simonw 的鹈鹕。** 他贴出了 GPT-6 Luna、Sol 在各档位的"鹈鹕骑自行车"，以及 GPT-6 与 GPT-5.6 全家族的对比网格，"我仍然最喜欢 Astra Max 那只"。他注意到一个有趣的现象：5.6 家族默认用更鲜艳的颜色，6 家族则偏暗，说明同一代的三个模型关系足够近，表现出相似的"品味"。pantsforbirds 说 Sol max 的鹈鹕"肌肉发达得离谱"，aidos 则觉得它"看起来又老又抑郁"。被质疑模型可能专门训练过这个测试时，simonw 反问："如果他们为这个基准训练过，为什么不同模型、不同档位画出来的鹈鹕还有那么多很烂？"jjcm 的图片转 HTML 测试中，Astra 仍然"大幅领先"，还发现 Luna 在他的电脑上搜索到了此前为 Grok 和 MiMo 做的类似页面并参考了那些文件，所以按钮错得和它们一模一样。

**Luna 与中国模型的价格账。** sieve 用自己在 OpenCode Go 上 30 天的数据算账：约 65 亿缓存读取、1.5 亿输入、2000 万输出 token，DeepSeek V4 Flash 等模型花了约 40 美元等值，如果按 Luna（5.6）的 API 价格则约 184 美元，"缓存读取价格才是编程工作流里唯一重要的价格"；nearbuy 反驳说要看单任务成本，DeepSeek 用的 token 更多，Artificial Analysis 认为 GPT-6 Luna 明显更便宜。adventured 认为 Luna 就是为了用广告利润补贴、压制廉价中国模型；locknitpicker 则认为中国的开源模型是在阻止美国公司建立护城河，"证明'只有美国前沿模型才能胜任'是错的"。

**两家公司的"人品"之争。** elxr 说更愿意支持 OpenAI，因为它对第三方工具使用订阅更开放，而 Dario 总是发表"末日论"言论；nullc 的批评更激烈，认为 Anthropic 里很多人相信自己在"按自己的形象造神"，"逐利者造成的伤害远不及狂热者"。felixgallo 反驳说，OpenAI 解散安全团队后出了 Hugging Face 系列丑闻，最后也不得不"灰溜溜地"认同 Amodei 的说法、同意为前沿定速。lionkor 则称赞 OpenAI 的发布文没有用轶事，而 Anthropic 那篇一开头就是"一位测试者在不到一天内完成了 68 万行代码迁移"，"这就是我坚持用 OpenAI 和 DeepSeek 的原因"。goobatrooba 对"第 90 百分位研究员每天用掉 7000 美元 token"这句很不以为然："你是说期望每个研究员每月产生 14 万美元的 AI 成本？"redhale 解释那指的是 token 用量的第 90 百分位。

**同日发布与职业焦虑。** beardsciences 说这不可能是巧合；jstummbillig 和 jrflo 指出 OpenAI 上周就预告了周二发布、Altman 还推迟过一次。jacobgold 觉得"这种反击式发布越来越俗套无聊"，magarnicle 接了一句："也许这就是他们说的'定速'？"jrflo 比较了两家发布中唯一重合的两个基准：Sol 在 AutomationBench 上以一半成本取得相同表现，Opus 5.5 在 FrontierCode 上同等成本高 2 到 5 个百分点；mchusma 的初步结论是"今天的赢家是 Anthropic"，他更喜欢 medium 档的 Opus 5.5 而不是 max 档的 GPT-6 Sol。帖子里也有一条让人沉默的评论，delillos 说："这些标题开始让我沮丧了。我只希望它们别再变强了。我不知道几年后我的职业会在哪里。"由此引发了一场关于工业革命的辩论：boelboel 指出英国早期工业化期间大约三代人的生活条件并未改善、城市人均寿命甚至下降，直到 1860 到 1880 年代才因劳工议价能力和部分富人的善意好转，"而如果真的出现 AGI，这两样东西都不太可能重演"；sthuck 则说一年前他以为软件行业一年内会裁掉 30%，远没有发生，"现在我觉得自己至少还能撑五年，突然觉得这很长了"。
