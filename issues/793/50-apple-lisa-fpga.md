---
layout: article
title: "我用 FPGA 重新实现了 Apple Lisa 电脑"
issue: 793
number: 50
category: watching
original_url: "https://www.youtube.com/watch?v=8jNQDcpHc68"
hn_url: "https://news.ycombinator.com/item?id=47999460"
date: 2026-05-08
---

## 文章摘要

YouTuber AlexTheCat123 用 FPGA 完整复刻了 1983 年的 Apple Lisa 电脑——这是苹果在 Macintosh 之前推出的第一款图形界面消费级电脑，售价高达 9995 美元（按今天的购买力换算约 3 万美元），主要面向办公专业人士。Lisa 在商业上是失败的，产量稀少、价格昂贵，绝大多数原始样机已经损坏或丢失，这让"用现代硬件重新做一台 Lisa"变成了爱好者圈子里几乎唯一可行的"复活"路径。

作者花了大约 8 个月时间，把 Lisa 的整套硬件——包括 Motorola 68000 CPU、自定义内存映射、CRT 显示控制器、键盘鼠标接口、ProFile 硬盘控制器和那块标志性的"双软驱（Twiggy drive）"接口——全部用 Verilog 写进了一块 FPGA 开发板里。FPGA 实现的好处是可以精确复刻原始时序，运行真实的 Lisa OS（Lisa Office System）和原版应用程序，而不是模拟器那样的"近似执行"。视频里他演示了 LisaWrite、LisaCalc、LisaDraw 等套件在自己的 FPGA 板上正常启动并响应鼠标点击。

作者强调这个项目的目的不仅是怀旧——Lisa OS 里许多设计领先时代：**Stationery（文档模板）机制**让用户可以把任何文档"另存为模板"，双击模板会自动生成同结构的新文档；这一概念后来被简化进了早期 Macintosh 的 Desk Accessories。Lisa 还首次实现了**软关机**（OS 检测到关机请求后保存状态再切电），1983 年就有的这种"优雅关闭"思路要等到很多年后才在 PC 世界普及。AlexTheCat123 希望把 FPGA 板做成开源硬件，让更多 Lisa 软件爱好者不必再去抢购天价老机器。

## HN 评论精华

1. **cyrc**：作为提交者，称赞这是"近年来最有价值的复古计算项目之一"——Lisa 是图形界面历史里被严重低估的关键节点，能跑原版软件的 FPGA 复刻意义重大。

2. **visarga**：指出 Lisa 在概念上的超前——Lisa 的"Stationery（文档模板）"和"Desk Accessories（桌面附件）"两种应用形态划分，事实上是后来 macOS App + Widget、甚至 iOS App + App Clip 这种分层应用模型的鼻祖。

3. **rbanffy**：补充冷知识——macOS 至今仍保留着 Lisa 的 Stationery 机制，在 Finder 里 File > Get Info 可以勾选 "Stationery Pad"，把任何文档变成模板。这个 1983 年的设计活到了 2026 年，少有人知。

4. **Joel_Mckay**：从历史定位角度分析——Lisa 的 9995 美元定价并不是面向消费者，而是 Xerox Star、Wang word processor 这一档"办公专用工作站"的对手。Lisa 失败不是因为设计不好，而是它生错了时代——再过两年 Mac 用同样的 GUI 把价格打到 2495 美元。

5. **JSR_FDED**：强调技术细节令人感动——Lisa 的软电源按钮（press to request shutdown，OS 处理后再断电）在 1983 年是闻所未闻的设计，比 Windows 的"开始 → 关机"流程早了 12 年。

6. **AlexTheCat123（项目作者本人）**：在评论区现身，确认整个项目耗时约 8 个月，目标是把 FPGA 板开源出来，让 Lisa 开发工具（LisaPascal、Workshop 等）能在新硬件上重新被使用，避免随原始机器一起消亡。
