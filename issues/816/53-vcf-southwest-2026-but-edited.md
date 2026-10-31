---
layout: article
title: "用 SGI 工作站，把 VCF Southwest 2026 剪成九十年代电视节目"
issue: 816
number: 53
category: watching
original_url: "https://www.youtube.com/watch?v=46wxttvq9qY"
hn_url: "https://news.ycombinator.com/item?id=50223435"
date: 2026-10-30
---

## 文章摘要

这段视频以复古计算机展 VCF Southwest 2026 为题材，尝试用 Silicon Graphics 工作站制作带有九十年代电视风格的节目。本次未能取得可阅读的字幕或视频正文，以下技术线索来自 HN 观众的具体描述，不是对视频全流程的独立核验。

评论者 jdboyd 指出其中使用的是 SGO Jaleo 视频编辑软件，并讨论了 SGI O2 的 FireWire 扩展和 SDI 录像设备之间的采集选择。发帖者 zdw 则特别关注让旧系统使用 NVMe 的内核模块，以及 PCI、PCIe、M.2 之间的转接组合。它的吸引力因而不仅是复古画面，还在于老软件、专用设备与新存储技术如何接起来。

这也引出一个工作流问题：围绕单一任务设计的实体控制与界面，和如今通用计算机上的编辑工具，各自保留了什么优势？评论提供了有趣的使用经验，但没有可核实的性能对比或完整复现步骤；想确认接线与剪辑细节，仍需直接观看原视频。

## HN 评论精华

- **喜欢外观，不等于喜欢交互。** josteink 欣赏 CDE 的视觉风格，却不想照搬它的操作方式；jeffbee 用演示者差点误点退出的片段说明这种矛盾。
- **难得看到旧编辑软件实际运行。** jdboyd 认出 SGO Jaleo，并认为已有 SDI 录像设备时，改用 FireWire 未必让采集更简单；jeffbee 补充了 O2 官方 1394 卡的 DV 支持限制。
- **新存储进入旧系统。** zdw 最感兴趣的是 NVMe 驱动和多层转接方案。这是观众对演示的描述，不是本文测试所得。
- **专用工作站与通用工具的取舍。** ChuckMcM 怀念围绕具体任务安排的控制器，同时认为今天可以更低成本搭建类似工作站；这属于个人判断，并无基准测试支持。
