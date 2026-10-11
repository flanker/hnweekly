---
layout: article
title: "Pyxel：自带美术与音效编辑器的 Python 复古游戏引擎"
issue: 813
number: 22
category: code
original_url: "https://github.com/kitao/pyxel"
hn_url: "https://news.ycombinator.com/item?id=49928215"
date: 2026-10-09
---

## 文章摘要

> 说明：本帖链接的 GitHub 仓库在写作时无法访问——作者在评论中解释，他的 GitHub 账号因令牌被盗用而遭到封禁，正在等待恢复。以下内容依据 PyPI 上 Pyxel 2.9.9 的项目说明、安装包内附带的 README、API 类型声明和示例代码，以及作者在 HN 帖子中的自述整理。

Pyxel 是日本开发者北尾隆（kitao）自 2018 年起独立开发的 Python 复古游戏引擎，采用 MIT 许可证，免费使用，在 GitHub 上已获得超过 1.8 万颗星。它的设计灵感来自老式游戏机的硬件限制：屏幕只能显示 16 种颜色，声音最多 4 个通道同时播放。作者的思路是，这些刻意的约束能让人轻松地做出像素风游戏，而不必在工具选型和底层细节上纠结。

**主要特点：**

- **Python 写逻辑，Rust 做引擎**：用户用 Python 编写游戏，引擎核心则由 Rust 实现。
- **内置创作工具**：自带像素画编辑器、瓦片地图编辑器、音效编辑器和音乐编辑器，图像、地图和声音都存放在资源文件中，因此一个人就能在同一套工具里完成美术、音频和代码。
- **简单的 API**：一个最小程序只需 `pyxel.init(160, 120)` 设定分辨率（默认 30 帧），再用 `pyxel.run(update, draw)` 注册更新和绘制两个函数。绘图 API 包括 `cls`、`pset`、`line`、`rect`、`circ`、`tri`、`blt`（贴图）、`bltm`（贴地图）、`text` 等；新版本还加入了 `blt3d`、`bltm3d` 这类伪 3D 透视绘制。输入有 `btn`/`btnp`/`btnr`，音频有 `play`、`playm`，甚至可以用 `gen_bgm` 生成背景音乐；另附 `rndi`、`noise` 等随机数和 Perlin 噪声工具。
- **完整的命令行工作流**：`pyxel run` 运行脚本，`pyxel watch` 监视目录自动重载，`pyxel edit` 打开资源编辑器，`pyxel package` 打包成 `.pyxapp`，还能把应用转换成可执行文件或单个 HTML 文件。
- **跨平台**：支持 Windows、macOS、Linux 和 Web，也能在部分基于 Linux 的复古掌机上运行。托管在 GitHub 上的 Pyxel 游戏可以通过一个 URL 直接在浏览器里启动。
- **浏览器内创作**：Pyxel Code Maker 可以完全在浏览器中制作游戏，Pyxel MML Studio 则用 MML 语言写芯片音乐。
- **丰富的示例**：安装包里附带约二十个从「Hello Pyxel」到平台跳跃、射击、贪吃蛇、合成器、Tiled 地图导入、透视效果的教学示例，以及《Megaball》《30 Seconds of Daylight》等完整小游戏；作者还维护了一个用户作品展示页。

作者在帖子中表示，欢迎大家提问关于引擎本身及其实现的问题。

## HN 评论精华

这个 Show HN 只有 11 条评论，讨论不多，但作者亲自回答了几个关键问题。

- **krapp** 问了两个问题：你们的 AI 政策是什么？和 PyGame 有什么区别？作者 **kitao** 回答：关于 AI，他有两个目标——一方面希望 Pyxel 简单的 API 能让人不借助 AI、亲手编程也觉得有趣；另一方面，简洁专注的设计也让它很适合 AI 辅助开发乃至 vibe coding，为此他发布了 Pyxel MCP 和 Pyxel Skill。至于 PyGame，后者更通用，而 Pyxel 刻意收窄了范围和规格，并自带像素画、地图、音效和音乐编辑器，重点是让初学者不必做大量工具和底层决策就能开始做游戏；这些复古限制同样吸引有经验的开发者。
- **jhbadger** 补充说，凭借可交互编辑精灵和声音的编辑器，Pyxel 更像 PICO-8、TIC-80 那样的「幻想游戏机」。
- **gregkb** 说用 Pyxel 重现了童年时代的演示场景（demoscene）特效，玩得很开心：安装简单、上手很快，颜色和位图组织方式上的固有限制恰好带来令人满足的挑战，正好满足对 8/16 位汇编的怀旧。
- **LarsDu88** 说自己一直很喜欢 Pyxel，但如今 AI 已经能一次生成多人 3D 射击游戏，自己用它创作的动力减弱了；他问能否直接用 Rust 写游戏。作者回答 Rust 模块确实可以用，但并不是为 Rust 用户设计的游戏开发 API，不建议把 Pyxel 当作学习 Rust 的起点。
- **dejobaan** 是从 6502 时代过来的人，表示一直很喜欢这类复古项目，也欣赏大家用它做出的各种作品。**davidedicillo** 则呼吁「让普通应用也做成 80 年代游戏的样子」，打算用 Pyxel 驱动自己接下来的项目。
- **az09mugen** 想试用时发现 GitHub 链接变成了 404，问作者是不是删了仓库。作者解释：仓库没有删除，而是他的 GitHub 账号在令牌被未授权使用后遭到封禁；他在封禁前已吊销令牌并删除了恶意工作流，正在联系 GitHub 等待恢复。
