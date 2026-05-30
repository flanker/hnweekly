---
layout: article
title: "Mistral 收购 Emmi AI：欧洲押注「工业物理 AI」这条窄赛道"
issue: 795
number: 49
category: startup_news
original_url: "https://emmi.ai/news/mistral-ai-acquires-emmi-ai"
hn_url: "https://news.ycombinator.com/item?id=48197995"
date: 2026-05-29
---

## 文章摘要

法国 AI 公司 **Mistral AI** 宣布收购奥地利工程 AI 初创公司 **Emmi AI**，官方称其为「欧洲最重要、最具战略意义的 AI 收购之一」。Emmi AI 总部位于奥地利**林茨（Linz）**，由首席科学官 **Johannes Brandstetter** 等人创办，团队规模超过 30 名研究员与工程师，专注于面向工业仿真的**物理 AI（Physics AI）**模型——用神经网络替代传统的计算流体力学、有限元等计算密集型方法，服务能源、汽车、半导体、航空航天等行业。

Emmi 的技术栈颇具看点：底层是 **Noether 框架**，产品线包括用于飞机机翼实时验证的 **NeuralWing**、面向注塑成型的大型工程模型 **Neuralmould**、可扩展至 **1 亿（100M+）网格单元**的 CFD 神经代理模型 **AB-UPT**，以及 2025 年 9 月开源的颗粒流仿真模型 **NeuralDEM**。这些**神经代理模型（neural surrogate）**的核心卖点是把原本需要数小时的仿真压缩到接近实时，从而支撑「数字孪生」式的工程研发。

商业层面，Emmi 在 2025 年 4 月完成了 **1500 万欧元**种子轮，由 3VC、Speedinvest、Serena、PUSH 等投资，号称「奥地利史上最大种子轮」。收购后，林茨将成为 Mistral 继巴黎、伦敦、阿姆斯特丹、慕尼黑、旧金山、新加坡之后的又一办公地点。战略意图很清楚：把 Mistral 的大语言模型能力与 Emmi 的物理仿真能力打包，做一套面向制造业 R&D 的整合 AI 栈，从而在 OpenAI、Anthropic 扎堆的通用模型之外，切入一个别人忽视的垂直领域。

## HN 评论精华

- **MeteorMarc**：提醒大家光刻机巨头 **ASML 是 Mistral 的重要投资方**，这让 Mistral 的「工业 AI」野心显得更可信。
- **SyneRyder**：引用 ASML 的措辞——双方有「探索在 ASML 产品组合及研发、运营中使用 AI 模型」的长期合作协议，但具体在做什么并不清楚；Mistral 的 Forge 页面只提到「在驱动其最复杂系统的专有数据上训练模型」。
- **yieldcrv**：泼冷水，认为 ASML 投资 Mistral 跟所谓「协同效应」无关——欧洲风险投资生态贫瘠、可选标的少，ASML 拿 EUV 印钞机赚的钱做点流动性差的股权投资，跟它在芯片供应链里的瓶颈地位毫无关系。后续他进一步抨击欧洲创投生态「是个笑话」。
- **stingraycharles**：称一位在 ASML 工作的朋友透露，这笔投资是「自上而下」被相当用力地推动的，并非 ASML 真正的战略布局，本质是「欧盟主权 AI」的政治叙事。此说法随即被 **throw1234567891** 质疑（「你朋友不该跟你说这些」）、被 **petcat** 讽为「这里的人就爱编故事」。
- **sho_hn**：反驳对欧洲的双标——美国和中国政府同样在关键技术上做战略投资和扶持，「凭什么只有欧洲人这么做就要被骂」。
- **kergonath**：指出 ASML 只是 Mistral 「面向公众的、光鲜的合作方」；Mistral 还在为法国政府和国防工业做不会公开宣布的项目，那些才是真正赚钱的。他补充 Mistral 擅长帮客户自建基础设施、安全部署模型、处理机密数据，并在自建数据中心。
- **sravanipuuta**：认为欧洲主权这条线让 Mistral 的战略从外部很难判读——如果相当一部分收入来自欧盟政府和国防合同，那么拿它跟 OpenAI/Anthropic 的公开基准对比就「用错了记分牌」，因为优化目标完全不同。好奇这些合同是按席位、私有化部署还是「带主权密钥的托管」来结构化的。
- **dainank / barrenko**：补充 Mistral 与卢森堡政府、以及（据称）欧盟本身都有战略合作。
- **SilverElfin**：表达了最常见的疑虑——看到标题时持怀疑态度，至今仍是。AI 用于制造业、做别人忽视的垂直确实是个差异化方向，但他真正好奇的是「Emmi 到底造出了什么？谁在用？」官网上找不到任何具体的 demo。
