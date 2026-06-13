---
layout: article
title: "Claude Fable 极度主动"
issue: 797
number: 8
category: favorites
original_url: "https://simonwillison.net/2026/Jun/11/fable-is-relentlessly-proactive/"
hn_url: "https://news.ycombinator.com/item?id=48498573"
date: 2026-06-12
---

## 文章摘要

Simon Willison 描述 Claude Fable 5 异乎寻常地"极度主动"——它会在没有明确指令的情况下，主动动用各种复杂手段去达成调试目标。

事情起因是他在 Datasette Agent 的界面里发现了一个水平滚动条的 bug。他只是把截图发给模型，附上一句极简的提示让它"看看依赖项"，随后便离开了座位。等他回来时，发现 Claude 已经自主打开了浏览器窗口并展开了大量测试。

Fable 展现出的技术手段令人惊讶：

- **自定义截图自动化**：用 PyObjC 枚举 macOS 窗口、按名称匹配定位 Safari，再通过命令行工具截图。
- **模板注入**：往 Datasette 模板里注入 JavaScript，在页面加载时自动触发键盘快捷键。
- **自建 CORS 服务器**：用 Python 标准库搭了一个 HTTP 服务器，通过 fetch 请求从浏览器页面收集 JSON 诊断信息。
- **Shadow DOM 遍历**：脚本化地穿透 Web Component 内部，测量 textarea 的属性。

这次会话消耗了约 12 美元的 token——对于一个两行的 CSS 修复而言投入相当可观，恰恰说明 Fable 在求解时有多么激进。

Willison 的核心结论是：这种"极度主动"在调试中确实有效，但也带来严重的安全隐忧——若没有沙箱隔离，如此强大的智能体一旦被恶意指令或提示注入攻击劫持，可能造成重大破坏。

## HN 评论精华

**bananaquant**（741 赞）认为这个修复治标不治本：质疑为什么 textarea 会溢出，以及为何这段 CSS 变通方案出现在两处，而不是把元素一次性样式化好。

**biztos** 指出把静态 CSS 嵌进 `__init__.py` 是糟糕的架构实践，真正的问题在于结构组织，而非那个 CSS bug 本身。

**simonw**（作者本人回复）罗列了从智能体行为中学到的具体技术点：screencapture 命令行用法、通过 Quartz 获取窗口 ID、KeyboardEvent 派发、Web Component 的 shadow DOM 遍历、Chrome 滚动条配置等，并辩护说"只要留意它在做什么，你能学到太多了！"

**peterbell_nyc** 把讨论重新框定为"如何训练/指挥智能体"而非"精通 CSS"，认为目标是学会高效地指挥智能体，这更像在管理一支开发团队。

**danudey** 对 Claude Fable 在何处撞上安全护栏并降级回退到 Opus 表示着迷，指出该智能体在被拦下之前，已远远越过了通常的安全边界。

**dekhn** 观察到前沿模型会自然地把训练语料中的各种单项技术组合起来，并指出"用 UI 作为 AI 系统的 API"代表了一种向人类界面收敛、而非依赖专用 API 的趋势。
