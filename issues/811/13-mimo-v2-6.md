---
layout: article
title: "MiMo v2.6：小米发布并开源新一代全模态模型"
issue: 811
number: 13
category: featured
original_url: "https://mimo.xiaomi.com/mimo-v2-6"
hn_url: "https://news.ycombinator.com/item?id=49792730"
date: 2026-09-25
---

## 文章摘要

小米 MiMo 团队于 2026 年 9 月 22 日发布并开源 MiMo-V2.6 系列，副标题是"前沿智能、全模态、公开构建"（Frontier intelligence, all the modalities, built in public）。官方把这次发布定位为其"RSI 路线"（递归自我改进）探索中的关键一步：在可验证的复杂任务上扩大强化学习（RL）算力，让模型通过探索与反馈不断拓展自身能力边界。

**产品线与规格。** 系列包含两款原生全模态模型（文本、图像、视频、音频合一）：MiMo-V2.6-Pro 是小米迄今最强的模型，MiMo-V2.6-Flash 则在智能、效率与成本之间取平衡。此外还推出 MiMo-V2.6-Pro-UltraSpeed，在质量不变的前提下输出速度最高快 20 倍。据 HuggingFace 模型卡：Pro 为稀疏 MoE 架构，总参数 1.02T、激活 42B（70 层，其中 60 层滑动窗口注意力、10 层全局注意力，384 个路由专家激活 8 个）；Flash 为 309B 总参数、15B 激活（256 个专家激活 8 个）；两者上下文长度均为 1M tokens，另配 681M 参数的视觉编码器、308M 的音频分词器，以及一个 5 层的 MTP 投机解码器。权重以 MIT 许可证发布，同时还放出了一个用 MiMo 生成数据对 Qwen3.5-9B 做监督微调的 MiMo-V2.6-Distill-Qwen-9B。

**把帕累托前沿再往外推。** MiMo-V2.6-Pro 在 Artificial Analysis 智能指数（v4.3，2026 年 9 月）上得分 46.32，超过 Kimi K3 和 Qwen3.8 Max，成为目前最强的开源模型。V2.6 系列沿用 V2.5 的 API 价格——同价更强，于是在"智能 vs 成本"图上又一次把前沿往外推了一截。

**RL 规模化，并且全部开源。** 这是本次发布最引人注目的部分：小米在训练过程中把生产级 RL 运行实时直播了出来（公开的训练仪表盘 mimo.xiaomi.com/rl）。在不到六天里，Flash 和 Pro 各自完成了 30 个 RL step、约 75 万条轨迹，成本分别约为 **85 万美元和 262 万美元**。训练任务的平均通过率相对提升 25% 和 12%。在未参与训练的长程软件工程基准 DeepSWE v1.1 上，Flash 从 48.8 升到 65.68（约 17 分），Pro 从 58.4 升到 72.57（约 14 分）。官方称 RL 样本效率高、整个过程持续提升，并能泛化到训练分布之外。

他们从三个维度扩大了 RL 算力：

- **更大的 batch 与更高吞吐**：完全异步架构，每次更新 1,568 个样本（模型卡写作 1,568 个 prompt × 16 条 rollout），训练上下文最长 1M，每 step 35 亿到 37 亿 tokens。
- **更多任务、更丰富的环境**：多任务训练集覆盖编程、通用 agent、视觉与网络安全，并混合多种 harness，让一种能力的提升反哺其他能力。
- **更多评分算力**：在每组 rollout 内部做相对比较，为长程任务提供更精细、更多样的奖励信号，形成自我改进闭环，并引导模型用更短路径、更少 token 完成任务。

随着规模扩大，他们冻结了路由器以抑制训练漂移，并从奖励设计、对抗评估、异常检测、验证器交叉检验等多方面防御 reward hacking。工程上还设计了统一的轨迹表示与惩罚机制、支持多个 agent 框架的高并发交互、控制面与数据面解耦以支持大规模轨迹迁移等。小米宣布把完整技术报告、训练环境和 RL 代码全部开源，供研究者复现和验证。

**附录中的完整基准（节选）。** 官方页面附录表格给出了与 DeepSeek V4.1 Flash、Kimi K3、Claude Opus 5、GPT 5.6 Sol、Claude Fable 5/5.1、GPT 6 Astra 等的对比，且大方列出了自己落后的项目：

- 编程：DeepSWE v1.1 上 Pro 71.9、Flash 67.9（DeepSeek V4.1 Flash 74.2、Opus 5 74.0、GPT 6 Astra 74.0、Fable 5 70.0、上一代 V2.5-Pro 仅 19.0）；ProgramBench Pro 26.5（Opus 5 为 37.0）；自家 MiMo Code Bench Pro 63.2（Opus 5 为 68.6）。
- 通用 agent：GDPVal 2.1（AA 的 Elo 分）Pro 1673，Fable 5.1 为 1735、Opus 5 为 1708；Toolathlon-verified Pro 76.9；AutomationBench v1.0.6 Pro 53.1，高于 Opus 5 的 50.3；Terminal Bench 4.0 Pro 34.9，明显落后于 GPT 6 Astra 的 59.6、Fable 5.1 的 55.1 和 Opus 5 的 49.0；Terminal Bench 2.1 Pro 89.9；OSWorld-Verified Pro 82.0；JobBench Pro 62.0。
- 视觉：自家 MiMo Visual Coding Pro 72.3（GPT 6 Astra 82.2）。
- 网络安全：CyberGym 上 Flash 95.1 反超 Pro 的 94.0；ExploitGym Pro 17.8（Astra 42.4）；ExploitBench Pro 47.9（Astra 100.0）；SEC Bench Pro 66.3。

**从 Vibe Coding 到 "Vibe World"。** 官方称 MiMo 的编程能力已泛化到软件工程之外，融合了 3D 空间推理、多模态感知和计算机使用（CUA）能力。展示案例包括：根据图片/视频/文本拆解任务、协调多个 agent 搭建可运行的 3D 交互世界；在 Blender 里根据描述或参考图生成 3D 资产；在具身仿真中读取多视角相机画面、闭环控制 Franka Panda 机械臂完成抓取、配色和精确放置；一句话生成完整前端界面和演示文稿（"MiMo 有自己的品味"）；端到端制作视频，包括镜头编排、配乐、卡点剪辑，以及用 MiMo-V2.5-TTS 为傅里叶分解之类的科普动画配旁白；还尝试作曲——为约十种乐器写了一首管弦乐《Night Road》并自行转成 MIDI，另写了两首 A 小调钢琴曲。

**科研案例。** 在没有专门针对科研做 RL 的情况下：小米的材料专家让 Pro 设计一种能吸附 PFAS（"永久化学品"）的新型金属有机框架（MOF），模型完成了文献与专利检索、提出假设、评估新颖性，并调用开源计算工具做"干实验"，筛出三种 UiO-67 型 Zr₆ 框架候选；在数学上，Pro 协助研究者用 Lean 4 完整形式化了 Li 与 Yorke 的经典论文《周期三意味着混沌》的主定理，项目超过 6,000 行 Lean 代码，由 Lean 内核验证且无未完成占位——而模型并未接受过任何 Lean 专项后训练。

**上架与定价。** Pro 和 Flash 即日起在 AI Studio、MiMo Code、MiMo Desktop（同时结束抢先体验、发布首个正式版）、MiMo API 平台和 OpenRouter 上线。API 价格与 V2.5 相同（美元/百万 tokens，缓存命中 / 缓存未命中输入 / 输出）：

- MiMo-V2.6-Flash：$0.0028 / $0.14 / $0.28
- MiMo-V2.6-Pro：$0.0036 / $0.435 / $0.87
- MiMo-V2.6-Pro-UltraSpeed：$0.036 / $4.35 / $8.7

缓存写入限时免费，另有面向高用量的 Token Plan。

## HN 评论精华

帖子拿到 1129 分、458 条评论，是本周最热的讨论之一。讨论大致分成四股：对小米透明度的赞赏、中美 AI 竞争与"监管俘获"之争、实际使用体验，以及照例的鹈鹕测试。另外 dang 提醒，几天前那个实时训练仪表盘已单独讨论过（155 条评论）。

**透明度是最大的加分项。** rao-v 的置顶评论说，尽管大家对"真正开放"的定义（开放权重、训练数据、训练代码）各有主张，但小米在训练过程上的透明令人印象深刻：训练期间公开的实时仪表盘"是非常棒的学习和教学工具"，技术报告里有很多类似 Google 或 DeepSeek 报告的幕后巧思，连没考好的基准分数也照登。tancop 补充说最难得的是他们对挫折的坦诚：每次重启都记录了原因，还讲了因为网络安全数据集拖累编程基准而把它撤掉的事；唯一缺失的是数据集描述，仪表盘上只有 "dataset-zrso" 这类随机 ID。rao-v 随后举例说明仪表盘的门道：在 on-policy RL 里，负责生成 rollout 的模型总会落后于正在更新的模型（agent rollout 动辄几小时，不可能每次小改进都重新部署），而仪表盘上恰好有 `partial/avg_staleness`（落后了多少个微迭代）和 `train_infer_diff/new_infer/kl`（两者生成分布的 KL 散度）这两个指标，"多酷啊！"——这条引来十几个人催他写成博客。anvuong 则被仪表盘上一条公告逗乐："pro 运行在第 17 步因专家负载不均导致 GPU OOM 而重启，我们已调整训练并行策略"，"不知道为什么笑得这么厉害，太熟悉了"。rao-v 还推测这个仪表盘大概率基于 verl 搭建。

也有冷静的声音。embedding-shape 认为，无论人们对中国模型的不信任是否有根据，这个仪表盘都证明不了什么，"就像把 HTTP 服务器的 journalctl 日志贴到网站上，然后声称这证明 NSA 没在监听"。aarondong 则想到，这是否削弱了 Anthropic 的"蒸馏攻击"指控——人家有自己的 RL 环境；不过 RL 数据集仍不透明，什么也没被真正证明。

**训练成本的正确读法。** 多人注意到"350 万美元训出 Opus 级模型"的说法，Havoc 觉得即便不算工资也便宜得惊人；redox99、leothetechguy、tw1984 纷纷纠正：那只是 RL 阶段的成本，不含预训练。gigatexal 则认为，主动强调训练花了多少钱，明显是在揶揄美国前沿实验室动辄数千万到上亿美元的投入。chenzhekl 问 DeepSWE 分数明明还在涨为何提前停训，creato 认为曲线已趋平缓、最终节点甚至有点"挑选"之嫌，训得越久越可能被别家抢先发布；rao-v 补充说总有一刻你该拿学到的一切去训一个更好的基座。

**为什么 Flash 在网络安全上反超 Pro？** heyjstn 提问，T1ny 从公开仪表盘里找到了答案：Flash 训练数据中有 4% 是网络安全任务，而 Pro 是 0%。wren6991 则觉得仪表盘上一句"我们在接下来的 Pro 运行中移除了网络安全数据集，因为在 rollout 日志中观察到一些不良模式"，配上模型卡里的"环境加固……防止 reward hacking"，有点让人发毛："该不会是有人开始造太多回形针了吧？"Zaraif13 也质疑，官方就这样轻描淡写地说自己在探索 RSI（递归自我改进），难道不值得警惕吗？

**基准怎么看。** user43928 表示不信任任何 Opus 5 超过 Astra 或 Fable 5.1 的基准，只认为 Terminal Bench 4.0 和 ExploitGym 较合理，并把这两项与 DeepSWE 的排名整理了出来：在 Terminal Bench 4.0 上 MiMo-V2.6-Pro（34.9）与 GPT 6 Astra（59.6）差距明显。OsrsNeedsf2P 欣赏小米敢把自己排在 DeepSeek 等模型后面；deaux 则指出在最不饱和、也就最有意义的基准上 MiMo 是领先的。GodelNumbering 细看后惊叹：1T 参数的 MiMo 2.6 Pro 在 15 项基准中有 14 项领先 2.8T 参数的 Kimi K3（剩下一项接近打平）。drob518 注意到对比里没有 GLM 5.3。dom96 用自己的 KillSwitch-Bench（一门模型没见过的语言上的编程测试）实测，MiMo-V2.6-Pro 得 38.8，与 Muse Spark 1.3 相当但便宜得多，排在 Opus 5（66.9）、GPT-6 Astra（57.9）和 Fable 5.1（46.7）之后。XCSme 的测试则显示它比 Grok 4.7 更强也更便宜。spwa4 给出的粗略定位是：Flash 约等于 GPT-5.6 Luna 级别，Pro 约等于 GPT-5.6 Sol / Opus 5 级别。

还有一段术语科普：jwpapi 认为图里的 "Pareto Line" 用错了，帕累托应该是 80/20 法则；hashmush、abound 等人纠正说这里指的是帕累托前沿（Pareto front），即不被任何其他点严格支配的那条边界，和 80/20 法则只是同名。

**实际体验：强但爱"想太多"。** 这是最一致的反馈。jjcm 用 Pro UltraSpeed、Grok 4.7 和 GPT 6 Astra 做了"设计稿转 HTML"对比：MiMo 用了 36 分钟，Grok 4.7 25 分钟，Astra 19 分钟；UltraSpeed 的每秒 token 数确实快，但想得太多，总耗时反而最长，效果也最弱（雕像的手臂被裁掉、视差效果最差）。Saline9515、Imanari 也抱怨简单修改都会产生巨长的推理过程，"感觉像是实验室在用蛮力刷分，而不是真的更聪明"。embedding-shape 发现 Flash 反而比 Pro 推理得更久。XCSme 说 medium/high 档位 token 用量太大，请求在他设定的 16 分钟时限内全部超时。omani 反映 Flash 会陷入死循环。cmrdporcupine 的评价较均衡：开放式提示下它会过度规划、做出混乱的架构选择，但在精细的代码层面非常能干，擅长发现边界情况，连其他更强模型看过都没发现的问题也能找出来。MisterMunchkin 在自己熟悉的冷门领域测试后觉得效果不错，只是训练数据明显被"claudeslop"（Claude 式套话）污染了——随后 Bluestein、nullc 等人用一连串"load-bearing""that's on me""worth flagging"之类的 Claude 口头禅玩起了接龙。

**硬件与部署。** stymaar 贴出参数：Flash 309B/15B 激活，Pro 1.02T/42B 激活。syntaxing 感叹这些模型对只有 128GB 统一内存的人来说太吊胃口了。帖子作者 volf_ 分享了在双 DGX Spark 上跑 Flash 的 vLLM 配方，平均 25–35 tok/s。esafak 指出它在 Artificial Analysis 的智能/成本图上居于前沿，而且作为中国模型难得在响应时间上也不错，但担心其编程套餐不对缓存 token 打折。thrownawaysz 注意到 UTC+8 的 0 点到 8 点有 0.8 倍计费优惠，正好对应太平洋时区的上班时间。wkcheng 问企业里怎么用，ricardobeat 提到其欧洲 Token Plan 托管在阿姆斯特丹，但随后承认自己可能记错了是否提供零数据保留。

**中美之争占了大量篇幅。** MangoCoffee 猜测这就是 Dario 和美国大实验室都在呼吁"放慢 AI 发展"的原因——怕足够好的中国模型砸了他们的利润。jwolfe 追问：限制最强模型怎么能阻止便宜模型追上？qlte 的解释是：真正的目标是打压无法加装护栏的开放权重模型，把它们挤出美国企业市场；cogman10 预测会走"安全"路线，要求只有 Anthropic 和 OpenAI 能通过的繁琐测试认证。awongh 引用 Dylan Patel 在 Dwarkesh 播客的分析，认为中国实验室算力差了可能两个数量级，至少落后两三年；SwellJoe 反驳说它们事实上只落后几个月。margorczynski 认为中国终将因电力优势胜出，引发一长串关于电网、数据中心能耗的计算与争论。lwansbrough 问"还有人比起美国模型更期待中国模型吗"，引来大量表态：Zambyte 的总结被多人认同——"美国模型在能力前沿，中国模型在效率前沿，而问题在于中国模型对绝大多数应用来说已经够用了"；zmmmmm 说他看重的其实是控制权，开放模型让所有玩家保持诚实；也有 swingandamiss 表示不愿支持经济和军事对手，nsoonhui 则说 GLM 5.3 和 DeepSeek v4 在他的生产环境里死循环烧光了 OpenRouter 余额。

**鹈鹕测试。** simonw 照例放出 Flash 和 Pro 画的"骑自行车的鹈鹕" SVG。phainopepla2 说"可以相当有把握地说，他们没有在刷鹈鹕基准"；valdrinNereth 被 Pro 版画里多出的一条鱼逗乐。Kurtz79 问 simonw 是否考虑做 Blender 3D 版本，simonw 说他几周前已用 Astra 在 Blender 里做过一只 3D 鹈鹕，但因为需要完整的编程 agent 环境，还没把它定为正式基准。

最后几条值得一提：ignoramous 指出 MiMo 由前阿里、DeepSeek 员工罗福莉领导，这或许解释了小米的技术和市场策略与 DeepSeek 如此相似；est 则说这款模型和定价正符合雷军对小米的愿景——"科技公司里的 Costco"。
