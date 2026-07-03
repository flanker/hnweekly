---
layout: article
title: "shot-scraper video：用 YAML 定义并自动录制网页应用演示视频"
issue: 800
number: 20
category: show_hn
original_url: "https://simonwillison.net/2026/Jun/30/shot-scraper-video/"
hn_url: "https://news.ycombinator.com/item?id=48735625"
date: 2026-07-03
---

## 文章摘要

`shot-scraper video` 是 Simon Willison 在 shot-scraper 1.10 版本中新增的命令，它接受一个 YAML 配置文件来自动化浏览器交互，并借助 Playwright 录制整个过程的视频。开发者可以在一个"故事板"文件里定义一连串动作（点击、输入文本、暂停、导航），工具便会自动执行这些步骤并录像，从而生成软件功能实际运行的真实演示。

YAML 配置文件可指定：启动服务器的命令、初始 URL 与视口尺寸、用于测试环境的 JavaScript 初始化，以及一系列"场景"（scenes）。每个场景包含细粒度的操作——为视觉清晰而设的暂停、元素点击、表单填写、条件等待等。工具把这一切录制进一个视频文件，产出适合用于文档或发布公告的精良演示。

作者特别指出，这种模式非常适合编码智能体（coding agents）使用：详尽的帮助文档本身就充当了嵌入式的操作说明，AI 系统可以据此自行理解并使用该功能——只需给出类似"用新的 video 命令录一段演示"这样的提示，智能体就能独立完成录制。

## HN 评论精华

这条帖子的讨论较为简短，但评论者都对这种"以配置代替手动操作"的思路表示欣赏。

- **awllau** 称赞这是"跳出框框的好思路"，表示会去试用，并联想到当下 LLM 已具备计算机操控能力：把它与开源视频编辑器结合，"基本上就有了自己自举出来的 Screen Studio（一款热门录屏工具）版本"。

- **flamendless8** 从开发者视角表示认同："这很酷！作为开发者，我觉得写配置、让工具替我完成录制，比手动录制更有吸引力、更省心。"
