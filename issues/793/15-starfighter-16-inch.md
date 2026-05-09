---
layout: article
title: "StarFighter 16 英寸 Linux 笔记本"
issue: 793
number: 15
category: show_hn
original_url: "https://us.starlabs.systems/pages/starfighter"
hn_url: "https://news.ycombinator.com/item?id=48031261"
date: 2026-05-08
---

## 文章摘要

英国小众 Linux 笔记本厂商 Star Labs 时隔三年半终于把 2022 年 11 月就预告过的 StarFighter 16 英寸机型送上市，起售价 1878 美元。这是该厂迄今最大、规格最强的产品：可选 Intel Core Ultra 或 AMD Ryzen 9，最高 64GB LPDDR5X-7500 内存，16 英寸 16:10 比例的 3840×2400 4K IPS 屏，120Hz 刷新率、625 cd/m² 亮度、178° 可视角，电池续航号称最长 18 小时。

机身用了不太常见的**等离子电解氧化（Plasma Electrolytic Oxidation, PEO）陶瓷涂层**，官方宣传"硬度是钢的四倍"且抗指纹；触控板是经过处理的玻璃面 + 触觉反馈。接口给得很全：2 个 Thunderbolt 4 / USB 4、3 个 USB-A、HDMI、3.5mm 音频、microSD，无线侧 WiFi 6E + 蓝牙 5.3；同时保留了**物理无线开关**和**带可拆磁吸式摄像头**的小巧思（不用时直接取下放进收纳槽）。键盘可定制多种国际布局并支持背光，附带 65W 氮化镓 USB-C 充电器。

软件层面，机器搭载开源的 **coreboot + edk II** 固件，并接入 LVFS 提供 5 年固件更新，是少数把 BIOS 也开源化的笔记本之一。官方没有"指定 Linux 发行版"，意思是任何主流发行版（Ubuntu、Fedora、Arch、NixOS）都可以装，整机享 1 年保修，明文允许拆机和换装第三方系统——这点对 Linux 爱好者很关键。整体定位偏向追求安全、可维修性、生产力的硬核 Linux 用户。

## HN 评论精华

- **lhl**：最高赞评论直接打出"延期"卡——这台机器 2022 年 11 月就发布，当时承诺 3-4 个月发货，结果 3 年半后才出来。建议买家先等独立第三方评测，不要冲首发。

- **katiestarlabs（厂家代表）**：官方下场回应延期问题，归因为"零部件供应问题、作为小厂代工厂排期靠后、固件开发耗时"。她确认评测样机已经发出，并贴出 Reddit 上的几篇用户评测链接。

- **simonjgreen**：从市场时机角度同情厂商——当下内存价格暴涨"对小厂是致命打击"，很多用户都在等内存降价再下单，所有精品 Linux 笔记本厂同时受伤。

- **ulrikrasmussen**：少数真实用户晒单——键盘、触控板、120Hz 屏都很满意，重负载下电池能撑 6-7 小时，整体很值回票价。

- **zamadatix**：从规格细节挑刺——宣传图给人看到的是"插槽内存（socketed）"，但实际是 LPDDR5X 板载（BGA 焊死），不能升级；CPU 选项之间的差价不合理；存储和配件无法 opt-out，捆绑销售。

- **999900000999**：性价比派的反对意见——同价位 Dell XPS 跑 Linux 也很顺，绝大多数 Windows 笔记本装 Linux 没问题，没必要为"Linux 专用"标签付溢价。

- **核心争议**：评论区把这台机器和 Framework 16、ThinkPad P16、System76 Bonobo 反复对比。共识是 StarFighter 在外观工艺、屏幕规格、固件开放度上有亮点，但价格高、可升级性受限、品牌信任度仍待积累。多数人建议"想支持开源固件就买"，否则 Framework 仍是首选。
