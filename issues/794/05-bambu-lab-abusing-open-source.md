---
layout: article
title: "Bambu Lab 正在践踏开源社区的契约"
issue: 794
number: 5
category: favorites
original_url: "https://www.jeffgeerling.com/blog/2026/bambu-lab-abusing-open-source-social-contract/"
hn_url: "https://news.ycombinator.com/item?id=48109224"
date: 2026-05-22
---

## 文章摘要

**Jeff Geerling**（知名 YouTube 硬件博主）去年就公开表态再也不推荐 Bambu Lab 打印机。他自己手里那台 P1S 之所以还能用，是因为他做了几件事：用 OPNsense 防火墙把打印机从公网隔离、停止固件更新、把打印机切到 Developer mode、卸载 Bambu Studio 转用 **OrcaSlicer**。他的话很扎心："我得这么折腾，才能让我自己花钱买的东西真的属于我自己，而不是属于 Bambu。"——这其实是当下消费电子"购买即租用"逻辑的一个浓缩样本。

文章的导火索是 Bambu 的最新一次操作。先理清族谱：**OrcaSlicer** fork 自开源项目 **Bambu Studio**，后者又 fork 自 **PrusaSlicer**，再往上是 **slic3r**——这条 fork 链全部在 **AGPLv3** 之下。Bambu 默认让所有打印任务都过他们的云（也就是说他们能看到你打过的每一个文件），除非用户费心切到 Developer mode + 屏蔽公网。然后社区里出现了一个叫 **OrcaSlicer-bambulab** 的 fork，让用户能绕开 Bambu 云直接驱动打印机的全部功能。**关键点是**：这个 fork 用的是 Bambu Studio 上游 AGPL 代码的**原封不动版本**——也就是 Bambu 自己 Linux 版的代码。

Bambu 的反应是发律师函并发布公开博客，指控这位开发者"**注入伪造身份元数据**冒充官方客户端发起请求"。Geerling 在文章里逐条反驳：第一，这位开发者用的就是 Bambu 自己 AGPL 项目里的代码，**根本谈不上"impersonation"**；第二，"如果你们对 DDoS 的唯一防御就是看 User-Agent 字符串，那这不是开源开发者的问题，是你们自己的安全设计问题"；第三，更讽刺的是 **2022 年 Bambu 自己 fork PrusaSlicer 的时候，曾经让所有 Bambu 用户的遥测数据发到了 Prusa 的服务器上**，而当年 Prusa 并没有反手发律师函。

Geerling 的总结是：Bambu 在**滥用开源社会契约**——一边吃 AGPL 上游的红利，一边对下游小型 fork 挥舞法律大棒"杀鸡儆猴"，处理的对象还是个只有极小用户量、并且曾经热心在 Bambu 官方 GitHub 帮忙解答 Linux/Wayland 问题的志愿者。他的建议很直接：**与其在评论区抱怨，不如下次直接买别的品牌**。Louis Rossmann 已经公开承诺为这位开发者出 10000 美元法律支援金。

## HN 评论精华

1. **hsuduebc2**：把事件政治化解读——"这感觉像是国家层面的压力。否则我看不出他们为什么这么干，**企业间谍/数据收集**反而是最合乎逻辑的解释。"

2. **capitangolo**：用市场用脚投票——"到现在我对 Bambu 的印象主要就是他们对用户的敌对态度……我家里现在的几台打印机，**没有一台是 Bambu 的**了。"

3. **arjie**：反向质疑——一边鼓吹开放系统、一边继续买专有消费设备本身就矛盾，这是一种很常见的"科技博主式伪善"。

4. **CarVac**：扩展案例——Chamberlain（车库门遥控）也干过类似的事，结果催生了社区方案 **RatGDO**；这是整个行业的通病而非 Bambu 独有。

5. **scottbez1**：对比 **Prusa** 的路线——Prusa 在保留商业可持续性的同时坚持开放设计，证明"开放"和"赚钱"不是非此即彼。

6. **imtringued**：拆穿限制性许可的把戏——这种法律威胁**根本拦不住真正想抄袭的人**，只能恶心住"老实用户"，达不到它声称的保护目的。

7. **15155**：法律视角——**AGPL 本身就明确授予了修改和分发的权利**，Bambu 发威胁信本身就在法律上站不住脚。

8. **kn100**：务实派——直接列出 OrcaSlicer 和社区方案路线图，告诉新人怎么完全绕开 Bambu 生态。

9. **NewsClues**：澄清原则——开源不等于免费、不等于禁止商业化，但**要求源码可用**是底线；Bambu 想吃这碗饭就得守这条规矩。

10. **整体共识**：HN 上 Bambu 的口碑这一年下降得很快，"性价比 + 即开即用"的光环正在被"**你不真正拥有这台机器**"的认知抵消；越来越多人把 Prusa、Voron 这类开放阵营重新拉回比较表。
