---
layout: article
title: "体素中的华沙：把公开城市数据变成可漫游地图"
issue: 817
number: 58
category: fun
original_url: "https://p.migdal.pl/warsaw-vibe-voxel/"
hn_url: "https://news.ycombinator.com/item?id=50339221"
date: 2026-11-06
---

## 文章摘要

这个项目用类似 Minecraft 与 MagicaVoxel 的体素风格重建华沙市中心。作者 stared 在 Show HN 正文说明，用户可以改变日期、时间与天气，自由探索城市，也可以点击行人接管其移动。它主要针对桌面浏览器优化，同时尝试支持移动端。这里依据作者帖子与公开评论整理：直接读取站点只得到加载提示，本次没有实际进入三维场景验证操作或性能。

项目把多种公开数据叠加：OpenStreetMap 提供建筑、街道、人行道与电车轨道，波兰 GUGiK／Geoportal 的激光雷达提供高度及树木信息，航拍影像帮助确定颜色，华沙城市树木数据库提供单株树种以表现季节变化。这样的组合使场景不只是随机堆方块，而是带有可辨认的城市结构；作者也提供 [开源仓库](https://github.com/stared/warsaw-vibe-voxel) 供复用。

不过，公开数据自动转换并不能保证城市模型准确。作者称初稿容易生成，修整则要逐一检查断开的街道、悬空树冠、破损桥梁，并与照片比对。覆盖范围也在迭代，评论提出老城缺失后，作者回复已经加入。应把它看作仍在更新的互动地图，而不是完整测绘成果；具体地标、楼层与浏览器流畅度需要实地或交互核验。

## HN 评论精华

- [stared](https://news.ycombinator.com/item?id=50340662) 描述人工校对的重要性：模型能帮助处理开放数据，但细节修复远比生成初稿耗时。
- [sixtyj](https://news.ycombinator.com/item?id=50339906) 报告移动 Safari 重载；作者回应性能与功能之间存在取舍，主要优化目标是桌面端。不同设备体验并不一致。
- [stared](https://news.ycombinator.com/item?id=50345305) 回复已按需求加入老城，因此早期「没有老城」的评论不能直接当作当前状态。
- [parkersweb](https://news.ycombinator.com/item?id=50351106) 喜欢点击人物和车辆跟随移动的设计。这是评论者的实际体验，不是本次测试结果。
