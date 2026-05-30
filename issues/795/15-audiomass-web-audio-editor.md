---
layout: article
title: "Show HN：Audiomass——浏览器里的免费开源多轨音频编辑器"
issue: 795
number: 15
category: show_hn
original_url: "https://audiomass.co/?multitrack=1"
hn_url: "https://news.ycombinator.com/item?id=48258015"
date: 2026-05-29
---

## 文章摘要

**Audiomass** 是一款由 **pkalogiros** 开发的开源、纯浏览器音频编辑器，完全在浏览器内完成波形编辑，底层基于 **Web Audio API**。这次 Show HN 的亮点是新增的**多轨（multitrack）**功能：用户可以叠加多条音轨、移动片段、对重叠部分应用交叉淡化（crossfade）、直接录音到指定通道，并最终合并导出为单个文件。

项目代码以 JavaScript 为主（约 92.4%），辅以少量 CSS 与 HTML。它可以在本地运行——通过自带的 Go 服务器（`audiomass-server.go`）或一个 Python 简易服务器启动，访问 `http://localhost:5055/` 即可使用；线上版本则在 audiomass.co。

仓库在 GitHub 上已收获约 **2.7k stars** 和 **299 forks**，附有 LICENSE 与第三方依赖声明（THIRD_PARTY_NOTICES）。评论区有人形容它的气质介于 Audacity 与老牌 Cool Edit Pro 2.0 之间——既有 Audacity 的影子，又带着一种「沉静与扎实」的设计感。

## HN 评论精华

- **serious_angel**：感觉灵感来自 Audacity，但在设计与质感上做了大改，给人「沉静与扎实」之感。试着导入 XM 格式时被提示「不支持」，询问是否有可能支持该格式，并附上一首在 VLC 里「光彩照人」的 .xm 作品链接，向作者的努力致谢。

- **JKCalhoun**：许愿一个云端版本——能「签出（check out）」别人的鼓点循环、加上吉他 riff、提交到一个分支，再让另一个人签出鼓 + 吉他、加上贝斯线后提交。认为和别人「jam」是最有趣的事之一，半开玩笑地提议叫「RiffHub」。

- **kirbysayshi**：看了代码后会心一笑——「看到安全闭包、函数赋值、顺序声明的 var……啊，我懂了，你是走『旧道』、掌握『失传知识』的人。」对这种开发风格很怀旧，虽然在团队场景里并不想念它，但称赞这是个超酷的应用。

- **sam1r**：为「开箱即支持 .flac 文件」点了大大的赞，「了不起的工作」。

- **genericacct**：希望能支持导入由 Suno、stemsplitter 等工具产出的 stem 分轨包（stem bundles）。

- **ugh123**：询问是否支持 MIDI 和 VST，自知「要求有点多」。

- **sgallant**：很赞，下周正好要做音频工作，本来还在发愁要用 Audacity。

- **HuzaifaYasin**：提问如何添加更多音轨，是否有数量上限。

- **yesbut**：从工程角度发问——理论上的文件/工程大小上限是多少？浏览器崩溃时会怎样？

- **algoth1**：一句怀旧——「有 2002 年 Cool Edit Pro 2.0 的味道！」

- **ZeWaka**：提了个细节建议——不太理解为何要移除被禁用按钮上的 tooltip 提示。
