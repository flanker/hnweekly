---
layout: article
title: "在 iPhone 上编写一款 GBA 游戏"
issue: 797
number: 52
category: fun
original_url: "https://blog.adamledoux.net/posts/2026-06-08-programming-a-gba-game-on-an-iphone.html"
hn_url: "https://news.ycombinator.com/item?id=48468495"
date: 2026-06-12
---

## 文章摘要

作者完全在 iPhone 上做出了一款 Game Boy Advance（GBA）游戏《TO THE TOWER》——这是一个 bitsy 风格的小游戏。整个开发、编译、测试流程都在手机上完成，无需任何台式机或笔记本。

作者主要用到了四款 iOS 应用：

- **iSH**：一个运行在 iOS 上的 Alpine Linux 终端环境，可以直接安装 ARM 工具链。
- **编译**：在 iSH 中使用 gba-bootstrap 工具链配合 gcc-arm-none-eabi。
- **Textastic**：专为 iOS 设计的代码编辑器。
- **Delta 模拟器**：用于在设备上直接运行测试游戏。

这篇文章的核心是证明"在手机上完整地做出一款 GBA 游戏是可行的"，把一个看似异想天开的念头变成了真正可玩的成品。游戏最终发布在 itch.io 上。

## HN 评论精华

**kmeisthax** 提出了一个绕开 iOS 限制的创意思路：把一个复古汇编 IDE 伪装成"教育类"应用，从而合法地把模拟功能"夹带"上架到 App Store。

**socalgal2** 给出了技术上的澄清：iOS 模拟器仍然受 JIT 限制和"仅限游戏"权限的约束，纠正了此前关于"苹果已放开政策"的说法。

**akkartik** 分享说自己也用 LÖVE 在手机上编程，目的是重现"微型计算机时代照着杂志敲入程序"的那种怀旧体验。

**msephton** 分享了用 Lua/Love2D 在 iPhone 上做游戏的经历，并附上了相关 iOS 游戏开发工具的资料链接。

**chroma_zone** 认可这种非常规的开发环境，并提到自己大学时用 Termux 在手机上写代码，发现移动端开发工具确实很有用。

**Xeoncross** 希望作者补充更详细的移动开发工作流文档，认为这能成为碎片时间里"刷手机"之外更有产出的选择。
