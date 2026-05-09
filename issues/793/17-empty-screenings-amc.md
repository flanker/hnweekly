---
layout: article
title: "Empty Screenings：找出 AMC 几乎没人看的电影场次"
issue: 793
number: 17
category: show_hn
original_url: "https://walzr.com/empty-screenings"
hn_url: "https://news.ycombinator.com/item?id=48018066"
date: 2026-05-08
---

## 文章摘要

[Empty Screenings](https://walzr.com/empty-screenings) 是 Riley Walz（HN ID **walzr**）做的一个小网站，slogan 简洁到有点冷：**"AMC 大约 10% 的场次售出 0 张票。这个网站把它们找出来。去享受你的私人影院吧。"**

输入一个美国邮编，网站就列出附近 AMC 影院里目前已知没人买票或几乎空场的场次时间表，用户可以挑一个时间过去，大概率得到一整个放映厅的私人体验。Walz 的实现思路并不神秘——AMC 在线选座系统会暴露每场电影已售出的座位（这样你才能选剩下的位置），他把这一公开数据反复抓取并聚合，发现 0 张票的场次远比想象的多。整个项目背后没有 AMC 的官方授权，页面底部明确写着 "This site isn't affiliated with AMC Theaters."

这个项目的精髓是它揭示了**对外公开数据**和**社会观察**结合后的奇妙结果。10% 的电影场次完全没人买票，意味着影院其实在为大量"无人观众"放映，运营成本之大让人乍舌；但对消费者来说，这恰恰是个隐藏福利——只要花一张正常票钱（甚至 AMC A-List 订阅卡免费），就能独占一整个放映厅。Walz 此前还做过 [bop spotter](https://walzr.com/bop-spotter)、[every NYC bus stop](https://walzr.com/every-bus-stop) 等城市数据小项目，特点都是"用数据轻轻撬开一个本来看不见的角落"。

## HN 评论精华

- **"这种事真有人提前订票？"**（**MrBuddyCasino**）：最高赞评论质疑：现在还有多少人提前买票？工具的价值是否够大？这一句引爆了一场跨地域的电影院使用习惯讨论。

- **指定座位的全球差异**：**caymanjim**（北美）反映自己从没遇到过指定座位的影院；**stingraycharles**（欧洲）说 90 年代他们就指定座位，并描述了荷兰当年通过电话预订系统 BelBios 的流程；**pjmlp**（葡萄牙）说欧洲电影联盟（Europa Cinemas）的小型独立影院不指定座位，只有大型 multiplex 才指定；**JoshGlazebrook** 表示"现在几乎所有美国主流影院都指定座位"，临柜买票只剩烂位置；**vasco**（澳洲）说"北美以外几乎全是指定座位"。共识：**北美电影院的指定座位制是 2010 年代以后才普及的，疫情后基本完成。**

- **"半空场不靠工具也能找"**（**hombre_fatal**）：他自己买票时一直会看放映厅的座位图——空座位多就买，少就换一场。Empty Screenings 帮你免去了一家家点的麻烦。

- **AMC A-List 与 0 票场次**（**yieldcrv**）：他怀疑很多 0 票场次其实是 A-List 订阅用户预订了座位但临时不去，因为订阅免费让人没有"沉没成本"压力，因此无人到场。这解释了为什么座位图看上去满，影厅却实际空。

- **小镇放映体验**（**robinsonb5**）：英国小镇 65 座的影院，他经常买到"私人场"——上次自己一个人在黑漆漆的影厅看《The Mummy》，"忘了恐怖片不该一个人看"。

- **影院经营反思**：评论区普遍感慨——10% 完全空场说明影院产能严重过剩，电影行业商业模式正在瓦解。**ravenstine** 把锐减原因归为"指定座位 + 高端餐饮"让本应轻松随意的看电影变成一种正式安排，反而劝退了冲动型观众。

- **小有人提到 Alamo Drafthouse、Harkins** 这类区域院线在体验上比 AMC 好得多，会员制让他们的"空场"出现频率更低。

- **Walz 的项目精神**：评论提到他过去那些公共数据小项目，被认为是"data-driven city wandering"的典型代表——不大不正经，但每个都让你重新看到一座城市的某个被忽视的层面。
