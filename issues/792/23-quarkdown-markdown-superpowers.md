---
layout: article
title: "Quarkdown —— 拥有超能力的 Markdown"
issue: 792
number: 23
category: code
original_url: "https://quarkdown.com/"
hn_url: "https://news.ycombinator.com/item?id=47919240"
date: 2026-05-01
---

## 文章摘要

Quarkdown 由 Giorgio Garofalo（iamgio）发起，定位是"Markdown 与 LaTeX 的现代结合体"——保留 Markdown 写作时的轻量感，却补上了 LaTeX/Typst 那种可编程、可严格排版的能力。它的卖点不是又造一种纯文本格式，而是把 Markdown 当作一个**可扩展的脚本宿主**：通过自定义函数与扩展，让作者把重复的版式逻辑抽象成"调用"，而不是手抄一遍 HTML 或 LaTeX。

从产出上看，Quarkdown 用同一个源码可以生成四类文档：

- **Paged**：科研论文、书籍、报告，瞄准的是 LaTeX/Typst 的位置；
- **Plain**：笔记和知识库，想取代 Notion/Obsidian；
- **Docs**：技术文档/wiki，对标 GitBook、Docusaurus、MkDocs；
- **Slides**：演示稿，对标 Beamer 与 Google Slides。

也就是说 Quarkdown 想做的是"一个工具产出五种结果"。它内置数学公式、表格、`.row/.column/.grid` 这类版式函数，能搭出多栏、卡片、网格等结构；同时强调"react 式实时预览"——编译速度足够快、保存即可见。开源协议永久免费，目前 GitHub 已经超过一万 star。整个项目还在持续吸纳社区 PR，比如把文件后缀从 `.qmd` 改为 `.qd` 就是去年因为和 Quarto 撞名而走的一次社区决议。

技术读者能从中看到一个明显的趋势：Markdown 阵营正在向"可编程文档语言"演化。Typst、Quarkdown、MyST、Quarto、MDX 都是同一坐标轴上的不同点位，分别在"是不是 Markdown 超集""是否引入新语法""是否能执行代码块""目标输出格式"等维度上做出取舍。

## HN 评论精华

- **amai** 提议官方 README 应该补一张大型对比表，覆盖 MyST、Pandoc、Quarkdown、Quarto、Typst。讨论里 ahofmann 指出 Quarkdown 已经有 [comparison 表](https://github.com/iamgio/quarkdown#comparison)，但 flexagoon 立刻反驳说表里没列 Quarto——而 Quarto 恰恰是最接近 Quarkdown 的对手。作者 iamgio 礼貌邀请 PR，但 t-kalinowski 翻出一个有意思的事实：去年 6 月作者本人就因为 Quarto 把扩展名从 `.qmd` 改成了 `.qd`，所以"不熟 Quarto"这个说法略微站不住脚。

- **revolvingthrow** 给出了一份很受欢迎的"使用情境速记"：Markdown 是"带一点点语法糖的 .txt"；Quarto 是"想在 Markdown 里跑代码块"；Typst 是"现代化的 LaTeX，少 90% 冗余、少 10% 功能（学术界讨厌一切现代东西，所以也会讨厌你用 Typst）"；Pandoc 是"导出器"。这种总结在评论里被广泛点赞。

- **netbioserror** 直接表态用 Typst 做过书、册子、卡片、文档"全套"，"在大多数场景顶部加一点 style 就够了，远胜其余工具"，是 Typst 阵营的强力站台。**noelwelsh** 则从技术角度质疑 Quarkdown 的求值模型："文本排版通常需要迭代到不动点"，因为局部布局变动可能撼动其他部分；Typst 用 `context` 解决了这一点，他没在 Quarkdown 里看到对应机制，已为自己的书从 pandoc/MD/LaTeX 切到 Typst。

- **CJefferson 与 leephillips/xigoi** 之间关于 Typst 可访问性的争论尤其值得关注：CJefferson 担心 Typst HTML 输出对盲人用户不友好（"我们好不容易让 LaTeX→HTML 在 arXiv 上能用，不会再切换到一个更糟的"），xigoi 反问凭什么 Typst 输出更差。CJefferson 的具体证据是几周前测试过 Typst 的 HTML 输出，表格渲染依然不正常，且官方路线图并非追求"全覆盖"。这条线提醒作者们：可访问性不是后期能补上的功能，而是产出格式的设计前提。

- **kitchi** 解释为什么 Typst 在学术圈推进缓慢："出版社模板大多基于 LaTeX，导师又不愿意为新工具花脑力，LaTeX 有 40 年沉淀。"**dgacmu** 在课堂小测验里试用 Typst，"显著减少了样板代码"，是个温和的转向例子。**nextaccountic** 提议要的不是"用 Typst 跑 day-to-day"，而是一个 Typst→LaTeX 的 transpiler，"出版社不必知道你私下用的什么"，类似 jj 之于 git。lametti 给出了已有方案 [TheZoq2/ttt](https://codeberg.org/TheZoq2/ttt)，他用它真的投了一篇期刊。

- **runningmike、nzoschke、smartmic、dleeftink** 各自补刀建议把 MyST、djot、TeXmacs、paged.js 也加入对比；leephillips 反驳 TeXmacs 路径已死，因为大家真正需要的是 LaTeX 生态。**hirako2000** 提醒 Markdown 的精神是"轻"，超出 Markdown 的部分可能恰恰违反了它的初衷；**sputknick/threetonesun** 则联想到 [xkcd 927](https://xkcd.com/927/)（每出一个新标准，标准就更多了）——LLM 时代 Markdown 几乎成了 lingua franca，但碎片化也在加剧。

- **Aeroi** 关心 Quarkdown 在处理 LLM 流式输出 Markdown 时的体验。作者 iamgio 承认 LLM 现阶段对 Quarkdown 的扩展函数还不熟，但基础 Markdown 没问题。这其实道出了所有"Markdown+"语言的一个新瓶颈：能否被 LLM 自然产出，会直接影响生态扩散速度。
