---
layout: article
title: "Knap：把数据变成 Markdown"
issue: 810
number: 30
category: data
original_url: "https://knap.md/"
hn_url: "https://news.ycombinator.com/item?id=49649109"
date: 2026-09-18
---

## 文章摘要

Knap 是一门专为 Markdown 设计的小型模板语言，由 Obsidian 的设计负责人 kepano 开源发布（MIT 许可）。它的定位很窄也很明确：把结构化数据（JSON）通过模板渲染成 Markdown 文件，只提供三样东西——变量、过滤器、逻辑。

语法上它是 Liquid/Jinja 风格的：用双花括号插入变量，用管道符串联过滤器，用 `if` / `for` 之类的标签块控制渲染。官网用一部电影的数据做示例，逐步演示了这套东西怎么工作：

{% raw %}
```
# {{ title }}

**Plot:**
{{ plot | blockquote }}
```
{% endraw %}

`blockquote` 这个过滤器会把纯文本段落转成 Markdown 引用块（前面加 `>`）。过滤器的价值在数组场景下更明显——把演员表这样的对象数组先 `sort:"Actor"` 排序，再 `table` 一下，就直接渲染成一张 Markdown 表格。配合 `if` 标签，还可以做到「有数据才渲染这一段」，避免生成一堆空标题。

{% raw %}
```
{% if cast %}
{{ cast | sort:"Actor" | table }}
{% endif %}
```
{% endraw %}

另一个重点用例是 YAML frontmatter。Knap 提供了针对笔记应用的扩展过滤器，比如 `wikilink` 把值包成 `[[双链]]` 格式、`yaml_property:"directors"` 把数组渲染成合法的 YAML 列表属性，于是可以从原始数据一次生成带完整元数据头的 Obsidian 笔记。

使用方式有两种。命令行：

```
npx knap render template.md --data data.json --output note.md
```

或者作为库嵌入自己的应用，`npm install knap`（pnpm / yarn / bun 同理），通过 API 调用。官网还带了一个可交互的 Playground，可以直接在浏览器里改模板看输出。

名字来自 knapping（打制石器），即通过敲击石头制成箭头、刮削器等工具的工艺。项目的来历也解释了它为什么长这样：Knap 最初是 Obsidian 内部的模板语言，现在被抽出来让任何工具都能用。目前官网列出的两个使用者都来自 Obsidian 生态——Obsidian Web Clipper（用可定制模板把网页存成 Markdown）和 Obsidian Importer（把各种应用和文件格式的数据转成可移植的 Markdown 文件）。

换句话说，它不是又一个通用模板引擎，而是一个「输出目标已知为 Markdown」的模板引擎：过滤器库是围绕 Markdown 和笔记应用的实际需求设计的（引用块、表格、wikilink、YAML 属性），这也是它区别于直接拿 Handlebars 或 Jinja 来拼字符串的地方。

## HN 评论精华

需要如实说明：这条提交虽然拿到 45 分，但讨论极其稀少，整个评论树里只有三条回复，没有任何技术争论或深入分析。三条评论全是正面的简短反应：

- **snthpy** 给了一句最准确的概括性总结：「tl;dr 这是一门从 Obsidian 出来的、用来生成 Markdown 的模板语言/工具。看起来挺有用。」
- **RestartKernel** 说出了唯一一条带定位判断的评价，也是这三条里信息量最大的：「我还挺喜欢这个的。它正好落在一个位置上——用 LaTeX 或 Typst 是杀鸡用牛刀，而纯 Markdown 又太简单了。」
- **dddw** 贡献了一条语言学趣闻：「看起来很聪明！（Knap 在荷兰语里就是聪明的意思。）」

由于社区反馈几乎为零，关于这门语言的关键问题——它与既有模板引擎（Liquid、Jinja、Handlebars）相比在 Markdown 场景下究竟省了多少事、过滤器库的覆盖面如何、以及从 Obsidian 内部抽出来之后能不能真的在其他生态里长起来——都还没有第三方的讨论可供参考。上面的摘要因此完全基于官网内容撰写。
