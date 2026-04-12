---
layout: article
title: "Cherri：编译为 Apple 快捷指令的编程语言"
issue: 789
number: 23
category: code
original_url: "https://github.com/electrikmilk/cherri"
hn_url: "https://news.ycombinator.com/item?id=47549824"
date: 2026-04-10
---

## 文章摘要

Cherri（发音为 cherry）是一门编程语言，能够直接编译为合法的、可运行的 Apple 快捷指令（Shortcuts）。项目由 Brandon Jordan（electrikmilk）创建，始于 2022 年 10 月，项目名称致敬了原始 Workflow 应用的倒数第二个更新版本"Cherries"。其核心目标是使创建大型快捷指令项目变得切实可行（在 Shortcuts 的限制范围内），并支持长期维护。

Cherri 解决了 Apple 快捷指令开发中的一个根本痛点：快捷指令的原生编辑器是图形化的拖拽界面，对于简单的自动化任务来说这很直观，但一旦项目规模变大，管理和维护就变成噩梦。没有版本控制、没有代码搜索、没有重构工具，所有操作都必须通过触摸或鼠标完成。Cherri 通过提供文本化的编程方式彻底改变了这一局面。

在技术特性方面，Cherri 提供了丰富的语言功能：类型检查和类型推断、枚举类型、可选参数、文件包含机制（可以在项目中组织多文件结构）、基于 Git 仓库的内置包管理器、以及从 iCloud 链接导入现有快捷指令的能力（`--import=` 选项）。该语言力求与快捷指令动作保持 1:1 的映射关系，使调试更加容易。同时针对运行时内存使用进行了优化，生成的快捷指令尽可能小。

开发工具链相当完善：除了核心的 CLI 编译器外，还有 VSCode 语法高亮扩展、macOS 原生 IDE 应用、在线 Playground 测试环境，以及字形搜索工具。安装方式支持 Homebrew（`brew install electrikmilk/cherri/cherri`）和 Nix（`nix profile install github:electrikmilk/cherri`）。使用调试模式（`--debug` 或 `-d` 标志）可以输出堆栈跟踪和 `.plist` 文件。

Cherri 语言被描述为"半自举"——大部分快捷指令动作是用语言自身编写的。项目采用 Go 语言开发，使用 macOS 签名机制为编译出的快捷指令进行签名，并在签名不可用时回退到外部签名服务器。作者强调"快捷指令语言应该更多而不是更少"，欢迎社区中出现更多类似工具。

## HN 评论精华

1. **AI 与 Cherri 的结合**：一位评论者分享了使用 Claude（AI 助手）从零开始学习 Cherri 语言并编写出完全可编译的快捷指令的经验，认为这令人惊叹——一个 LLM 能够学习一门小众新语言并生成有效代码。这也说明 Cherri 的语法设计足够直观和一致。

2. **Apple 快捷指令生态的讨论**：评论者讨论了 Apple 快捷指令生态的现状。有人指出快捷指令本质上是 Apple 的低代码/无代码平台，但其图形界面在处理复杂逻辑时变得极其笨拙。Cherri 这样的工具填补了"高级用户想要更强大自动化能力"与"Apple 不太可能开放更底层 API"之间的空白。

3. **与其他快捷指令工具的比较**：有评论者将 Cherri 与其他快捷指令开发工具进行对比，如 Jellycuts 等。Cherri 的优势在于它运行在 macOS 上而非 iOS 上，提供了真正的桌面开发体验，并且支持版本控制——这对于严肃的项目开发至关重要。

4. **技术实现的好奇**：一些技术导向的评论者对 Cherri 如何处理快捷指令签名机制感兴趣。Apple 要求快捷指令必须经过签名才能运行，Cherri 的 macOS 签名回退到外部服务器的设计引发了安全性讨论。

5. **对 Apple 平台开放性的反思**：评论中出现了关于 Apple 平台开放性的更广泛讨论。Cherri 的存在本身就说明了用户对 Apple 原生工具局限性的不满。有人认为 Apple 应该官方提供基于文本的快捷指令定义格式，而不是将用户锁定在图形编辑器中。
