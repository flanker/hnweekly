---
layout: article
title: "Lisette：受 Rust 启发、编译为 Go 的小型编程语言"
issue: 789
number: 24
category: code
original_url: "https://lisette.run/"
hn_url: "https://news.ycombinator.com/item?id=47646843"
date: 2026-04-10
---

## 文章摘要

Lisette 是一门受 Rust 启发的小型编程语言，由 ivov 开发，其核心理念可以概括为"Rust 语法，Go 运行时"。它试图将 Rust 最受开发者喜爱的类型系统特性和表达力带入 Go 的成熟生态系统和运行时环境中，从而在两者之间找到一个独特的平衡点。

Lisette 引入了几个 Go 语言原生不支持但开发者长期渴望的核心特性。首先是代数数据类型（Algebraic Data Types），这使得开发者可以精确地建模复杂的数据结构，用类型系统表达"一个值可以是 A 或 B 或 C"这样的概念。其次是模式匹配（Pattern Matching），提供了比 Go 的 switch 语句更强大、更安全的分支控制能力，并支持穷举性检查——编译器会确保你处理了所有可能的情况。

第三个关键特性是 Hindley-Milner 类型推断系统，这意味着在大多数情况下你不需要显式声明类型，编译器能够自动推导出正确的类型，同时仍然保证完全的类型安全。第四个特性是默认不可变性——Lisette 中的绑定默认是不可变的，这符合函数式编程的最佳实践，有助于编写更安全、更可预测的代码。

在 Go 互操作性方面，Lisette 保留了 Go 风格的接口、通道（channels）和 goroutine，可以直接调用 Go 标准库。这意味着开发者不需要放弃 Go 丰富的包生态系统。错误处理方面，Lisette 使用了 Result 类型和 `?` 操作符来简化错误传播，这比 Go 经典的 `if err != nil` 模式更加简洁。

一个重要的设计决策是 Lisette 不支持 nil。与其让 nil 到处传播引发难以调试的空指针错误，Lisette 使用 `Option<T>` 来显式编码值的缺失。这是直接借鉴了 Rust 和 Haskell 的做法。然而这也引入了一个实际问题：当与纯 Go 代码交互时，Go 的类型化 nil（typed nil）意味着一个接口类型的变量在进入 Lisette 时应该表示为 `Option<Option<http.Handler>>`，虽然可以通过匹配 `Some(Some(h))` 来避免两步解包，但仍然显得笨拙。

Lisette 的项目网站（lisette.run）提供了完整的文档和示例，GitHub 仓库（ivov/lisette）包含了源代码和变更日志。该项目在"编译到 Go"的语言列表中引起了关注，与 Borgo（另一个受 Rust 启发的编译到 Go 的语言）以及 Sky（受 Elm 启发的编译到 Go 的语言）等项目形成了一个有趣的生态。

## HN 评论精华

1. **typed nil 的尴尬**：讨论中最具技术深度的话题围绕 Go 的 typed nil 问题展开。评论者指出，Go 的接口类型变量可以是"接口本身为 nil"和"接口包含的具体类型为 nil"两种不同状态，这在 Lisette 中需要表示为 `Option<Option<T>>`，使用起来相当别扭。这暴露了在 Go 类型系统之上叠加 Rust 风格类型系统的根本挑战。

2. **与 Borgo 的比较**：多位评论者将 Lisette 与 Borgo（另一个 Rust-to-Go 语言）进行对比。有人认为这个生态位越来越拥挤——Borgo、Lisette、Sky 都在做类似的事情。讨论焦点在于不同项目在类型推断、FFI 边界处理和运行时开销方面的取舍。

3. **defer 语句的局限**：评论者指出 Lisette 并没有消除 Go 中调用 defer 的需要，这令一些人失望。Rust 使用 Drop trait 实现的 RAII 模式是其资源管理的核心优势之一，但由于 Lisette 编译到 Go，它受制于 Go 运行时的垃圾回收模型，无法提供类似的确定性资源释放。

4. **"为什么不直接用 Rust"的质疑**：不出意料，有人提出了这个根本性问题。支持者的回答是：Lisette 的价值在于你已经有一个 Go 代码库或团队，但想要更好的类型安全。完全迁移到 Rust 的成本远高于在现有 Go 项目中渐进式引入 Lisette。

5. **对编程语言多样性的积极态度**：尽管有人质疑这类项目的实用性，但也有评论者持开放态度，认为每一种新的编程语言尝试都在探索设计空间中的不同点，即使最终不被广泛采用，也能为主流语言的演进提供灵感。Go 最近引入泛型就是一个例证——来自其他语言的压力推动了 Go 的进化。
