---
layout: article
title: "一个人花三年，把《沙丘 II》塞进了 PICO-8"
issue: 794
number: 59
category: fun
original_url: "https://liquidream.itch.io/undune2"
hn_url: "https://news.ycombinator.com/item?id=48107404"
date: 2026-05-22
---

## 文章摘要

**UnDUNE II** 是开发者 **Paul Nicholas（Liquidream）**把 1992 年的经典即时战略游戏 **Dune II: The Building of a Dynasty** **demake（降级重制）到 PICO-8 幻想机**上的作品——业余时间断断续续做了**约三年**。Dune II 是 RTS 这个品类的**奠基之作**——基地建设、采矿、单位生产、迷雾、AI 对手等一套范式由它定型，后来的 C&C、Warcraft、StarCraft 全部站在它的肩膀上。

**PICO-8** 是一个**故意限制极严的"幻想主机"**：128×128 像素、16 色调色板、最多 8KB ROM 代码、4 个声道。把这么大的 RTS 塞进去需要把游戏拆成 **12 张 PICO-8 卡带**串起来——但功能并不缩水，Liquidream 实现了**全部 3 个家族（Harkonnen / Ordos / Atreides）+ Sardaukar 萨多卡精锐**、**19 种建筑、21 种单位**、皇宫超级武器、迷雾、沙虫、9 关战役任务。控制比原版**更顺**——可以直接点击敌人发起攻击，不再需要先点"Attack"按钮。

游戏跨平台发布：HTML5 网页版可以直接玩、Windows/macOS/Linux 原生包、以及作为 PICO-8 卡带在真实 PICO-8 硬件上跑。**音乐和音效是重新做的 PICO-8 chiptune 版**，但听起来仍然能立刻让"frigate has arrived"在脑子里复活。这是一个**致敬式的爱好者作品**，免费/付费随意，给整代玩过 Dune II 的人一次"再玩一遍但用 5 分钟而不是 5 小时"的机会。

## HN 评论精华

- **butler14**：第一时间被勾起回忆——"Classic 这个词都不足以形容"。他特别提到原版**每点一个单位就报告一次"Reporting!"**、移动时"Acknowledged!"，重复到极致但**就是有味道**。挂在底下：**jnovek**："'Frigate has arrived' 永远烙在我脑子里，从没在现实中遇到过同样怀旧的人——还好 HN 替我兜底了。"

- **tosti**：硬核派——从 GPLv2 项目 **adplug** 找出能播放 Dune II 原版 `.adl` 音乐文件的工具：`adplay -s 6` 直接听到"normal/peaceful music"。一长串子串展开 Frank Klepacki（Dune II 作曲）vs. Stephane Picq（Dune I 作曲）的考据。

- **larodi**（25 年 DJ + 20 年开发经验）：**长篇盛赞 Stephane Picq** 的 Dune I 原声——认为它是**90 年代电子游戏配乐的真正突破**，复杂的旋律进行 + 工程上的极限榨干（MOD 格式声道数有限）+ 自合成的硬件音色，"90 年代的 eurotrance 拼不过它"。

- **madmoose**：**逆向工程级硬核**——最近他亲手反向了 Dune（不是 II）的 AdLib/SoundBlaster Pro 音乐驱动，用 nukeykt 的 Nuked-OPL3 重新渲染了 "Morning" 一曲。贴了 YouTube 链接，作为"对早期 PC 音频工程的现代致敬"。

- **busfahrer**：上下文科普——这是 **PICO-8 fantasy console** 上的作品，硬件限制极严（128×128/16 色/8KB 代码），"光是开场的 3D logo 就已经令人震惊，更别说整个游戏"。链接 lexaloffle.com/pico-8.php 给不熟悉的人。

- **neals**：天真发问——"什么是 demake？" 评论区集体科普——**skywal_l**：remake 是用更强平台重写以提升画质/音效；demake 反其道而行，用更弱平台重写。**bytecauldron**：经典 demake 是把 3D 游戏拍扁成 2D，挑战在于在严苛硬件限制下重建。

- **burnt-resistor**：上报一个**重现的原版 bug**——"在 windtrap（风力发电）的一部分上叠盖 refinery（精炼厂）建筑也能成功"。**Liquidream（作者）** 当场回复："哎呀，从没听说过这个 bug，谢谢提醒。"——demake 居然忠实复刻到了 bug 级别。

- **mano78**：另一个老梗——"我都忘了**不放在水泥地基上也能建建筑**"。**tuzemec / neals / b3lvedere** 接力补完：可以建，但**血量减半**且**掉血更快需要频繁修**；当年用这招在敌方家门口快速 rush 炮塔，是一种 cheese 战术。

- **dasKrokodil**：考据小 nitpick——"我记得原版**沙虫不吃步兵，只吃载具**"。这种细微差别会被 Dune II 死忠玩家立刻识破。

- **Liquidream（作者收尾）**：感谢 @tosh 帮发帖——"Itch 关注突然涨了，我才知道发生了什么。很高兴大家喜欢这个'小'热情项目，给大家一个回忆童年（至少是我童年）的机会。"
