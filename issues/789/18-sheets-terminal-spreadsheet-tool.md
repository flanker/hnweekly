---
layout: article
title: "Sheets：基于终端的电子表格工具"
issue: 789
number: 18
category: show_hn
original_url: "https://github.com/maaslalani/sheets"
hn_url: "https://news.ycombinator.com/item?id=47636456"
date: 2026-04-10
---

## 文章摘要

Sheets 是一个由 maaslalani 开发的终端电子表格应用程序，使用 Go 语言编写，以 MIT 许可证开源。该项目在 GitHub 上获得了 1.8k 星标和 55 个 fork，展示了开发者社区对终端工具的持续热情。

Sheets 的核心特点是将类 Vim 的操作体验带入电子表格领域。用户可以通过 `sheets budget.csv` 命令启动交互式电子表格编辑器，在终端中直接操作 CSV 文件。导航采用 Vim 风格的键绑定：h/j/k/l 键移动单元格，`gg` 跳转到顶部，`G` 跳转到底部，`gB9` 跳转到特定单元格。搜索功能支持正向搜索（/）和反向搜索（?），还支持标记功能（`ma` 设置标记，`'a` 返回标记位置）。

编辑功能同样借鉴了 Vim 的模式设计：按 `i` 进入编辑模式，`I` 从单元格开头编辑，`c` 清空并编辑。行操作包括用 `o`/`O` 插入行、`dd` 删除行。支持 `y`（复制）、`x`（剪切）、`p`（粘贴）操作，以及 `u`（撤销）和 `ctrl+r`（重做）。`v` 进入可视选择模式，`V` 选择整行。命令模式通过 `:` 访问，支持 `:w` 保存、`:e path.csv` 打开不同文件、`:goto B9` 跳转到特定单元格、`:q` 或 `:wq` 退出等操作。

该工具还支持命令行直接查询，无需进入交互模式即可获取特定单元格或范围的值。数据输入非常灵活，支持从文件加载、通过标准输入管道传输数据，或直接创建新表格。安装方式包括 Homebrew（`brew install sheets`）和 Go（`go install github.com/maaslalani/sheets@main`）。

该项目在 Hacker News 上引发了一场关于终端电子表格工具和电子表格历史的丰富讨论。评论者们纷纷提到了这个领域的先驱——从 1970 年代的 VisiCalc 到 Lotus 1-2-3、Quattro Pro 等早期电子表格程序，这些最初都是在终端环境中运行的。讨论还涉及了多个相关项目，包括 sc-im（一个更成熟的终端电子表格）、VisiData（一个功能强大的终端数据分析工具，支持跨格式的即席数据分析和连接）、以及 GNU Oleo 的现代化分支 Neoleo。有趣的是，甚至有人分享了在现代 Linux 上运行 Lotus 1-2-3 Unix 版的方法。

## HN 评论精华

1. **gigatexal**："Vim 键绑定？我来了！"这条简洁的评论获得了广泛共鸣，有人推荐了 Neovim 的 csvview.nvim 插件作为替代方案。这反映了 Vim 用户群体对在各种应用中保持一致操作体验的强烈偏好。

2. **blippage**：作为 GNU Oleo 的维护者和 Neoleo 的作者进行了深入的技术分享。他介绍了 VisiData（一个用 Python 编写的快速数据工具）、GNU Oleo（一个可追溯到"很久很久以前"的电子表格）以及他自己的 Neoleo 项目。Neoleo 约 5000 行 C++ 代码（相比 Oleo 的 50000 行 C 代码），没有已知内存泄漏，使用手写解析器替代了 lex/yacc，并提供了 Tcl 绑定来扩展功能。后续评论中 Gormo 特别强调 VisiData 远不止数据录入，它基本上是电子表格和关系数据库的结合体，可以打开 Postgres 表、本地 CSV 和来自 REST API 的实时 JSON 数据，在单个终端会话中进行复杂连接、过滤和标准化。

3. **lateforwork**："所有电子表格以前都是在终端里运行的。"他还分享了一个可以下载 Quattro Pro 4.x 的链接，引发了一波怀旧讨论。有人分享了使用 Lotus 1-2-3 的童年回忆，另一位则在 HP-85 上使用过 VisiCalc（大约 1980 年），还有人分享了在现代 Linux 上运行 Lotus 1-2-3 Unix 版的 123elf 项目。

4. **halosghost**：推荐了 sc-im（一个更成熟的终端电子表格工具）和 Teapot（一个更具创新性的电子表格）。后续讨论中 somat 对电子表格领域几乎没有创新空间感到遗憾——"如果不是 Dan Bricklin 设想的那种电子表格方式，人们就不想要它"。他还提到了 Lotus Improv，一个在 NeXTSTEP 上运行的革命性电子表格，并打算在 NeXT 模拟器上亲自体验。

5. **jnpnj**："很酷，大多数时候我不想为了一个快速表格就启动 Emacs org-table 或 Google Sheets。"项目作者 maaslalani 也在评论中积极互动，对此表示认同。另一位评论者提到了 Quattro 那个年代的软件视频，对其在极小的空间/CPU 下实现的功能之丰富感到由衷惊叹。
