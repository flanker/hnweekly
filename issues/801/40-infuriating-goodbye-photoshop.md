---
layout: article
title: "与 Photoshop 令人恼火的告别"
issue: 801
number: 40
category: design
original_url: "https://anderegg.ca/2026/07/12/an-infuriating-goodbye-to-photoshop"
hn_url: "https://news.ycombinator.com/item?id=48891200"
date: 2026-07-17
---

## 文章摘要

作者 Gavin Anderegg 在忠实使用 Photoshop 超过 30 年后，终于把它从电脑里卸载了。促成这一决定的，是对软件本身和 Adobe 商业行为积累已久的双重不满——而如今他的工作也不再需要 Photoshop 的那些高级功能。

在软件质量层面，他列举了一连串退化：更新失败到必须重装、每次启动后设置被重置、偏好设置需要反复手动重新配置、以及硬塞进来的欢迎屏幕。而更让他愤怒的是 Adobe 的一些不道德做法：Adobe 竟然偷偷修改了他系统的 `/etc/hosts` 文件用于许可证验证；用更慢的网页版控件替换了原生 UI 控件；还砍掉了他很看重的 Creative Cloud 云同步文件功能。

最"令人恼火"的部分在于订阅陷阱。作者被锁定在一个"年付按月扣款"的套餐里，无法轻易取消。当他试图退订时，取消界面竟然用白底白字显示，内容根本看不见；好不容易取消后，卸载过程又反复失败。整个体验被他形容为一场精心设计的、让你无法脱身的迷宫。

在替代品方面，文章提到了 Pixelmator Pro、Acorn、Affinity Photo 以及苹果的 Creator Studio（作者最终的选择之一）。他的结论是：尽管仍对早年那个优秀的 Photoshop 心怀怀念，但如今它已沦为维护糟糕的臃肿软件；转向 Pixelmator Pro 带来了"软件质量上的巨大提升"，以及不必再担心退订套路的内心安宁。这篇文章本质上是一封老用户的分手信，也是对订阅制软件生态整体劣化的控诉。

## HN 评论精华

- **GavinAnderegg**（作者本人）：在评论区确认了那个最劲爆的细节——Adobe 通过以 root 权限运行的特权辅助工具，在不弹出密码提示的情况下，静默修改了用户的 `/etc/hosts` 文件。
- **pauldoerwald 与 pier25**：就"Adobe 为何堕落"展开讨论。pauldoerwald 认为根源在于 Adobe 放弃产品质量、转而追逐季度财务指标，而非订阅制本身必然导致腐化；pier25 则总结道 CS6 是最后一个扎实的版本，一切都从 2013 年 Creative Cloud 上线后开始变差。
- **d3Xt3r 与 geenat**：讨论替代方案 Photopea，它提供了类 Photoshop 的界面和快捷键、启动飞快，但 geenat 指出它在处理大文件时会卡顿，Affinity 表现更好。
- **InsideOutSanta**：现身说法，从 Photoshop 转到 Acorn（做像素级工作）和 Affinity（做通用任务）后长期满意，称在用了 30 年 Adobe 后"完全不觉得少了什么"。
- **trembolram**：抛出隐私隐忧——Affinity Studio 会向 serifservices.com、canva.com、sentry.io 等多个服务器发送数据，不像早期的 Affinity Designer 1。**jkestner** 也对 Canva 收购 Affinity 后未来的变现方向表示警惕。
- **cmyk_student**（GIMP 贡献者）：为 GIMP 正名，反驳它"已停止开发"的指控，指出非破坏性编辑功能正在积极实现中，尽管用户对方向有分歧。
