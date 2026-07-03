---
layout: article
title: "在浏览器里生成矢量 PDF 的方格纸工具"
issue: 800
number: 18
category: show_hn
original_url: "https://freegraphpaper.net/"
hn_url: "https://news.ycombinator.com/item?id=48761294"
date: 2026-07-03
---

## 文章摘要

FreeGraphPaper.net 是一个免费的在线工具，用来生成并打印各种自定义方格纸——无需登录、无水印。它支持多种纸型：标准方格网（5mm、1cm、1/4 英寸、1/8 英寸间距）、点阵纸、30° 三角等距（isometric）纸、六边形网格，以及横线纸和康奈尔笔记纸。

工具完全在浏览器内运行。用户选择或自定义模板后，会看到一个实时更新、"真实比例"的预览，然后即可下载可直接打印的 PDF 或 PNG 文件。网站特别提醒要"以 100% 比例打印（关闭'适应页面'）"，以确保网格尺寸精确。它支持从美制（Letter、Legal、Tabloid、ANSI、Arch 系列）到 ISO 标准（A、B、C 系列）的大量纸张尺寸，间距、颜色、页边距均可自定义。

作者在 HN 上介绍了背后的技术设计：一个纯函数 `buildPaper(config)` 输出几何图元（线、圆、文字），屏幕上的 SVG 预览、浏览器内的 PDF、构建时生成的 PDF 与 OG 预览图全都从这一个函数派生而来。绘图逻辑不在各输出格式间重复，因此新增一种纸型只需一个渲染函数加约五行的一行式注册代码。他做这个的动机是受够了那些"打印比例错误、把 PDF 藏在注册墙后、或者打上水印"的方格纸网站。

## HN 评论精华

评论区以正面反馈和功能建议为主，气氛友好。

- 实用性获广泛认可：**rpdillon**、**smalltorch**、**setnone**、**analog8374** 等纷纷点赞其设计简洁、用途明确。**indianmouse** 认为它解决了其他网站的诸多痛点，对需要方格纸的学生很有用，并询问是否会开源。**relium** 感慨自己 1985 年在第一台 LaserWriter 上写过 PostScript 方格纸生成器，勾起怀旧。

- 绘图管线（plotter）用法：**genewitch** 询问能否导出 SVG 以便载入绘图仪制作方格纸，并反馈说可以先把 PDF 导入 Inkscape 再转 SVG（不过 Cricut 的切图软件会缩放导致精度损失，他判断问题出在 Cricut 而非本站或 Inkscape）。

- 技术讨论：**Theodores** 建议在 SVG 中贯彻 DRY 原则——画一条主线放进 `defs` 里当 symbol，其余线条用 `use` 克隆并只指定 x 或 y 偏移，从而只需写一次 stroke-width 等属性。

- 功能请求：多位用户希望增加更多科学/工程图纸类型——**fortran77** 想要更多对数（log）选项、log-linear 和史密斯圆图（Smith chart）；**riponcm** 也提到 log-log 图；**Topology1** 问能否加五边形图案，**jagged-chisel** 指出正五边形无法平铺、可考虑其他五边形密铺，**bakul** 则建议加 Penrose 密铺。

- 反馈的问题：**ChrisMarshallNY** 报告在 Mac 版 Safari 上标准方格看不见（原因是线条太淡，缩放后可见）；**jagged-chisel** 发现"在方格纸上绘制"功能有 bug——切到六边形网格并勾选"吸附到网格"后，仍吸附到原来的方形网格。**timpark** 也建议把预览线条加深加粗，方便辨认十字绣、编织等不熟悉的图案。**asibahi** 询问能否做成可下载的桌面应用。
