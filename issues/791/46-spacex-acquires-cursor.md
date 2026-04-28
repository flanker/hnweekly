---
layout: article
title: "SpaceX 称已达成协议以 600 亿美元收购 Cursor"
issue: 791
number: 46
category: startup_news
original_url: "https://twitter.com/spacex/status/2046713419978453374"
hn_url: "https://news.ycombinator.com/item?id=47855293"
date: 2026-04-27
---

## 文章摘要

SpaceX 在 X（Twitter）官方账号上发布声明，称已与 AI 编程工具公司 Cursor（公司主体 Anysphere）达成协议。由于 Twitter 链接通常无法直接抓取，本节内容主要根据 HN 讨论和同时被引用的 Reuters、Bloomberg、Guardian、NYT 报道整理。

交易的真实结构比标题更复杂：根据 Bloomberg 与 SpaceX 自身公告（参见 cursor.com/blog/spacex-model-training），这并不是一笔确定的收购，而是一份"二选一"协议——SpaceX 在今年晚些时候**有权（option）**以 600 亿美元收购 Cursor，或者支付 100 亿美元换取双方的合作；如果 SpaceX 决定放弃行权，仍需向 Cursor 付清 100 亿美元的协作 / 分手费。换言之，SpaceX 实际上花了 100 亿美元买下了一份"看涨期权"——锁定 Cursor 在自己估值持续走高时的交易窗口，同时获得 Cursor 现有用户与训练数据的访问权。Cursor 自己的官方博客只强调"模型训练合作"，对那份 600 亿美元的期权只字未提。

战略意图被 HN 多位评论者解读为三层叠加：第一，xAI 的 Colossus 数据中心今年将建成约 2GW 的 GPU 算力，如果没有大客户就会闲置；Cursor 是当下少数能消化巨大 token 预算、又能提供高质量人类反馈数据的产品。第二，xAI 在企业级几乎零市场份额，而 Cursor 据称已渗透 NVIDIA、众多财富 500 强工程团队——SpaceX 通过收购权"借壳"接入企业渠道。第三，SpaceX 即将 IPO，需要一份壮观的"AI + 火箭 + 卫星 + 通信 + 编程"全栈故事来支撑万亿美元级估值；把 Cursor 的 ARR 与一份 600 亿美元期权打包进 S-1，远比承认增长见顶要好看。

但 HN 上的怀疑情绪明显占了上风。多数人觉得 600 亿美元给一个 VS Code 分叉 + 一个基于 Kimi K2 微调的 Composer 模型，是 dot-com 级别的离谱估值。再加上 Cursor 用户最近一年大量流失到 Claude Code、Codex、OpenCode，"6B 都嫌贵"的呼声不少。也有评论者把交易类比为 2008 年的 CDO（用 SpaceX 的 AAA 资产去打包一堆 subprime 资产），或者把它和 WeWork、推特收购案并列为 Elon "shell game" 的最新一章。

## HN 评论精华

- **Lonestar1440**：把这笔交易重新解读为最关键的"期权 + 服务"框架："SpaceX 花 100 亿美元买了一份 600 亿美元的看涨期权外加一堆服务。如果到行权日 Cursor 实际不值 600 亿美元，他们可以让期权过期；如果值更多，他们能拿到一笔好买卖。如果那 100 亿美元的服务本来就值 80 亿美元，那这单子很难亏。"——这是 HN 上少有的、能让这桩看似离谱的交易显得合理的视角。

- **nikcub**：给出了最详尽的战略多因素分析：(1) xAI 今年将有约 2GW 闲置 GPU 算力等待消化；(2) 即使 Cursor 在消费者侧热度退潮，它依然握有大量"开发者真实工作"训练数据；(3) Cursor 的企业关系是 xAI 完全没有的；(4) Cursor 长期以零售价向 Anthropic/OpenAI 买 token、毛利岌岌可危，被收编后能切到内部模型；(5) 这种"X 美元"作价方式对 SpaceX IPO 估值是友好的。

- **alyxya**：从协同角度看好——SpaceX 有算力没 LLM 训练（尤其 RL）人才，Cursor 有 RL 栈和团队但没自有基模和算力。Claude Code 和 Codex 的领先势头已经很大，两家如果不抱团基本都没好下场。

- **AirMax98**：尖锐质疑：Cursor 显然在走下坡路、IDE 这条路在 CLI 兴起后可能根本不是正确赛道，"我看不懂这跟航天有什么关系，但作为软件人，我看不出这是一个好决策"。

- **AJRF**：拿出实地数据——他在一个 1000+ 开发者的 Discord 群做投票，今年一月时已有 80% 把 Cursor 换成了 Claude Code。即便有选择性偏差，他的判断是 Cursor 正被前沿模型实验室的自研产品蚕食。

- **lemonish97** / **lossolo** / **gip**：从财务工程角度看穿这一切——用 SpaceX 股票付账可以同时做到三件事：抬高 SpaceX 估值（IPO 前需要更宏大的故事）、强行催热 xAI 用量、把烫手 Cursor 老股东置换为 SpaceX 股东。"Elon 的故事力远比基本面重要"。

- **5129ah**：用最辛辣的反讽：「我也被'授权'年内以 30 万亿美元收购 Tesla 或者支付 5000 亿美元做合作，请相信我，这一切都会发生。」直接戳穿"agreement to acquire"这种表述的水分。

- **mrcwinn**（少数派看多）：劝大家别只盯着 Elon 个人爱憎——他在做的是同时锁定营收、锁定 GPU 客户、锁定未来"在太空运行 GPU"愿景的拼图，对 Cursor 而言则换来了 Anthropic / OpenAI 之外的去风险算力承诺。"Laugh all you want, he may have the last laugh."

- 大量评论的反应是"立刻取消订阅，转 Claude Code / Codex / OpenCode / Windsurf"——比如 **darksaints**、**Marciplan**、**d1egoaz**、**alphabettsy** 等，反映出 Cursor 用户中存在一个相当强的"反 Musk"心智，这类用户流失对 Cursor 后续品牌价值是个真实的压力测试。
