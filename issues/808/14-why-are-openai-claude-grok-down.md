---
layout: article
title: "为什么 OpenAI、Claude 和 Grok 会同时挂掉？巧合吗？"
issue: 808
number: 14
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=49551096"
hn_url: "https://news.ycombinator.com/item?id=49551096"
date: 2026-09-04
---

## 文章摘要

这是一条在事故正在发生时开出来的 Ask HN 帖子，拿到 392 分。发帖人只贴了三家的状态页（status.openai.com、status.claude.com、status.x.ai）和三条各自的独立讨论串（ChatGPT 315 条评论、Claude 146 条、Grok 142 条），提出了一个所有人当时都在问的问题：2026 年 9 月 3 日上午，OpenAI、Anthropic、xAI 三家前沿模型提供商为什么会几乎同时不可用？

从时间线上看，Anthropic 最早报出"部分故障"，时间是太平洋时间上午 6:23，影响 Claude Mythos 5.1、Fable 5.1、Opus 5 等多个模型，用户看到的是"容量约束"或"Claude is at capacity"；xAI 大约 6:30 跟进，并提到了 Memphis 的故障；OpenAI 最晚，7:43 开始 ChatGPT 和 Codex 大面积报错，最典型的症状是 Codex 客户端返回 `unexpected status 404 Not Found`，URL 指向 chatgpt.com/backend-api/codex/responses，带一个 cf-ray 头。GitHub 也在 7:20 左右发出了「Grok 4.6 Copilot AI Model Provider 事故」的通知。

整个讨论串的走向是一场大型的实时集体推理，几百人拿着手上的碎片证据轮流提出假说再互相证伪。开头一小时基本是各国用户报到（巴西、土耳其、南非、保加利亚都有），中间穿插大量段子——"看来只能回去用纸笔了"、"今天班上完了，走喝一杯"、"现在我都没法跟人说我是软件工程师了"，以及好几个人套用《终结者》台词把 OpenAI 当天要发布的 Astra 模型说成天网觉醒。

真正有信息量的推理主要围绕四条线索。**第一条是 cf-ray 头**：好几个人注意到自己错误信息里的后缀是本地机场代码（ORD、DTW、IST、EWR、ATL、YUL、SOF、GRU、JNB），一度被当成"这是 Cloudflare 故障"的证据，直到 ascorbic 和 sixdimensional 解释清楚 cf-ray 本来就是 Cloudflare 的请求 ID，尾部是服务该请求的边缘节点所在城市机场代码，这只说明流量经过 Cloudflare，不能推出 Cloudflare 出了问题。Cloudflare CTO 后来公开否认是他们的问题。**第二条是共享基础设施**：多人指出 Anthropic 正在大规模租用 xAI 的数据中心容量，wnmurphy 顺着这条线做了最接近结论的推断——Anthropic（6:23）和 xAI（6:30）时间上高度吻合，很可能是 xAI Memphis 的 Colossus 1 出问题连带打击了租用方，而 OpenAI 的 7:43 则更可能是独立事件。**第三条是级联失效**：Insanity、CSMastermind、cortesoft 等多人提出，一家挂掉后用户和自动 fallback 逻辑会把流量灌到另一家，把本就没有余量的对方也压垮。pixl97 补了一句很扎心的产业现实——"任何没有跑满 100% 的 GPU 都是被浪费的 GPU"，jonas21 也强调在 GPU 极度紧缺的环境下这些公司根本不存在"按个按钮就扩容"的选项。nevir 提醒大家别只想着人工切换，Cursor、Copilot code review 这类工具本身就配了自动降级链路，"这几乎是一种可预期的涌现行为"。**第四条是阴谋论**：有相当一部分楼层滑向了 NSA/Room 641A、监听设备被插进推理链路、乃至企业合谋的猜测，madrox 一边参与一边主动出面标注"我不想制造 FUD"，把话题拉回 xAI 基础设施那条更靠谱的解释上。

## HN 评论精华

**最有分量的一条来自 OfficialTurkey**，他自称在 OpenAI 工作、并且就是当天这次事故的 Incident Commander。他给出的说法非常克制：故障源于他们内部的一个路由错误（routing error），与 Astra 发布无关，并且明确表示"我们不对其他厂商的故障发表评论"。这条回复下面还衍生出一段轻松的插曲——swader999 打趣问他是不是特意争取了"事故指挥官"这个职位，OfficialTurkey 澄清这只是事故存续期间的一个临时角色，用来推进流程、跟踪工作流、明确谁是决策人，他本人只是做基础设施的普通工程师；eli 和 stillpointlab 补充说这个称呼直接来自消防、警察、急救系统通用的 Incident Command System，好处是在多个响应主体需要协同时理清指挥关系。

**strictnein 是"平淡解释派"的代表**。他引用报道指出 OpenAI 官方说法是"9 月 3 日上午 7:43 PT 开始的一个路由错误"，Anthropic 的问题更早、6:23 PT 开始，只影响特定几个模型，然后反问：这两家公司本来就都不是可用性优等生，两次部分宕机时间上重叠了而已，为什么大家非要在普通解释够用的时候去找非凡解释？computerex 追问"所有这些公司精确同时挂掉的概率有多大"，jeffbee 回敬"'精确同时'是指相隔 80 分钟吗"，computably 甚至做了个粗算：如果按每周一次三小时宕机窗口、独立泊松过程估计，两家重叠的概率约 1/56，一点都不稀奇；schiffern 补充说 80 分钟的间隔恰恰符合经典级联失效的特征而非同时故障。

**hparadiz 给出了级联说最生动的版本**：太平洋时间上午 9 点/东部中午 12 点的工作日，他所有同事的第一反应都是"Codex 挂了，我试试 Claude"，乘以几百万人就够了，而且 OpenAI 的宕机时间正好撞上他们发 GPT-6 的推文、比开始灰度早一小时。serf 反驳这个说法隐含了一个前提——每一家前沿厂商都恰好精准地按需求配置了容量、没有任何弹性，他不相信这些公司能把负载预测做到那么准，也不相信在互相竞争的环境下会主动保持这么低的余量。lowbloodsugar 则用时间顺序反驳级联方向：Anthropic 6:23、OpenAI 7:43，看起来不是 OpenAI 的流量压垮别人，而是相反；他还补了一个很有画面感的观察——Anthropic 那条讨论串一开始塞满了"最后一根稻草！我终于转投 Codex 了，那边完全没问题！"的营销账号，等 Codex 也挂了之后这些帖子全被删掉了。

**"为什么 Gemini 没事"成了一个反复出现的支线。** erdos_2 打趣说如果级联理论成立，那说明根本没人拿 Gemini 当备胎；Insanity 承认自己列清单时压根没想到 Gemini。nevir 和 JacobAsmuth 提出了更善意的解释——Google 有巨大的冗余容量，或者能通过 load shedding 吸收几乎无限的需求尖峰。也有人报告 Gemini 当时同样在报错，于是 bornfreddy 打趣"他们八成是故意弄坏一点免得被落下"，joshstrange 接着编了个段子：想象 Anthropic、Google、OpenAI、xAI 的机架都在同一个机房里，所有人都在喊自己挂了，Google 看了看自己那排服务器，悄悄用脚把电源拔掉说"哎呀，我们也挂了"。

**Terr_ 提供了整个讨论里最有工程价值的一条建议**：如果所有人的降级链路都是同一份"先用 X，不行用 Y，再不行用 Z"的顺序表，那就会复现 1991 年那篇《The Power of Two Choices in Randomized Load Balancing》描述的问题——正确做法是随机挑两个候选，比较健康度后选更好的那个，成本很低但效果显著。

**derdi 提出了对级联说的一个实证质疑**：大家老说"每个人都能随时切换"，但在他所在的超大公司根本没有 Anthropic 的合同，组织越大越官僚，越不可能同时和所有厂商签约。foldr 和 krzs9 给出了相反的经验——很多公司有一个半官方的首选厂商加上几个备用订阅，krzs9 所在的八千人公司可以随便选 Google、OpenAI、Anthropic、xAI 的模型，用 agent 无关的 harness 切换非常容易。

另外几条被高频提及的观察：Havoc 感叹"科技公司的状态页毫无用处基本已经成了传统"；netsec_burn 调侃状态页的黄色表示服务器着火、红色表示 Sam Altman 已经倒在地上流血；utopiah 借机说这证明这些公司在常规 IT 平台运维上并不比别人强，noir_lord 附和说他们真正擅长的是把政府拉进自己的叙事里推数据中心建设。lrvick 说他从没像现在这样为自己在家自建 GPU 机架感到得意；sarkarghya 更直接："所以说，孩子们，还是得自己买 GPU"。最后版主 dang 贴出了相关的后续讨论串《Nobody is saying why OpenAI and Anthropic had outages》。
