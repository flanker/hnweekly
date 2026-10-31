---
layout: article
title: "Windows 11 还能直接运行多老的图形程序？"
issue: 816
number: 48
category: learn
original_url: "https://blog.enlyth.com/posts/oldest-windows-gui-app-win-11/"
hn_url: "https://news.ycombinator.com/item?id=50248055"
date: 2026-10-30
---

## 文章摘要

作者把问题限定为不修改程序、不使用额外模拟工具，在 Windows 11 上直接运行历史 GUI 二进制文件。他先找到 1993 年的 FreeCell，随后继续翻查早期 Windows NT 版本，最终展示了链接日期为 1992 年 11 月的 CDPLAYER.EXE。

更早的候选并非都能成功：早期 PE 格式尚未稳定，Windows API 调用约定也发生过变化。文章用栈清理差异解释某些程序为什么崩溃。CDPLAYER 依靠系统中仍提供的运行库避开了其他候选的障碍；没有光驱时，它能够打开，但作者另外模拟光驱后才演示音频播放。

文末还用 otvdm 运行 1985 年的黑白棋，把这个例子明确列为放宽规则后的补充。文章展示的是作者找到的可运行样本，不是穷尽所有历史软件后证明的世界纪录。

## HN 评论精华

- **旧界面的可发现性。** mijowi 喜欢传统 GUI 的功能性，elxr 特别提到按钮看起来像按钮；也有人怀念菜单栏。
- **源码保留与二进制兼容是两回事。** xacky 提出旧 X11 程序，layer8 追问是否真能让当年的原始二进制直接运行。
- **规则决定答案。** MisterTea 认为系统过去也带模拟层，第三方兼容层未必算作弊；作者 enlyth 解释自己要找的是原版 Windows 双击即用的程序。
- **旧技术仍有实用价值。** userbinator 说早年 Win32 小工具仍在使用；lolpanda 认为简单状态机的原生应用依然响应迅速。这些是用户经验，不是跨框架性能测试。
