---
layout: article
title: "牧羊犬：一款由 Fable 一次生成的游戏"
issue: 798
number: 60
category: fun
original_url: "https://koenvangilst.nl/lab/claude-fable-shepherds-dog"
hn_url: "https://news.ycombinator.com/item?id=48513728"
date: 2026-06-19
---

## 文章摘要

这篇文章记录了作者用 Anthropic 的模型 Fable 一次性生成出一款名为《牧羊犬》（Shepherd's Dog）游戏的经历。作者半开玩笑地称 Fable 是"那个据说危险到不能公开给世界看"的模型，借此调侃它的强大能力。

最让作者惊讶的是生成过程本身：模型进行了一次长达 45 分钟、消耗超过 20 欧元 API token 的超长推理，最终一次性产出了一个 2319 行、零外部依赖的单个 index.html 文件。整个游戏被打包进这一个 HTML 文件中，打开即玩。

作者表示，这是 AI 第一次一次成型地实现出他心中所设想的游戏概念——"完全就是我想象中的样子"。在此之前他曾用更早的模型多次尝试都未能成功，相关失败记录都保存在他的 GitHub 仓库里。文章既是一次有趣的小游戏分享，也是对当前 AI 编程能力进步的一个直观注脚。

## HN 评论精华

- **sixhobbits**：玩得很开心，并贴出了一个可绕开 GitHub 直接进入游戏的链接。
- **jna_sh**：指出游戏操作顺滑、完成度高，说明模型对实现细节理解得很深，尽管概念本身算不上革命性。
- **redrobein**：认为相比其他 AI 作品，这款在动画流畅度和难度递进的清晰度上都更出色。
- **neonstatic**：质疑作者的创意是否真的原创，毕竟类似的游戏"数不胜数"。
- **Ouman / nmstoker**：认为关键不在于"原料"是否原创，而在于"组合（synthesis）做得好不好"，以及能否在现代浏览器上良好运行。
- **raincole**：认为这篇文章的价值在于展示 AI 的进步，而非游戏本身的新颖性。
