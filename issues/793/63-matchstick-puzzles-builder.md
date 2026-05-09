---
layout: article
title: "别只玩我的火柴棒谜题，几秒钟就能自己造一个"
issue: 793
number: 63
category: fun
original_url: "https://mathstick.github.io"
hn_url: "https://news.ycombinator.com/item?id=47983485"
date: 2026-05-08
---

## 文章摘要

Mathstick 是一个网页版的**火柴棒谜题（matchstick puzzle）**游戏，本次发布的是 v2。火柴棒谜题是一类经典的视觉逻辑题——画面上用若干根"火柴棒"摆出数字、形状或方程，玩家需要**只移动一根火柴**让它变成正确的等式或满足题目要求。游戏的标语就是 "Fix It By Moving One Stick"。

v2 最大的新功能是 **Puzzle Maker**：玩家可以自由地拼放火柴棒做出自定义题目，再生成可分享的链接发给朋友"上头"。作者在 HN 自评论里贴出了一个示例链接（一长串 base 编码的题目数据放在 URL 参数里），说明谜题是**纯客户端编码**的、不需要后端。除此之外，游戏延续了 v1 的**关卡解锁** + **火柴棒货币**机制——通关获得火柴棒，用来解锁下一关或购买道具，难度随进度递增。

进度全部保存在浏览器 localStorage 中，不需要登录，但代价是清缓存或用无痕模式会丢档。整个项目是开源的纯静态网站（hosted on `mathstick.github.io`），搭配音效开关、数据管理等基础设置选项。作者还特别建议玩家**先玩一玩 Puzzle Maker**——亲手摆弄火柴棒能更直观地建立"哪些数字之间只差一根"的图形直觉，再去解高难度关卡会顺很多。

## HN 评论精华

1. **trangram（作者）**：在评论区直接介绍 v2，并给出他自己用 Puzzle Maker 制作的示例链接邀请大家试玩、留言。他还特别建议先玩 Puzzle Maker 入门——"learn-by-doing" 比硬解题更能建立对数字形状的直觉。

2. **vunderba**：报了一个**典型的拖拽 bug**：在小尺寸视口下，如果不小心把火柴棒拖出屏幕外，它可能"卡"在边缘动不了，恢复不回来。这是 web 拖拽游戏里非常常见的边界情况，开发者要做兜底处理。同时附上了维基百科 *Matchstick puzzle* 词条作为科普。

3. **a_t48**：故意把 INT64_MAX 这种极端数字塞进 Puzzle Maker 想看效果，结果**Puzzle Maker 检测到了 12 个"相同"的解**——明显是 solver 的去重逻辑出了 bug。他直接 @了作者，是非常具体的复现报告。

4. **trangram（作者再次发言）**：再次推荐 Puzzle Maker 作为入门工具，"边做边学"比直接解硬题更能把握火柴棒数字形状的规律。

5. **eager_learner**：典型的初学者好奇心——问作者这种小游戏需要多少 JS / CSS 功底？读完《Eloquent JavaScript》加任意一本 CSS 入门是否就够？这条回复反映了 mathstick 的代码量虽然不大、但对自学者来说是一个**够得着的练手项目**。

6. **ncruces**：建议作者**加一个 PWA manifest**，这样在 Android 上就能"安装到桌面"作为独立 app 使用。对于这种"打发时间"的小游戏，PWA 是非常合适的分发方式，成本很低。

7. **yuppiepuppie**：提示作者把项目提交到 [hnarcade.com](https://hnarcade.com)，那是一个聚合 HN 上分享的网页小游戏的目录站——好作品值得被更多人看到。

8. **dvh**：眼尖地指出页面上 "invetory" 是 typo（应为 inventory）——HN 评论区永远不缺 proofreader。
