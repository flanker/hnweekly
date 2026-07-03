---
layout: article
title: "Xteink X4：能贴在手机背后的口袋墨水屏阅读器"
issue: 800
number: 17
category: show_hn
original_url: "https://blog.omgmog.net/post/xteink-x4-e-ink-reader/"
hn_url: "https://news.ycombinator.com/item?id=48662381"
date: 2026-07-03
---

## 文章摘要

Xteink X4 是一款廉价（约 40 英镑）、口袋大小的墨水屏（e-ink）阅读设备，尺寸仅 114 x 69 x 5.9mm，重 77g。它配备 4.3 英寸、220 PPI 的显示屏，核心竟然只是一颗 ESP32 芯片，内置 16GB 可扩展存储。屏幕清晰、无残影、翻页即时，支持 Wi-Fi 和蓝牙，USB-C 充电，续航可达 14 天，还附带品牌 microSD 卡和 MagSafe 磁吸配件。

作者 Max Glenister 认为它最大的优点是便携性——"能真正塞进裤兜里并消失不见"。原厂固件功能可用但过于基础，好在活跃的第三方固件生态把它变成了一台称职的阅读器。缺点方面：原厂固件深度不足、microSD 卡槽不好插拔、MagSafe 手机磁吸被过度宣传（对已有手机配件的用户并不实用）。

在自定义固件上，作者最终选择了 Inx 固件，它提供标签页导航、按书设置、阅读统计、KOReader 集成和高亮功能；其他值得一提的还有偏排版的 Papyrix 和面向写作、支持蓝牙键盘的 MicroSlate。总体结论：以这个价格，X4 配合社区固件带来了超出预期的阅读体验。

## HN 评论精华

这条帖子几乎变成了 X4 车主的"真香"大会，讨论集中在使用体验、固件生态和产品哲学上。

- 核心卖点是"随身"：大量用户（**square_usual**、**miloignis**、**crimsdings**、**fernandotakai** 等）反复强调它贴在手机背后或塞进口袋"随时可读"，成了对抗刷手机（doom-scrolling）的利器。**kstrauser** 的总结广受认同："它不是我拥有的最好的阅读器，但它是我随身携带时手头最好的阅读器。"**mmstghjx** 则说想刷手机时就掏出 X4 读一章，"把被手机偷走的时间夺回来"。

- 固件生态是灵魂：几乎人人都在推荐刷第三方固件 **Crosspoint**（及其分支 CrossInk、cpr-vCodex 等）。**sieve** 说刷上 Crosspoint 后"完美运行"，通过 Wi-Fi HTTP 服务器传书极其方便，证明了"一颗微控制器足以做出一台真正的电子书阅读器"。即便是被锁定 USB 刷机的中国版，也能通过 SD 卡放固件 + UP+POWER 重启的方式轻松刷入。

- X3 vs X4 之争：**timw4mail**（两台都有）总结 X3 文字更锐利、更白、续航略好；X4 则屏幕更大、用标准 USB-C 充电（X3 是特制磁吸/pogo pin）、固件支持略好。多数人因 USB-C 通用性推荐 X4。

- 已知缺陷：**ludvigcosma**、**jdhawk** 等指出在强烈阳光直射下屏幕会褪色（据说是屏幕排线缺 UV 滤光所致），Crosspoint 已加入"Sunlight Fading Fix"软件缓解；**emme**、**senorcrab** 警告屏幕非常脆弱，容易在背包里被压碎。排版引擎（尤其原厂）能力有限，**criddell** 吐槽出现"单词粘连、无连字符"的糟糕排版。

- 关于未来与产品哲学：多人提到即将推出带前光（frontlight，**Kaibeezy** 纠正墨水屏只能前光不能背光）和触摸屏的 X4 Pro，以及基于安卓的 S4。但社区普遍希望保留极简本色——**0cf8612b2e1e**、**c45y**、**sieve** 都认为加触摸屏是"偏离最小化阅读器的初心"，只想要加个前光。**functionmouse** 则悲观地说这类中国廉价设备"好是好在偶然，厂商并不真懂，每次改动只会更糟"。多数人反倒庆幸厂商没锁死设备，倒闭后也能靠社区固件续命。
