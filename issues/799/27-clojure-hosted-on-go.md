---
layout: article
title: "Glojure：托管在 Go 之上的 Clojure"
issue: 799
number: 27
category: code
original_url: "https://github.com/glojurelang/glojure"
hn_url: "https://news.ycombinator.com/item?id=48578326"
date: 2026-06-26
---

## 文章摘要

Glojure 是一个在 Go 运行时环境中实现的 Clojure 解释器。项目自我定义为"一个托管（hosted）在 Go 之上、具备可扩展互操作支持的 Clojure 解释器"。所谓"托管"，意味着 Go 的值和 Clojure 的值可以互换——所有 Go 库都能从 Clojure 代码中访问，反之亦然。

与大多数基于 Go 的 Clojure 实现不同，Glojure 是一门"托管"语言。这一术语表明 Clojure 是运行在 Go 之上的，而非被独立编译。这种设计哲学正好对应了标准 Clojure 在 JVM 上的运作方式——在那里，Java 库可以与 Clojure 代码无缝集成。换句话说，Glojure 试图把 Clojure 之于 Java 的关系，复刻到 Go 生态上。

在功能方面，Glojure 提供了完整的开发体验：交互式 REPL，支持 vi/emacs 编辑模式、Tab 补全和持久化历史记录；支持命令行表达式求值和脚本执行；也支持创建独立的可执行程序。互操作能力是其核心亮点——可以直接访问 Go 的标准库（fmt、net/http、time、strings、math 等 20 多个包），通过把 Go 的 "/" 分隔符转换为 ":" 来避免符号冲突，并可通过代码生成工具暴露自定义的包。此外，开发者还可以把 Glojure 作为脚本层嵌入到 Go 应用中，让用户通过 Clojure 插件扩展程序，或添加可脚本化的配置。

项目明确说明仍处于早期开发阶段，"预计会有 bug、缺失的功能和有限的性能"，在 1.0 版本之前不保证向后兼容。实现上采用的是树遍历（tree-walk）解释器，这与 let-go 等采用字节码解释器的移植项目有所不同。类型映射上也存在差异，例如 Clojure 的 `long` 对应 Go 的 `int64`，而 `char` 则映射到自定义的 `lang.Char` 类型，而非 Go 标准的 16 位字符。安装需要 Go 1.24+，通过 `go install github.com/glojurelang/glojure/cmd/glj@latest` 即可。

## HN 评论精华

**adityaathalye（8 天前）** 报告了重要进展："工作已经完全迁回了上游的 glojurelang/glojure 仓库。"该项目现在获得了 Clojurists Together 的资助，已完成 20 多个版本的发布。其目标是让 Clojure 在 Go 开发中的地位，能像它在 Java 中那样突出。

**didibus** 强调了 Glojure 的竞争优势：相比其他 Clojure-on-Go 的实现，Glojure 提供了"完整、正确的互操作"，并具备"AOT 到 Go 源码的流水线"，能够直接编译 Go 库。

**gregwebs** 从实用角度表达了认同："Go 的运行时、工具链和生态系统都很棒——以它为目标平台是合情合理的。"

**shikck200** 提出了关于 REPL 的技术疑问：它究竟是编译成 Go 再执行，还是包含一个虚拟机？他指出大多数 Go 的 REPL 在性能上都很吃力，因为它们必须编译再执行，而非真正地求值。

**jonathanstrange** 指出了一个潜在短板：项目缺乏"安全或沙箱机制"，这可能会限制它在嵌入不受信任脚本场景中的应用。

**nathell** 补充说，let-go 等竞争项目也在这个领域快速推进，生态正变得越来越活跃。
