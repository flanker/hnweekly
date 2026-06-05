---
layout: article
title: "很难说服自己买一台 Framework 12"
issue: 796
number: 41
category: watching
original_url: "https://www.jeffgeerling.com/blog/2026/its-hard-to-justify-framework-12/"
hn_url: "https://news.ycombinator.com/item?id=48323869"
date: 2026-06-05
---

## 文章摘要

Jeff Geerling 的这篇评测以一个真实场景开篇：他刚高中毕业的侄子（也是教子）想要一台笔记本，对学生来说价格（更确切说是"性价比"）是最重要的考量。Geerling 让侄子在两台机器中选：一台是他买来测试的 Framework 12，另一台是几个月前买来在工作室使用的 MacBook Neo。

他用自己的整套基准测试跑过两台机器后，得出一个鲜明的结论：Mac 在大多数情况下更快、更省电、更安静、做工更好、屏幕好得多，而且便宜得多。Framework 则更贵、（多数情况）更慢、更吵（风扇经常拉满）、屏幕相当差；但它是触摸屏、有 360° 转轴，并且更易维修和升级。问题在于：为了一个整体更差的体验，你愿意多付 20–40% 的钱吗？低端 Framework 的 DIY 版起价 749 美元（还得配二手 8GB 内存和 256GB SSD），预装版 799 美元；而对学生，苹果的基础款 Neo 只要 499 美元。文章指出，苹果的 MacBook Neo 颠覆了"性价比笔记本"的格局——苹果本不该既是最便宜又是最超值的选项，但 Neo 恰恰落在了这个位置。

性能方面：Geekbench 6 显示苹果低端 CPU 核心远快于 Intel；Neo 全程静音（无风扇），几乎所有任务能效都接近两倍。Framework 唯一略微领先的是"持续性能"——在 HPL 这类长时间满载的 FP64 负载下，它的风扇让它降频更少，但代价是风扇拉满时在机身附近产生约 40–45 dBa 噪音（工作室本底噪音只有 33 dBa），而且两者差距并不大。GPU 方面 Intel 表现糟糕（GravityMark），但日常 UI 和 4K 视频播放两者无明显差异，只有游戏或 GPU 加速计算才会感觉到。

做工与妥协：Neo 的做工远超其价位（苹果的规模优势）；Framework 为了达到价格和尺寸目标处处妥协——屏幕色彩明显偏差、比 Neo 更厚更重（尽管屏幕更小）、塑料顶盖在平板模式下会蹭到橡胶脚垫导致需要反复清理、扬声器很差。可换的模块化端口是最大亮点：可以装四个"扩展模块"，每个最高 USB 3.2 Gen 2x1；而 Neo 只有两个端口。Framework 12 的困境在于：因为想要 360° 转轴实现平板模式，它在屏幕上做了妥协，本可以靠更好的触控笔功能弥补，却用了较旧的触控笔技术，导致平板/绘图模式不如现代 iPad 或 Surface。最终，侄子选了 MacBook Neo，Geerling 自己则把 Framework 当"工具机"，并坦言试用后发现"笔记本当平板用其实相当笨拙"。

## HN 评论精华

**throwaway2037** 评价这是一篇"残酷但礼貌"的对比（典型的中西部 Geerling 式"用善意杀死对手"），为 Framework 团队感到揪心——任何想在这个市场与 Mac Neo 竞争的团队都会备受打击；但他依然看好 Framework，因为"极客们为它疯狂"。

围绕"可维修/可升级"是否真有意义，评论区展开了激烈讨论：

**SoftTalker** 指出 Framework 的主打卖点就是"易于定制、升级和维修"，而 Mac Neo 想必是用到变电子垃圾为止的封闭设备。**akkartik** 回应说 Neo 其实"还不错"，iFixit 称它是"多年来最易维修的 MacBook"，并认为这是 EU 法规起的作用。但 **AshamedCaptain** 反驳："多年来最易维修"几乎毫无意义，对一个会拿来跟 Framework 比较的人来说简直是侮辱；**makeitdouble** 补充 Neo 在 iFixit 上仍只有 6 分，零件配对、第三方限制依旧存在，而最新 ThinkPad T 系列是 10/10，说明更好的设计完全可能。

**hadlock** 提出一个尖锐观点：Framework 永远是一台"表态设备"（statement device），就像只用来买菜、从不上土路的现代四驱 SUV——笔记本的可升级性绝大多数人永远不会真正用到，"人们买的是这个理念"。但 **SoftTalker**、**ssl-3** 等人现身说法，称自己确实升级过存储、内存、Wi-Fi 模块、换过电池和屏幕。

**helterskelter** 和 **starkparker**、**tvshtr** 则强调"可维修性"（而非"可升级性"）对非技术用户的价值：妻子/伴侣摔坏 XPS 屏幕后，对"半小时就能自己换屏"的 Framework 很感兴趣；还有人因为某台 ASUS 笔记本电池坏了就无法用交流供电、更换又极其繁琐昂贵，转而买了 Framework。多人还指出 Framework 从第一天就完美支持 Linux 是其"隐藏的杀手级特性"。也有人（hadlock）反驳说想要可换屏直接买 ThinkPad 就行。**ssl-3** 还提到一个现实问题：Framework 二手保值率出奇地高，供需偏向卖方，导致很难买到便宜的 Framework。
