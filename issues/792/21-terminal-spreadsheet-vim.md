---
layout: article
title: "Show HN：cell —— 带 Vim 键位的终端电子表格编辑器"
issue: 792
number: 21
category: show_hn
original_url: "https://github.com/garritfra/cell"
hn_url: "https://news.ycombinator.com/item?id=47920289"
date: 2026-05-01
---

## 文章摘要

cell 是 Garrit Franke 用 Rust 写的终端电子表格编辑器，最大的卖点是把 Vim 的肌肉记忆原封不动地搬到了二维表格上。作者在 Show HN 里说，他在写需求文档的时候根本不需要去"设计"键位——`i` 进入插入、`Esc` 回到 normal、`hjkl` 移动、`dd/yy/p` 剪切复制粘贴、`:w/:q` 保存退出、visual 模式按字符/行/块选中，全部直接生效。配合标准的 jump list、marks、撤销/重做以及 headless CLI 模式，这是一款专为 Vim/Neovim 用户准备的电子表格。

公式引擎实现了 Excel 风格的子集：`=SUM`、`=AVERAGE`、`=COUNT`、`=MIN`、`=MAX`、`=IF`，支持 A1、AA10、A1:B3 形式的单元格引用与区间。文件层面同时支持 CSV/TSV、自定义分隔符以及原生 `.cell` 格式——后者保留公式且仍是人类可读的纯文本，这意味着你既可以用 cell 编辑日常数据，也可以把它当成 git 友好的"轻量 Excel"。代码组织成 Cargo workspace：纯逻辑层 `cell-sheet-core`（不依赖 TUI）和基于 ratatui 的 UI 层 `cell-sheet-tui`，便于今后被其他前端复用。当前最新版本是 0.4.0（2026 年 4 月发布），Linux/macOS/Windows 均提供预编译二进制。

发布之后，作者在评论里还顺手把社区提的 headless 脚本接口（`cell file.cell --read/--write`）加进了 v0.1.7，反应速度非常快。

## HN 评论精华

- **WillAdams** 提了一个非常有价值的产品方向建议：希望 cell 不要只复刻 A1/B2 这种坐标系，而是借鉴 Lotus Improv、Javelin、Quantrix、Flexisheet 那一套"维度优先"的设计——把列和行作为命名分类，公式作用在维度而非坐标上。这是上世纪被遗忘的一支电子表格流派，对建模复杂业务非常强。作者承认没考虑过，已经开了 issue #11 跟踪。后续讨论里 WillAdams 也务实地承认 UI 是真正的难点，建议至少先支持给行列命名，然后在公式里用名字引用。

- **marcyb5st** 提议加一个"程序化访问"模式，比如 `cell file.cell --write A2 "42"` 或 `--read "=SUM(A1:A10)"`，方便用 cell 给 shell 脚本当电子表格后端。作者立刻开了 issue，并在 v0.1.7 里实现并合并，体现了独立项目在早期能给用户带来的高反馈密度。

- **slu** 把 cell 与已有的 sc-im（终端电子表格的事实标准之一）对比。作者回复说 cell 本身就深受 sc-im 启发，并在 README 里加了一张对比表，强调 cell 走的是更"Neovim 化"的路径。

- **ximm** 是另一位曾经做过 spreadsheet TUI 的开发者，他指出 Excel 中拖动填充（drag-fill）配合 `$A2 + B$1` 这种相对/绝对引用是日常高频功能。作者回复 cell 内部 AST 已经携带 abs_col/abs_row 信息，所以 `$A2 + B$1` 已经能正确解析；缺的是一个把公式按 range 复制并自动平移相对引用的"操作"。难点不在重写公式，而在 Vim 没有"拖动"这个动词，正在考虑用 visual 选区 + fill-from-anchor，候选键位是 `Y`。**teo_zero** 进一步提议用 `I`/`A`，类比 Vim 在 visual block 上的"多行插入"语义，这是一个非常本地化的设计建议。

- **Theaetetus** 表示终于找到一个"低延迟、感觉很轻"的电子表格选项，"用着让我感到平静"，这是 TUI 工具能拿到的最高赞美之一。

- **rpassos** 补充了一个互补的工作流：他现在用 medialab/xan 处理 CSV 阅读和分析，但写起来不行；CSV 在终端的列宽不齐让人很难手写，cell 这种"CSV-aware 的 Vim"正好能补上。作者也呼应说 xan 和 cell 互相 pipe 是合理的搭配。

- **bewuethr** 顺带推荐了 csvkit 的 `csvlook` 和 mikefarah/yq 的 CSV 输入支持，作为对比和补位工具。

整体上社区对 cell 的反馈非常正面，主要建议集中在三方面：维度化建模、拖动填充、可编程接口。作者响应迅速，把 cell 推向了"Vim 用户的实用 Excel"这一相对空白的细分市场。
