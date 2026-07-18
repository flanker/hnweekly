---
layout: article
title: "这位环保主义者把 40TB 公共数据变成了一款电子游戏"
issue: 801
number: 61
category: fun
original_url: "https://blog.exe.dev/meet-the-conservationist-who-turned-40-terabytes-of-government-data-into-a-video-game"
hn_url: "https://news.ycombinator.com/item?id=48906893"
date: 2026-07-17
---

## 文章摘要

这篇文章讲述了奥地利环保主义者 Raffael Hickisch 的故事——他借助 AI 与云计算，把海量公共数据变成了保护工作的实用工具，还顺手做了一款电子游戏。

Hickisch 于 2014 年参与创建了中非共和国的 Chinko 自然保护区，通过公私合作的方式管理受保护土地，横跨数万平方公里。他的核心工作是用数据分析追踪人类活动轨迹、执行土地使用政策、打击滥砍滥伐。为此他整合了大量公开数据集：NASA 的火点数据、人类聚居地信息、森林砍伐记录、奥地利的 LIDAR 激光雷达扫描（单个文件就有 15GB）、护林员的 GPX 轨迹文件，以及高分辨率航拍影像。

真正让这个项目出圈的，是他基于这些数据做的一款"开拓者"（Settlers）风格的游戏（siedler-oesterreich.exe.xyz）：玩家可以购买地块、种树造林、寻找宝藏，而游戏中呈现的其实是真实的 LIDAR 数据，连每棵树的高度都是实测值。这让枯燥的环境数据变成了一个可玩、可教育的交互界面。

技术上，他用 100 台并行的虚拟机处理了约 40TB 的数据，覆盖了奥地利大约 30% 的国土；还做了一个名为 "Five Megapixel Conservation" 的应用来可视化全球保护工作。文章的核心观点是：这类过去需要花费数万美元、雇专业开发者才能搞定的工具，如今被 Hickisch 用开源方式民主化了。他说，现在小国也"拥有了和资金雄厚行业同样的工具"，能够独立监测自己的保护区。（本文发布在 exe.dev 平台上，该平台正是他搭建这些应用所用的工具。）

## HN 评论精华

- **crawshaw**：帮其他找不到游戏入口的读者贴出了游戏链接 siedler-oesterreich.exe.xyz。他还评论说，Medium、Substack、YouTube 之所以流行，靠的是它们的内容发现机制，而不是因为托管文字或视频有多难。

- **jparishy**：分享了一个高度相关的类似项目——他正在用费城的市政开放数据做一款以费城为主题的开放世界游戏，字里行间满是对这座城市的热爱。评论者 **alwa** 也盛赞其中透出的"费城情怀与奇思妙想"。这条讨论说明"把公共数据变成游戏"正在成为一股小潮流。

- **jparishy**（另一条）与 **iberator** 就"15GB 到底算不算大"展开争论。jparishy 认为 2026 年下 15GB 数据根本不算什么，很多人下个游戏或追剧都不止这个体积；iberator 则提醒：并非人人都有又快又不限量的网络，他用的 4G 手机限速 40Mb/s、每月只有 200GB 流量。

- **entropicdrifter**：指出一个技术细节——流式加载虽然意味着数据要完整传输，但客户端设备在任一时刻并不会渲染完整体积的下载内容，因此海量 LIDAR 也能在浏览器里跑起来。

- **pistoriusp** 与 **leereeves**：讨论了地理语境。有人强调要把 Hickisch 的工作放在撒哈拉以南非洲的现实中理解；leereeves 澄清，游戏里用的其实是 Hickisch 家乡奥地利的数据，两者语境不同。
