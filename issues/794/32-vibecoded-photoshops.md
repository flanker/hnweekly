---
layout: article
title: "那些 vibecode 出来的 Photoshop 到底在哪？"
issue: 794
number: 32
category: design
original_url: "https://indiepixel.de/blog/posts/where-are-the-vibecoded-photoshops/"
hn_url: "https://news.ycombinator.com/item?id=48177228"
date: 2026-05-22
---

## 文章摘要

作者 gizmo64k 在文章里直面 2026 年最具杀伤力的指责：**"你这是 vibecoded 出来的"**——一句话否定整个项目，潜台词是"你只是写了个 prompt，没有真本事"。"Vibecoding"在批评者口中描述的是这样一种工作方式：**一个 prompt 进去，零验证，一个能跑的 app 出来**，作者还顺手挂上自己的名字。

作者的反击非常直接：**"那 vibecoded 的 Photoshop 在哪？vibecoded 的 Excel 在哪？任何需要架构判断才撑得起来的软件，vibecoded 的版本在哪？"** 答案是没有，因为根本没人能那样做。他把软件工程拆成三层：**Level 1 是语法和机械操作**（写循环、调 API、配置文件），AI 在这一层确实大幅降本；**Level 2 是设计判断**（数据结构选择、接口边界、错误处理策略）；**Level 3 是架构**（系统怎么演化、各模块怎么解耦）。**AI 主要降的是 Level 1 的成本，Level 2/3 仍然需要人**。

文章中段的洞察很有意思：作者认为"vibecoded"这个指控**本身就在干 vibecoded 的事**——未经验证、未经测试就广泛传播。那些最大声指控他人的人，**自己的身份认同恰恰建立在 Level 1 的工作之上**，AI 把他们手艺的市场价格压下去时，他们感到自己被淘汰，于是从 Level 1 工人转型成"vibecoded 警察"，靠攻击别人维持优越感而不是去升级技能。

整篇博客的论点最终落在：**AI 没有抹掉专业性，它只是把专业性的层级推得更高**。会写循环不再值钱，但能判断"什么时候该用循环、什么时候根本不该写这段代码"还是稀缺的。Photoshop 不会被 vibecode 出来，**不是因为 AI 不够强，而是因为做出 Photoshop 需要的判断本来就不在 prompt 里**。

## HN 评论精华

- **gizmo64k**（作者本人）：在评论区进一步澄清——其实他不是说"不可能出现新版 Photoshop"，而是 LLM 时代的真正趋势是另一种：**人们用 AI 写一个只满足自己需求的小工具，而不是去复刻通用工具**。Excel 的角色正在被 ChatGPT 取代——大量"原来要用 Excel 报表分析"的工作直接变成一个 prompt + 临时脚本。Photoshop 同理，**很多原本"用 Photoshop 改一下图"的需求已经被 ChatGPT/Gemini 吃掉**。

- **netdevphoenix**（最高赞）：把对 AI 的反对者形容为"二元思维"——没有人在 vibecode Photoshop 替代品，就像没有手机用户在直接竞争职业摄影师。AI 做的事和当年摄影、视频拍摄、录音技术一样：**降低门槛，让媒介变得普及，但低端品的质量也跟着下降**。

- **joenot443**（反驳）：这让他想起 2010 年代"每家会有 3D 打印机做小修小补"的预言——结果证明大多数非技术用户根本不想要 3D 打印机。**Vibecode 出的 app 如果不连接现成 API，本质上是孤岛**——问问你家人他们日常用的哪个 app 不依赖外部服务，数量很少。

- **saulpw**（神补刀）：3D 打印机的真正难点不是打印而是 CAD 设计——"现在大家终于在 vibecode 浪潮里发现，**写代码不是难的，软件设计和架构才是**。" **zeroonetwothree** 跟上："这从来就是难的部分，所以资深工程师才比应届生贵 5–10 倍，不是因为他们打字快。"

- **mitkebes**（乐观派）：但 AI 可能也能搞定 CAD 这一关——他试过让 Gemini 用 OpenSCAD 写参数化模型，对常见物件效果出乎意料地好，"测量、套参数、导出 STL"流水线很顺。

- **hombre_fatal**（实操派）：他自己刚 vibecode 了一个计步器 app，因为最流行那款开始收订阅费搞 Duolingo 式连击。重点是——**AI 赋能的不是"非技术人员变成开发者"，而是"原本有意愿但没动力 yak-shave 学开发的人现在能动手了"**。

- **zeroonetwothree**（成本派）：实操中算账并不划算——vibecode 自己的 app 可能消耗 $100–200 的 token 加上时间成本，付 $10/月订阅常常更便宜。**只在"市面上根本没有对应产品时"才值得自己写**——这种情况意外地多。

- **fragmede**：3D 打印问题已经解决——makerworld.com 上已经有几十万个开源模型，"很多人遇到坏件直接搜现成的"。这或许预示了 vibecoded apps 的未来：**真正流行的不是每个人写自己的，而是 marketplace + 微调**。
