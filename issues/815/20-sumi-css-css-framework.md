---
layout: article
title: "Sumi.css：一个无类优先的极简 CSS 框架"
issue: 815
number: 20
category: show_hn
original_url: "https://github.com/vladocar/Sumi.css"
hn_url: "https://news.ycombinator.com/item?id=50180039"
date: 2026-10-23
---

## 文章摘要

Sumi.css 是 Vladimir Carrer 发布的一个极简 CSS 框架，名字取自日语「墨」（sumi）。它的定位是「无类优先」（classless-first）：你只写普通的语义化 HTML，页面就已经像是做完了的样子；只有想要某种特定效果时才需要加 class。

**核心卖点。** 整个框架只有一个文件，不需要构建步骤，压缩并 gzip 后 6.7 KB，没有 JavaScript，也没有依赖。标题、表单、表格、卡片、手风琴、模态框和导航栏都只靠 HTML 元素本身就能得到样式。它自动跟随访客系统的明暗模式，也可以用 `data-theme` 强制指定。所有颜色、字体、尺寸、圆角都是以 `--s-` 开头的 CSS 自定义属性，方便换主题。元素样式统一包在 `:where()` 里，特异性为零，所以你自己写的任何普通选择器都能直接覆盖，用不着 `!important`。可访问性方面，它有清晰的焦点环，用 `aria-invalid`、`aria-busy`、`aria-current` 等 ARIA 属性驱动状态，并支持「减少动画」偏好。

**哪些元素会被美化。** 标题、段落、列表、引用、代码块等使用流式字号和一致的垂直间距；`<nav>` 里放两个 `<ul>` 就是左右分组的导航栏，放进 `<aside>` 则变成竖排；`<button>`、提交按钮和 `<a role="button">` 都是按钮；输入框、下拉框、文本域是带焦点环和辅助文字的全宽字段；复选框、单选、滑块、文件和颜色选择器都换成与主题匹配的自定义控件；`<article>` 变成卡片，其直接子元素 `<header>` 和 `<footer>` 成为带底色的条带；`<details>/<summary>` 是带动画箭头的手风琴；`<dialog>` 是带模糊背景的模态框；`<progress>` 和 `<meter>` 也有样式。`<header>`、`<main>`、`<footer>` 作为 `<body>` 直接子元素时会自动居中。

**少量可选的 class。** 布局有 `.container`、`.narrow` 和自适应列数的 `.grid`，以及 `.cols-12` 配合 `.span-8` 这类固定栅格（48rem 以下自动变成单列），放在 grid 里的卡片会等高且页脚对齐；`.row`、`.spread`、`.stack` 处理弹性排列；按钮有 `.secondary`、`.contrast`、`.outline` 和大小变体。还有一些属性驱动的功能：`role="switch"` 把复选框变成开关，`aria-busy="true"` 显示加载转圈，`data-tooltip` 做提示框，`.dropdown` 把 `<details>` 变成下拉菜单，`.callout` 做提示信息框。

**排版和主题。** 标题字号是流式比例，从手机上的 1.2 倍逐渐增长到桌面上的 1.28 倍，基础字号从 16px 过渡到 17px，用户无需写媒体查询。`.prose` 提供约 68 个字符宽、带断字的长文阅读栏，`.serif` 切换到系统衬线字体。主题变量可以全局覆盖，也可以只写在单个元素的 style 上局部生效。文档提醒：暗色调色板在源码里出现两次（跟随系统和强制暗色各一份），改的话两处都要改。浏览器支持方面依赖 `:where()`、`:has()` 和 `color-mix()`，需要 Chrome/Edge 111、Firefox 121、Safari 16.2 及以上，更老的浏览器仍能显示可读的页面，只是悬停和焦点颜色会朴素些。仓库附带一个列出全部组件、能实时编辑主题变量的 docs.html，以及分步教程。

## HN 评论精华

本帖 97 分、20 多条评论，讨论比较轻松：一部分人借机推荐各自喜欢的无类 CSS 框架，尤其是 PicoCSS 停止维护后的替代品；另一部分在质疑这类框架在 AI 时代的意义，以及项目是否由 AI 生成。

**PicoCSS 退场后的接班人。** xd1936 说很高兴看到 Sumi，因为他心爱的 PicoCSS 已经宣布停止维护，cincaalex 也觉得 Sumi 出现的时机正好。culi 推荐「完全兼容 Pico」的 Blades CSS，结果招来不少批评：skrebbel 说那个站完全没打算向不了解 Pico 的人介绍自己；xd1936 认为网站像是拼凑出来的，好几个新组件只是 class，违背了 Pico 的初衷，而且 Pico 原作者明确要求分叉不要沿用 Pico 的名字，他们却叫「anyblades/pico」，还有一个框架对比页面「完全是胡扯」；ricardobeat 抱怨手机上导航体验很差，berofeev 怀疑他们是不是在用假数据喂语言模型。其他人推荐了自己或喜欢的方案：el_io 推荐 sakura，Leftium 介绍自己做的 Nimble.css，说它修复了 Pico 的许多恼人之处；codazoda 分享自己做的 Neat CSS，不追求完全无类，但只有 3KB，已经用在很多项目里；amanzi 提到刚发布 v1 的 Oat，madebysnacks 推荐「No Class」。trescenzi 和 dsego 分别贴出收集即插即用样式表和无类 CSS 主题的列表。

**无类框架的用途。** elicash 讲了一个实际案例：他曾让一位不懂技术的同事每周多次修改一个 HTML 文件再上传到 S3，用无类框架让对方几乎不可能出错，效果很好；他还说「限制本身也可以很有趣」。yokuze 已经发现一个小 bug 并提了 issue。

**用 `<a role="button">` 对吗？** CSS 性能专家 csswizardry 针对 README 示例里的 `<a href="#start" role="button">` 提出批评：这是有目的地址的链接，伪装成按钮会带来很多错误的交互暗示，请直接用链接。oliwarner 反驳说很多界面模拟表单流程，如果只是页面内的位置跳转而非状态变化，用按钮样式做导航没问题，「现在已经不是 1996 年了」。

**AI 时代还需要 CSS 框架吗？** delbronski 诚恳地问：既然这类小框架的意义是快速得到一致美观的界面，现在为什么不直接用 AI？SimonLarsen 给了两个理由：信不信由你，还有人不用 AI；AI 生成的网站长得都差不多，从一个干净的起点出发再按自己口味调整，能避免明显的「AI 味」。jszymborski 打比方说，有时候不想搬出立式搅拌机，就用手搅；能用螺丝刀就不必上电钻。

**对 AI 痕迹的反感。** precompute 说它用了「LLM 最爱的左边框装饰」，直接跳过。efilife 质问一个小小的 CSS 文件也需要 vibe coding 吗，「乐趣何在？」。
