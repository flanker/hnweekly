---
layout: article
title: "Glider 4.0 的非官方网页版，连房间编辑器也带来了"
issue: 816
number: 69
category: fun
original_url: "https://dbohdan.github.io/glider/"
hn_url: "https://news.ycombinator.com/item?id=50237019"
date: 2026-10-30
---

## 文章摘要

这个项目把 John Calhoun 的 Macintosh 游戏 Glider 4.0 移植到浏览器，并保留 Room Editor。根据[项目 README](https://github.com/dbohdan/glider)，实现逐单元把 THINK Pascal 源码转成 TypeScript，再补足所需的 Macintosh Toolbox；发布给玩家的是源码移植，不是运行整套 Mac 系统的模拟器，也不需要 Mac ROM。

作者称移植由人指导、Claude 实现，双方共同设计。更重要的是验收方法：开发时在确定性的模拟环境里运行原版，给原版和移植版相同输入，再比较像素、声音与时序。有差异时先采集原版证据，而不是直接修改到“看起来差不多”。

项目还增加键盘、手柄及触屏支持，并用浏览器存储保存数据。README 明确列出保留的原版行为与已知问题。这里依据源码仓库说明，未实际游玩或独立复跑其全部对照测试。

## HN 评论精华

- **原作者注意到编辑器。** JKCalhoun 赞赏这个版本连独立的 Room Editor 也移植了，强调创作工具对保留游戏体验的价值。
- **AI 适合复活旧软件吗？** devindotcom 认为这类难以接触、调试和移植的老软件，是 AI 工具有益的使用场景之一。这是对案例的评价，不能泛化为所有移植都会成功。
- **真实设备暴露细节。** joelkesler 报告 iPhone Safari 上快速点击按钮会触发页面缩放，说明像素对照之外还需要设备交互测试。
- **相关项目并不相同。** daviewales 推荐 Glider Pro 的 Aerofoil 移植，statico 则讨论其他经典 Mac 应用网页版。它们是不同版本与技术路线，不能混为本项目的实现。
