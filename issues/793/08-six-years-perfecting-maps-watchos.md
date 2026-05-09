---
layout: article
title: "六年只为做好 watchOS 上的地图"
issue: 793
number: 8
category: favorites
original_url: "https://www.david-smith.org/blog/2026/04/29/maps-on-watchos/"
hn_url: "https://news.ycombinator.com/item?id=47990606"
date: 2026-05-08
---

## 文章摘要

独立开发者 David Smith（代表作 Pedometer++、Watchsmith）写下了他在 watchOS 上做地图的六年历程。文章的起点是 2020 年——当时他第一次尝试给徒步用户在手表上提供地图，方案是在服务器端实时渲染地图图块再传到手表。这个原型证明了"在手腕上看地图"是有用的，但缺点也很明显：必须时刻联网，离线场景几乎不可用，电量也吃不消。

2021 年开始，Smith 转向**自建渲染引擎**。由于 watchOS 只允许 SwiftUI，而 SwiftUI 当时连基本的地图绘制能力都没有，他不得不一笔一画地用 SwiftUI 原语去绘制矢量瓦片。这一选择带来的副作用反而成了优势——同一套引擎可以原生集成进 widget 与复杂功能（complications）。他在文中坦承，自己反复在"指标视图 vs. 地图视图"的模态切换上做过妥协，但始终觉得"这是个不舒服的折中"，直到设计师 Rafa Conde 介入，最终敲定让地图作为主界面，指标叠加在左上角，点击地图才进入浏览模式。

最戏剧性的决定是**没有用 MapKit**。Smith 解释，MapKit 在 watchOS 上的可定制性、暗色模式控制和地形细节都不够好。他直接联系了知名独立制图师 Andy Allan（OpenStreetMap 生态中的资深人物），定制了一套针对 watchOS 小屏可读性优化、并兼容 watchOS 26 Liquid Glass 的瓦片样式。从颜色对比、文字密度到等高线粗细都重新调过——这是一种"地图老派工艺"在新硬件上的重生。

文章的潜台词是一种"自用驱动产品"的哲学：Smith 自己是远足爱好者，深知在荒野中"频繁、轻松地确认位置"有多关键，于是把这种需求变成了功能优先级。第八版 Pedometer++ 在 2026 年 4 月发布，正是这六年迭代的结晶——它告诉我们：好产品不一定来自更大的团队，而来自一个肯在窄屏上死磕六年的人。

## HN 评论精华

1. **apt-apt-apt-apt**：解释了技术核心——Smith 不是在做"静态图片地图"，而是真正的矢量瓦片渲染。也就是说河流标签会沿着河岸弯曲、缩放时不糊，这才解释了为什么需要专业制图师介入而不是直接用任何现成 SDK。

2. **Amorymeltzer**：盛赞 Smith 的专注度——"He really is _such_ a committed and dedicated developer"。Watchsmith 多年的积累让他对 watchOS 的奇怪限制了如指掌，这是地图项目能成立的隐形前提。

3. **thrownawaysz**：把矛头对准 Apple——主打户外的 Watch Ultra 居然没有原生徒步/地形地图，对一台号称"探险者装备"的产品来说"是一种失败"。

4. **kumarvvr**：反驳上面观点——为什么所有事都要 Apple 自己做？第三方独立开发者反而能比 Apple 更早、更细地满足专业人群的需求，这正是平台生态的意义。

5. **ndr42**：吐槽 App Store 页面体验——多个价位并列展示，看不出哪个是订阅哪个是一次性购买，对潜在用户极不友好。

6. **raylad**：补充定价细节——基础计步功能免费，地图与轨迹相关的高级特性是 $29.95/年订阅。在独立开发者经济里这是一个相对克制的价格点。

7. **joshstrange**：从生态视角发问——Apple 应该学会拥抱社区创新而不是动不动 sherlock（即把第三方功能直接做进系统、抢走用户）。Pedometer++ 是个范本：好产品该被鼓励长大，而不是被吞掉。

8. 不少评论也提到一个细节：Smith 的实现说明"SwiftUI 在 watchOS 上画出像样矢量地图"是可行的，这对其他想做手表小工具的开发者是技术信号——以前大家都觉得 watchOS 太受限以至于做不了重 UI，事实可能并非如此。
