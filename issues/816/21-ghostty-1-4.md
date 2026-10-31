---
layout: article
title: "Ghostty 1.4：压缩回滚记录、完善图形协议并修复安全问题"
issue: 816
number: 21
category: code
original_url: "https://ghostty.org/docs/install/release-notes/1-4-0"
hn_url: "https://news.ycombinator.com/item?id=50261998"
date: 2026-10-30
---

## 文章摘要

Ghostty 1.4 在 10 月 27 日发布，重点包含终端性能、内存占用、Kitty 图形协议和 SSH 集成。不可见的回滚记录现在可透明压缩，官方报告典型文本历史的物理内存减少 70% 至 90%；隐藏标签页也会释放 GPU 资源。macOS 的读取与解析分离后，维护者在指定机器和负载下测得吞吐提升。这里应区分内存、解析速度和整条 I/O 路径，不能把某项倍数推广到所有终端操作。

发布说明同时列出多项安全修复，涉及粘贴保护、恶意终端输出、图形指令和 macOS 链接打开等路径，触发条件各不相同。功能方面，Kitty 图形动画和相对布局得到补齐，Linux 渲染流程调整以改善分数缩放，macOS 27 的窗口和标签页问题也有修正。这是一轮终端核心与平台整合的更新，而不仅是更换视觉主题；评估升级时，兼容性和安全修复与速度同样重要。

## HN 评论精华

**作者更看重可复用终端核心。** mitchellh 强调 libghostty 才是长期目标：把终端状态机提供给 IDE、编辑器和其他应用。他介绍了跨平台构建、语言绑定与下游采用情况，这些属于维护者报告。

**用户需求不完全相同。** cassianoleal 觉得现有 Konsole 已足够快，最缺的是 tmux 集成；teroshan 留在 WezTerm，原因是 Windows 与 Linux 可共享配置。选择终端不能只靠单项速度。

**远程环境仍有边界。** Hitech-Redneck 提到 SSH 中的异常，awkwardleon 建议检查远端是否认识 Ghostty 的终端类型。Lerc 则分享用 Kitty 图形显示编码代理渲染结果的玩法，展示了协议能力的实际用途。
