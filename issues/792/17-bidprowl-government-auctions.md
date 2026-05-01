---
layout: article
title: "Show HN：我把 28 家美国政府拍卖网站聚合成了一个搜索"
issue: 792
number: 17
category: show_hn
original_url: "https://bidprowl.com"
hn_url: "https://news.ycombinator.com/item?id=47961378"
date: 2026-05-01
---

## 文章摘要

[Bidprowl](https://bidprowl.com) 是开发者 **scarsam** 推出的一个聚合搜索产品，它把美国政府体系散落在至少 28 个不同平台上的公开拍卖列表全部抓取、清洗、去重，统一成一个搜索框背后的标准化数据源。

**为什么需要这个产品**：美国政府拍卖体系极其分裂 —— GSA 卖联邦车队退役车辆，DLA Disposition 处理军用装备，US Marshals 通过 bid4assets 发售查封资产，PublicSurplus 服务学区与州机构，GovDeals 是数千个县市级机构的入口，Fannie Mae 与 HUD 拍卖法拍房…… 这些站点之间互不联通，且 UI 大多停留在 2008 年的水平。

**当前覆盖规模**：
- 聚合 26-28 家主要平台：GSA Auctions、GovDeals、Ritchie Bros、Purple Wave、Fannie Mae、GovPlanet、HiBid、Auction.com、PublicSurplus、JJ Kane、Bid4Assets、PropertyRoom、IRS Auctions、Municibid、DLA Surplus、Freddie Mac、US Marshals、AuctionZip、GovEase、Proxibid、HUD Homes、Hubzu、USPS Surplus、GovLiquidation、GSA Fleet、Treasury，加上加州、得州、佛州的州级 surplus 项目
- 截至上线时活跃 listing 数：作者在帖中说约 18 万条（180,276），首页展示 73,000+，每周新增约 53,000 条，每日刷新两次
- 品类分布（来自首页）：车辆 18,320、重型设备 16,621、通用 surplus 9,166、工具与工业品 8,650、办公家具 8,559、电子 4,378、地产 3,188、查封资产 2,631、医疗与科研 1,282、军剩 830

**技术栈**：Next.js + Postgres + 每个数据源独立的 TypeScript 爬虫 + 每日刷新。作者明确说部署在 **Vercel**（"我知道，正在迁移中…"）。

**最难的不是爬虫，是去重**：由于同一笔 Fannie Mae 法拍房会以三种不同地址在三个平台同时出现；同一型号的 2008 Ford F-150 在 GSA Fleet 和 PublicSurplus 看起来结构一样但 VIN 不同 —— 必须通过对元数据指纹化才能正确区分"同一物品"与"同型号不同实物"。

**商业模式**：纯聚合，免费。每一条 listing 都直接跳转到原始拍卖站完成竞价，"我们不碰你的出价，也不抽成"。Bidprowl 的赚钱方式是 SEO 流量 + 长尾关键词广告 —— 因为 "美国政府拍卖" 这种长尾词在 Google 几乎没有像样的答案页。每条 listing 还附带一个**deal score**（1-10 评分），基于价格相对品类中位数、出价速度、剩余时间和起拍价比率综合算出。

**作者展示的"奇货"清单**（文章发布时全部活跃）：
- 2000 年 Bell 430 直升机（行政机型），起拍 25 万美元，0 出价
- 1985 Cessna 182R，密苏里，起拍 3.3 万美元，0 出价
- M75 装甲人员运输车，挂在 Ritchie Bros，无人出价
- 一个全新未用的 Rolls-Royce 船舶推进器，起拍 50 万美元
- 2.3 公斤铱铂合金锭（警方查封品），52 次出价，目前 17.5 万美元
- 1927 年 Seagrave 消防车，"能跑能开有产权证"，起拍 2.4 万美元
- 一台来自名为 "Donkey & Burro" 的厂商的卡车安装式叉车

## HN 评论精华

- **作者 scarsam 自我补充**：在主帖下进一步说明了爬虫与去重的工程细节，并欢迎大家提问关于"如何爬那些非常不希望被爬的联邦站点"以及 deal score 算法。

- **xnx**：泼冷水 —— "三周前刚有过一个 GovAuctions 的 clone（指 govauctions.app）。" 暗示赛道已经拥挤。

- **player_piano**（GovAuctions.app 的作者本人现身）：发现了 Bidprowl 后通过自己的网站接到了"流量异常"告警 —— "上 Hacker News 让我自己的网站收到了 traffic spike 通知。说起来我过去一周在自己站上看到了大约 10 个不同来源的爬虫。" 言下之意：这一波"政府拍卖聚合站" 不只是双雄，而是已经有一群在抓同一个数据池。

- **rovr138 / kjs3** 顺势讨论了"诱饵条目（fictitious entries）"反爬策略 —— 维基百科条目里那种故意埋的虚假信息，可以用来追踪是谁在拷贝你的数据库。**player_piano** 表示这听上去是个不错的周末创作活动："批量 lot：15 万辆政府剩余 Cybertruck，仅轻度使用。"

- **kjs3** 顺嘴提了一句："顺便问一下，我们是不是已经集体意识到 Vercel 挺烂的了？我没收到通知。" —— 引出对 Vercel 价格和 Next.js 体验的吐槽支线。**zdragnar** 较平衡地评价："最近在 HN 和 Reddit 上几乎看不到对 Vercel 的正面评价，大多数批评集中在价格或 Next.js。可能有大量沉默用户在静悄悄地用，但他们安静，所以听不到。"

- **83**：典型用户视角的好评 —— "你的网站太棒了。我也受够了每天去看五个 UI 像 2005 年的政府拍卖网站。" **player_piano** 接话："谢谢，目前最大的亮点是我朋友通过我这个站买到了一台工业车床。"

- **craftkiller**：意外的教育价值 —— "感谢你的网站！发帖之前我都不知道政府还在线拍卖房子。顺着链接我还学到了 FHA 100 美元首付项目（指美国住建部的 HUD $100 down 计划）。" **edm0nd** 现实主义反驳："但是真要住进 HUD housing 区你愿意吗？通常不在好地段、犯罪率高、房子还要花 5-6 位数翻新。除非你急着要个住一年的窝，否则有更好的地方花钱。"

- **gavmor**：报告了一个数据质量问题 —— "你的'Madison, WI'里很多其实在 Greenbay。" 暗示去重和地理标准化还有提升空间。

- **bbstats**："第一个搜的就是 Pokémon 卡，发现一些出价比市场价高 50% 的 listing —— 要么是 shill bidding（哄抬出价），要么就是无脑盲拍。"

- **yodon**：性能反馈 —— "首页能加载，单个州的页面好像加载不出来。"（HN hug of death 的经典症状。）

- **godzillabrennus（同类项目大背景观察）**："大家正在意识到很多政府数据源都是免费的，并基于此 vibe-coding 一堆应用。" 这句话精准地概括了当下这一波"聚合 + AI 包装公开数据"创业潮。

- **baby_souffle**：提出明确的功能缺口 —— "需要一个把搜索结果限定在某个 zip 码或地理点 X 英里范围内的过滤器。"

- **millzlane**：警告数据准确性问题 —— "这个站到处都是错信息和坏链接。"
