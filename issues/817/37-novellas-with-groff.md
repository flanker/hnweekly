---
layout: article
title: "用 groff 排版自己的中篇小说"
issue: 817
number: 37
category: books
original_url: "https://movq.de/blog/postings/2026-10-31/0/POSTING-en.html"
hn_url: "https://news.ycombinator.com/item?id=50325694"
date: 2026-11-06
---

## 文章摘要

movq 写完两部私人小说后，分享了写作和排版工具链。第一部约一万五千词、五十页 A5，耗时七个多月；第二部约四万词、一百二十页，写了十天。这是个人创作经历，不是通过工具保证的生产率指标。作者也明确说不会公开小说，因此技术文章展示的是流程与示例，不是邀请读者索取故事原稿。

核心排版器是 groff，也就是常被用来生成 Unix 手册页的系统。对于只有章节、段落和少量强调样式的小说，作者自己编写了很小的宏包，逐步处理页码、页眉、页脚、分页陷阱、句间距和德语断词。文章详细展示了排版状态与命令之间的关系，并坦承有些细节仍未完全理解，保留 TODO，而非把方案包装成无需调整的通用模板。

作者看重它的小巧和速度：在自己的老电脑上，两部作品的构建分别约需 700 毫秒与 1.6 秒，包含预处理。写作端又结合 Vim、轻量标记和 Git，把正文编辑、版本管理与成品生成拆开。它说明老工具仍可满足明确且有限的需求，不过也暴露了自建工具链的取舍：页面控制力更强，同时要自己负责格式规则与异常情况。对小说写作者而言，最终衡量标准仍是能否持续完成内容，而不是管线有多精巧。

## HN 评论精华

**选择远不止一种。** [tmoertel](https://news.ycombinator.com/item?id=50326743) 分享从 Google Docs 到 Markdown、Git、Emacs，再用 Typst 生成 PDF 的流程；[sloum](https://news.ycombinator.com/item?id=50326777) 则使用 HTML、打印样式和 WeasyPrint。

**别把调工具当成写小说。** [pmdr](https://news.ycombinator.com/item?id=50326911) 提醒这类管线也可能成为拖延。tmoertel 随后自嘲，封装容器、字体和 Makefile 很容易占掉本该写作的时间。

**电子书不是随意压缩 HTML。** [forgotpwd16](https://news.ycombinator.com/item?id=50328602) 说明 EPUB 有规定的结构与元数据，脚本或应用的意义在于处理这些约束，而非仅仅替用户执行压缩。
