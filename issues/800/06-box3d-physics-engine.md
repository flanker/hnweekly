---
layout: article
title: "Box3D：一款开源 3D 物理引擎"
issue: 800
number: 6
category: favorites
original_url: "https://box2d.org/posts/2026/06/announcing-box3d/"
hn_url: "https://news.ycombinator.com/item?id=48745445"
date: 2026-07-03
---

## 文章摘要

Box3D 是知名 2D 物理引擎 Box2D 的作者 Erin Catto 推出的全新开源 3D 物理引擎，完全用 C17 编写，已在 GitHub 上发布。它继承了 Box2D 经过验证的设计理念，功能相当完整：支持三角网格（triangle mesh）与高度场（height-field）碰撞、烘焙式复合碰撞（baked compound collision）、连续碰撞检测（continuous collision detection）、可选调度的多线程、宽 SIMD 接触求解器（wide SIMD contact solver）、用双精度浮点表示位置以支持大世界，以及跨平台确定性（determinism）与录制/回放功能。

Box3D 的诞生源于实际需求。Catto 在 Kintsugiyama 工作室开发一款生存类游戏《The Legend of California》，但 Unreal Engine 自带的物理系统 Chaos 存在诸多局限——缺少陀螺力矩（gyroscopic torque）支持、网格碰撞处理不佳。与其和这些约束搏斗，他决定自研方案：最初 fork 了 Valve 的 Rubikon-Lite，随后融入 Box2D 的各项优化，逐步演化为 Box3D。

除了满足游戏开发的具体需要，Catto 也希望通过开源工作把自己积累的物理引擎知识在不同工作之间传承下来。目前 Box3D 已被多个项目采用，包括 Facepunch Studios 的 s&box 平台和若干独立多人游戏。

## HN 评论精华

讨论主要围绕 Box3D 在现有物理引擎生态中的定位展开。

- **HexDecOctBin** 好奇 Box3D 与 Jolt 的对比——两者血统都很硬（一个出自 Valve 与 Erin Catto，另一个用于《地平线》系列游戏）。

- **sph** 点出了 Box3D 的一大优势：用 C 编写意味着它能为 Odin 等其他语言提供符合语言习惯的绑定。此前有人尝试把 Jolt（C++）绑定到 C 再到 Odin，结果"体验很不愉快"。

- **flohofwoe** 梳理了开源 3D 物理引擎的稀缺历史："远古先驱"是 ODE、Bullet 和 Newton Dynamics（都在 2000 年代初首发），之后近二十年几乎空白，直到 2021 年的 Jolt，如今又有了 Box3D。他认为这个小而精的名单每增加一员都值得欢迎。这条评论还引出了 **erwincoumans**（Bullet 原作者）的现身感慨"让我感觉自己老了"。

- **artifact_44** 从 Web 生态角度补充了大量选项（ammo.js、cannon.js、rapier.js、jolt.js、physx.js 等），并根据自身经验认为 PhysX 目前最稳健。**mikulas_florek** 则提醒 NVIDia 已在 2018 年将 PhysX 开源。

- 一条温情的支线：**RobLach** 回忆 Box2D 曾是许多独立物理游戏的基石，好奇如今生态是否足够空白以迎来复兴；由此勾起 **mangogogo**、**thederf** 等人对 IncrediBots 的集体怀旧，后者还分享了自己在疫情期间完成的 IncrediBots-2 HTML5 开源移植版。
