---
layout: article
title: "4D DOOM（HYPERHELL）-- 第一款四维类 DOOM 游戏"
issue: 788
number: 55
category: fun
original_url: "https://github.com/danieldugas/HYPERHELL"
hn_url: "https://news.ycombinator.com/item?id=47549128"
date: 2026-04-03
---

## 文章摘要

HYPERHELL 是开发者 Daniel Dugas 创作的一款自称"史上第一款四维类 DOOM 游戏"的实验性项目。玩家在一个真正的四维空间迷宫中探索，遭遇迷失的灵魂、恶魔和暗天使。游戏的核心叙事围绕一个名为"交易者"（Bargainer）的角色展开，他提供一种改变自身以超越维度限制的交换，这可能是逃离四维迷宫的唯一途径。

技术上，Dugas 探索了多种四维环境的渲染方法，最终开发了一种通过"4D 之眼"（一个配备 3D 传感器的相机）进行渲染的新方法。他使用 96x96x96 的体素网格模拟四维摄像机，这导致了极大的计算开销。游戏引入了名为"Unblink"的独特机制，利用 4D 眼传感器渲染实现直觉化的四维导航和感知。项目基于 WebGPU 技术，需要兼容的浏览器（推荐 Chrome）和 GPU（在 Macbook M1/M2 和 Nvidia GPU 上测试通过）。代码库由 58% HTML 和 42% JavaScript 组成，在 GitHub 上获得了 175 颗星。可在 dugas.ch/hyperhell 在线体验。

## HN 评论精华

**分辨率与可玩性争议**：keyle 抱怨极低分辨率影响了四维游戏中本就需要高度清晰度的体验。somat 解释这源于用 3D 体素网格模拟 4D 相机的技术限制。开发者 chronolitus 详细说明了 96x96x96 体素网格带来的巨大计算成本，并添加了自定义分辨率 URL 参数作为解决方案。

**四维游戏设计探讨**：knolan 推荐了 CodeParade 的《4D Golf》作为更好的高维空间探索游戏。martin-t 建议使用透明度和基于移动的感知来改善体验。gipp 认为第四维度缺乏与其他维度相同的可见性，造成了困惑。pasquinelli 澄清说维度本质上是等价的，人类实际上只能直接"看到"两个维度。

**与其他项目对比**：danbruc 两次提到了开发了 16 年仍未发布的《Miegakure》（一款 4D 解谜游戏），多位评论者表示它仍在他们的 Steam 愿望单上。omershapira 推荐了自己的 4D 第三人称游戏《Horizon》。tenthirtyam 注意到与 2002 年立体视觉 4D 迷宫的相似性。

**技术反馈**：forthac 提供了 Firefox WebGPU 的设置说明。burnt-resistor 反馈了 iOS/iPad 上对键盘的依赖和 UX 问题。npodbielski 抱怨反转的 X 轴和缺乏进度系统。razorbeamz 批评了 AI 生成的 logo，开发者表示更喜欢人工创作的艺术品并愿意付费委托。

**认知与感知讨论**：dluan 好奇年轻大脑是否能更好地适应高维思维。scordata 认为该项目对可视化高维思维很有价值。bstsb 表示游戏虽然视觉上奇怪但确实有趣。
