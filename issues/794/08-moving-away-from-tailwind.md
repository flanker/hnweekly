---
layout: article
title: "Julia Evans 离开 Tailwind：重新学会用结构化的方式写 CSS"
issue: 794
number: 8
category: favorites
original_url: "https://jvns.ca/blog/2026/05/15/moving-away-from-tailwind--and-learning-to-structure-my-css-/"
hn_url: "https://news.ycombinator.com/item?id=48158400"
date: 2026-05-22
---

## 文章摘要

Julia Evans（jvns）把她名下几个网站从 Tailwind 迁回**手写的、有组织的 vanilla CSS**。八年前她离不开 Tailwind，是因为自己根本不会管理大量样式；八年过去，她意识到自己的 CSS 功力已经足以撑起项目，不需要再借助一个"工具类即设计系统"的框架。

她列出的离开理由很务实：Tailwind 越来越依赖完整的构建流水线，未压缩的 CSS 文件膨胀到了 **2.8MB**；做一些非常规设计时框架反而碍手碍脚；为了改一些边角又得在 Tailwind 和 vanilla CSS 之间来回切换，代码库变成混血儿。她还引用另一位作者的观点——**"Tailwind 在贬值 CSS 这门手艺"**——这一点她深有同感，与其租用别人的脚手架，不如真正把 CSS 学透。

文章的核心价值不在"离开"，而在她**系统地说清自己留下了什么**。她总结出九个组织维度：（1）reset 样式直接借 Tailwind 的 preflight；（2）按组件单独拆文件，避免互相污染；（3）颜色全部走 CSS 变量；（4）字号体系定义成 `xs / sm / lg` 等档位，**每档配套行高（line-height）**；（5）保留少量 utility class（如 `.sr-only`）；（6）尽量克制的 base styles；（7）用 `section > *+*` 这类选择器统一处理间距；（8）响应式优先用 CSS Grid 的 `auto-fit`，能不写 media query 就不写；（9）打包用 esbuild。

她想表达的核心是：**Tailwind 的"系统"那套思维（design tokens、间距阶、字号阶）非常值得保留**，但你完全可以把它装回到原生 CSS 里。框架只是其中一种实现方式。

## HN 评论精华

- **TonyAlicea10**（无障碍方向的老兵）：批评 Tailwind 把开发顺序颠倒了——"HTML 是用来标记文档语义的，你应该先把 HTML 写对，再用 CSS 上样式。"Tailwind 鼓励你为了挂样式而塞 div，这是反 web 标准的。

- **alwillis**：从 DRY 视角开火——Tailwind 既违反 DRY 又同时把样式逻辑切两半（HTML 和 CSS 同时承担），认知负担加倍。他还指出 `@layer` 和 `@scope` 已经原生解决了当年 Tailwind 用来背书自己的"层叠失控"问题，没必要再用框架。

- **danaw**（写 CSS 20 年）：站 Tailwind——他承认自己一度抗拒，但现在大型应用里 Tailwind 让他不用再追"选择器太宽误伤了哪里"的玄学 bug，**团队一致性和打包优化是真利好**。

- **spiderfarmer**：写 vanilla CSS 二十年后皈依 Tailwind 派——他特别看重"省下读别人组织结构的时间"。这种"上手即懂"对大团队来说意义巨大。

- **关于无障碍（accessibility）的争论**：批评派认为 Tailwind 让代码变成 div 汤，新人学不到正确的语义 HTML；辩护派回怼"工具不背锅，纪律才背锅"。**embedding-shape** 提醒大家：无障碍要**在残障用户来之前就做好**，事后修补几乎不可能。

- **替代方案大集合**：ITCSS（按特异性分层）、CSS Modules（构建时作用域）、Cascade Layers（`@layer` 原生解决特异性）、Every Layout（充分利用层叠）、纯语义 HTML + 作用域 class——评论区把 2026 年的 CSS 工具箱列了个遍，得出结论：**今天的 CSS 已经不是 2017 年那个不得不靠 Tailwind 才能写大的 CSS 了**。

- **alwillis / TonyAlicea10 共识**：长期看 div 汤会让调试、迁移、新人入职都更痛。短期速度 vs 长期可维护性，是这场辩论真正的轴。

- **danaw 反驳的核心**：在 Fortune 100、100+ 组件设计系统里，Tailwind 的真正价值不是"少写 CSS"，而是**避免不同人写出来的 CSS 互相打架**。

- **小结**：jvns 的文章在 HN 上意外引发的不是"对 / 错"，而是**职业级 CSS 从业者** vs **应用产品工程师**两种工种诉求的清算——前者觉得 CSS 是要学的，后者觉得 CSS 是要避开的。
