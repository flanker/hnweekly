---
layout: article
title: "Skub -- 一款滑块解谜浏览器游戏"
issue: 788
number: 59
category: fun
original_url: "https://skub.app"
hn_url: "https://news.ycombinator.com/item?id=47506958"
date: 2026-04-03
---

## 文章摘要

Skub（发音 [sgob]）是一款受经典桌游《Ricochet Robots》（弹跳机器人）启发的滑块解谜浏览器游戏，由开发者 Kasper Storgaard 制作。游戏规则简单而富有挑战：将冰球滑动到目标位置，用最少的步数获胜。

游戏界面简洁优雅，采用深色主题设计。棋盘是一个带有墙壁障碍的网格，玩家需要滑动冰球（圆形标记），冰球会沿滑动方向一直移动直到碰到墙壁或方块障碍物才停下。棋盘上还有方形的阻挡物（blocker），增加了解谜的复杂度。游戏提供每日谜题（Daily Puzzle），按难度分级（如 medium），同时有编号存档系统方便回顾。新手可以通过"Learn the basics"教程入门，老玩家可以浏览谜题存档（Archives）挑战历史关卡。

技术上，Skub 是一个使用 Deno Fresh 框架构建的 Web 应用，代码开源在 GitHub（kasperstorgaard/ricochet）。游戏支持 undo/redo 操作，有分享功能和玩家档案系统。网站注重隐私，明确表示不分享或出售用户数据，仅使用 cookie 追踪游戏方式用于改进。

## HN 评论精华

**浏览器兼容性问题**：daisydevel 发现在游戏页面按浏览器的前进/后退按钮会改变游戏状态而非页面导航。开发者 kasperstorgaard 承认这是个好发现，应该在状态变化时替换历史记录而非推入新记录，并指出游戏面板内已有专门的 undo/redo 按钮。

**用户体验改进建议**：stanac 反馈每次完成关卡都要输入用户名很烦人，询问能否记住之前的选择。开发者回应说已经有相关机制，首次输入后应该会通过 cookie 存储匿名用户 ID 实现自动提交。stanac 还提到 Firefox Windows 版中 Tab 键导航不一致的问题。

**名字的梗**：chuckadams 贴出了一个 Perry Bible Fellowship 漫画的链接，该漫画中有个叫"Skub"的虚构产品引发了两派人的激烈争论（"Pro-Skub" vs "Anti-Skub"），这个梗在网络上广为流传，暗指人们会为最无聊的事情争论不休。

评论数量不多但质量较高，主要集中在具体的 UX 改进建议上，开发者响应迅速并积极采纳反馈。
