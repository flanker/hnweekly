---
layout: article
title: "Cheese Paper：一款专为写作设计的文本编辑器"
issue: 796
number: 17
category: show_hn
original_url: "https://brie.gay/cheese-paper/"
hn_url: "https://news.ycombinator.com/item?id=48341407"
date: 2026-06-05
---

## 文章摘要

Cheese Paper 是一款免费、开源、专为小说创作者设计的文本编辑器。它的核心理念是把写作项目存储为简单的纯文本文件，这些文件在任何文本编辑器中都能继续编辑，从而把控制权和离线能力牢牢交还到用户手中。

在功能组织上，Cheese Paper 通过场景文件（scene files）、角色档案（character profiles）和世界观设定笔记（worldbuilding notes）来管理整部作品。作者在撰写正文时可以让相关笔记与手稿文本并排显示，方便随时参考。项目还支持灵活的导出能力：大纲导出会把所有元数据汇编进一份文档，而故事导出则会把各个场景合并为 markdown 格式，再借助 Pandoc 等工具进一步转换成 EPUB、PDF 等格式。

这款软件最鲜明的特点是它对数据所有权和隐私的极致强调。它没有任何遥测（no telemetry），网络请求也降到最低，所有文件都保留在用户自己的电脑上，除非你主动通过 Syncthing 或 Nextcloud 等服务进行同步。作者明确表示，Cheese Paper"不是一个会出卖你数据的在线服务"，以此与那些订阅制的竞品划清界限。在差异化定位上：与把笔记和正文分散在不同文件里的 Obsidian 不同，Cheese Paper 把笔记直接与场景整合在一起；与 Scrivener 和 Manuskript 相比，它更看重简洁性和文件的可移植性，并且能在运行时接受外部编辑而不会崩溃。底层采用的 markdown 和 TOML 格式，让它能与外部程序和各类同步方案无缝衔接。

## HN 评论精华

1. **"段落即节点"的写作设想**：用户 aleda145 最近重拾短篇小说创作，他比较了几款编辑器后最终用日常主力工具 VS Code 来写。他表达了一个长久以来的梦想——做一款把每个段落都当作"工作单元"的编辑器，整篇文章由不同段落像图中的节点一样链接而成，这样就能轻松尝试不同写法而无需删除任何文字。他表示下次创作时一定会试试 Cheese Paper。

2. **标题应明确是"虚构写作"**：用户 blacksmith_tb 建议，如果标题写明"专为虚构（小说）写作设计"或许会更清楚，因为很多其他类型的写作并不涉及"角色"这样的概念，而 Cheese Paper 的角色档案功能显然是面向小说的。

3. **纯文本文件夹格式才是正解**：用户 pooploop64 提到了类似理念的老牌工具 CherryTree，但批评它使用了一种"没人在乎的特殊数据库格式"，导致无法在移动端使用。他盛赞 Cheese Paper 用"普通文件夹+普通纯文本文件"作为数据格式的做法——"在理想世界里一切都该如此：你的文件就是你的文件，客户端程序只是帮你更高效地使用它们。"

4. **众多同类工具的对比**：评论区俨然成了写作工具大赏。tlhunter 推荐 Linux 上的 Manuskript，称其功能更多、界面更好看；abricot 提到自己在用的 NovelWriter；citizenkeen 则一句话点题："这是 markdown 版的 Scrivener 吗？"此外 n3storm 对 Cheese Paper 和 Manuskript 都偏"压缩式"的 UI 设计提出了批评，认为元素之间靠分隔线硬塞在一起是对 Fitts 定律的滥用。

5. **org-mode 党与玩梗党**：vim-guru 抛出经典质疑——既然 Emacs 的 org-mode 早已存在且几乎能胜任任何场景，为什么还要这么多支持 markdown 写作的新工具？而 personjerry 则贡献了一句神吐槽："和其他那些主要用来玩 Nethack 的文本编辑器相比……"（调侃 Vim/Emacs 等"万能编辑器"），buggylearning 则在追问能否做成 VS Code 插件。
