---
layout: article
title: "和 LLM 搭档，请用「无聊」的语言"
issue: 795
number: 21
category: code
original_url: "https://jry.io/writing/use-boring-languages-with-llms/"
hn_url: "https://news.ycombinator.com/item?id=48237012"
date: 2026-05-29
---

## 文章摘要

**Jacob Young** 提出一个有意思的论点：大语言模型在**一致、同质的代码库**上训练后，能生成更高质量的代码。换句话说，**「LLM 会放大不一致的技术栈，又悄悄强化一致的技术栈」**——碎片化的生态会让智能体产出更差，而标准化的生态则产出更好。他的核心原则是：**训练语料里方差越低的语言和生态，越能被编程智能体可靠地表达和执行。**用向量空间的话说，一致的训练数据带来一致的推理结果。

据此他把语言分成两类。**「无聊」（适合 LLM）的**：Go、Ruby on Rails、以及用法统一的 Python。**「碎片化」（麻烦）的**：JavaScript/Node 生态、以及包管理器多到打架的 Python。他指出 JavaScript「至少有十几个生产级框架」功能彼此重叠地竞争，而「Rails 基本上只有一个」；Go 则因为「抗拒便利特性」反而意外地成了理想选择。

文章重点夸了 **Go**：(1) **goroutine** 比 async/await 或回调更简单，没有「函数染色（colored function）」问题；(2) `net/http` 和加密标准库世界级且使用广泛；(3) `gofmt`、`go vet`、`golangci-lint` 自动强制单一规范风格；(4) **垃圾回收**避免了智能体犯内存错误（不像 Rust 的借用检查那么复杂）；(5) 失败模式有界、「坑（footgun）」比 Python 这类动态语言少。作者并未宣称 Go「客观最好」，而是给出务实结论：**「这门语言和工具链足以写出一个工作工程师想要的大多数非可视化软件。」**

## HN 评论精华

- **wryoak**：报告用 Opus、GPT、GLM 等模型扩展 Elm 编译器的良好体验，即便是非主流语言也没出现明显幻觉。
- **BoumTAC / gampleman**：用 Grok Build 写 Elm，称赞编译器反馈能帮 LLM 迭代、elm-review 保证质量；并指出模型搞不定 Elm 是两年前的事了，如今的模型已能很好处理。
- **amarant**：强调**编译期错误对 AI 的帮助大于运行时错误**；其个人体验是 gdscript 表现最差，Rust 因严格而表现最好。
- **echelon / sunshowers**：认为 Rust 与 LLM 配合极佳，连 DeepSeek 这类较弱模型也能有效处理；并指出**和类型（sum types）与穷尽式模式匹配**对 LLM 代码生成大有裨益。
- **tikhonj**：提出**训练数据量比语言设计更重要**——新语言需要巨大的代码语料才能让智能体好用。
- **lmm**：把「无聊」重新表述为达到「成功之坑（pit of success）」，但警告 Go 广为人知的坑不代表它没有更隐蔽的坑。
- **quietbritishjim**：不认同 goroutine 让 LLM 的并发更简单，反而主张结构化并发（Trio、asyncio.TaskGroup、Kotlin、Swift）。
- **Weebs**：指出 LLM 始终会幻觉，比如问 C# 接口实现时会声称它支持局部类等不存在的特性。
- **tlonny**：提出**适度（而非过度）的类型检查最优**——太多（如 Rust）会让智能体卡在架构的局部极小值里出不来。
- **jaen**：认为带强 REPL 的 Clojure 能给智能体更优的反馈循环，可实时内省而无需重新编译。
- **jryio（作者）**：澄清「无聊」指的是在语法、生态和工具链上低熵、随时间缓慢复利的语言，并把 Ruby 和 Rust 也列为有力候选。
