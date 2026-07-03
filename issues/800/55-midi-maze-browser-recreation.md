---
layout: article
title: "浏览器复刻雅达利 ST 经典游戏 MIDI Maze"
issue: 800
number: 55
category: fun
original_url: "https://github.com/diegoparrilla/midi-maze-js"
hn_url: "https://news.ycombinator.com/item?id=48710543"
date: 2026-07-03
---

## 文章摘要

midi-maze-js 是对 1987 年雅达利 ST（Atari ST）经典游戏 MIDI Maze 的"忠于原线路"（wire-faithful）的浏览器复刻版本。原版 MIDI Maze 由 Xanth Software F/X 开发、Hybrid Arts 发行，是一款极具历史意义的作品：它最多可将 16 台 Atari ST 通过各自的 MIDI 端口首尾相连成一个环（ring），让玩家共享同一个第一人称竞技场，在其中操控一张张咧嘴笑脸互相射击。它可以说是史上第一款联网多人第一人称"死斗（deathmatch）"游戏，比《Doom》（1993 年）早了好几年，后来还被移植到 Game Boy，即 Faceball 2000。

这个复刻项目从重构出的 C 源码逐行移植而来，力求还原原作行为。代码主体使用 TypeScript 编写（约占 57.6%），可在桌面和移动端的现代浏览器中直接游玩。值得一提的是，项目本身是"用 Claude Code（Claude Opus 4.8）构建"的，并严格按照逆向工程得到的源码逐行实现。

功能上，游戏支持单人模式（离线对战 AI 无人机 drone）和联网模式（通过 WebSocket 经由一个 orchestrator 与真实的 Atari ST 组成同一个环）。它完整实现了第一人称视角与准星瞄准、迷宫、多种无人机类型、计分、组队、生命值和换弹等机制，还提供了显示游戏状态与互操作校验和（interop checksums）的调试覆盖层。项目采用 Vite、TypeScript、ESLint 等现代工具链，并用 Playwright 自动生成截图用于文档。

## HN 评论精华

该帖在提交时评论区尚无讨论，暂无高赞评论可供提炼。

从项目本身来看，它最值得关注的两点在于：一是对早期联网 FPS 历史的考古价值——MIDI Maze 通过 MIDI 环网实现的多人死斗，被视为 Doom 之前的联网多人射击雏形；二是其"wire-faithful"的实现思路，即不仅复刻玩法，还忠实还原原始网络协议，使得浏览器版本能够与真实的 Atari ST 硬件跨设备联机对战。此外，项目公开说明由 Claude Code 辅助、按逆向源码逐行移植完成，也为"AI 辅助复刻老游戏"提供了一个可参考的实践样本。
