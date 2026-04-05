---
layout: article
title: "CSS 注定被 DOOM 占领 -- 用 CSS 3D 渲染 DOOM"
issue: 788
number: 54
category: fun
original_url: "https://nielsleenheer.com/articles/2026/css-is-doomed-rendering-doom-in-3d-with-css/"
hn_url: "https://news.ycombinator.com/item?id=47557960"
date: 2026-04-03
---

## 文章摘要

Niels Leenheer 实现了一个令人叹为观止的技术演示：完全使用 CSS 3D 渲染经典 FPS 游戏 DOOM，没有使用 Canvas 或 WebGL。游戏中的每一面墙壁、地板、木桶和怪物都是一个 div 元素，通过 CSS 3D transforms、数学函数、@property 自定义属性、clip-path、锚点定位（anchor positioning）以及 SVG 滤镜等技术实现了完整的 3D 第一人称射击体验。

该项目的核心技术包括：利用 CSS 3D transforms 构建透视空间，通过 CSS 数学函数处理坐标计算，使用视口裁剪（viewport culling）技巧优化性能（仅渲染玩家视野内的元素），以及通过 animation-delay hack 模拟条件逻辑。游戏逻辑部分运行在 JavaScript 中（作者提到使用 Claude 基于原始 DOOM 源码生成了近似的游戏循环），但所有渲染完全由 CSS 完成。在线 demo 可在 cssdoom.wtf 体验，在 Safari 移动端和 Firefox 上运行效果尤佳，Firefox 的 WebRender 引擎因将大量渲染任务放在 GPU 上表现尤为出色。整篇文章约 4200 字，阅读时间约 20 分钟。

## HN 评论精华

**CSS 图灵完备性引发安全担忧**：yourapostasy 担心 CSS 越来越接近图灵完备会开辟新的攻击向量。rebane2001（CSS x86 模拟器作者）指出 CSS 实际上已经图灵完备，并表示自己也曾计划用纯 CSS 实现 DOOM，但看到依赖 JS 的项目登上首页感到有些沮丧。

**浏览器兼容性与性能**：多位评论者测试了 demo。bethekidyouwant 惊讶于它在 Safari 移动端完美运行。lights0123 赞扬 Firefox 的 WebRender 在元素移动方面的 GPU 加速表现优于 Chrome。nine_k 报告 Firefox 运行流畅但按键映射有问题（Alt 键会触发菜单）。

**CSS 作为编程语言的争论**：大量评论讨论了 CSS 是否应该继续向编程语言方向演进。bradley13 质疑这是证明 CSS 伟大还是 CSS 已经"跳过鲨鱼"。titzer 认为这是"抽象反转"的典型案例。Levitating 认为 Qt6/GTK 等 UI 框架在布局方面优于 CSS flexbox。pier25 则表示自己写了 20 多年 CSS，现在是最好的时代。

**实用性与趣味性**：socalgal2 警告不应在实际项目中使用 3D CSS 渲染游戏，因为 CSS 3D 存在 O(N^2) 的平面交叉问题。pverheggen 对视口裁剪技巧印象深刻。h1fra 发现可以通过删除 div 来实现"穿墙挂"，ehsankia 进一步指出给 .wall 加 opacity: 0.7 可以实现经典透视挂效果。

**怀旧与展望**：batisteo 回忆起当年需要 4 张 gif 才能实现圆角的日子。MrDOS 提到 2006 年 Ars Technica 发布永远跳票的《永远的毁灭公爵》将成为浏览器游戏的愚人节文章，如今看来令人感慨技术进步之大。
