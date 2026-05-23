---
layout: article
title: "25 周年，他用 vibe coding 复活了 Scorched Earth"
issue: 794
number: 60
category: fun
original_url: "http://www.scorch2000.com/web/"
hn_url: "https://news.ycombinator.com/item?id=48129694"
date: 2026-05-22
---

## 文章摘要

**Scorched Earth** 是 Wendell Hicken 在 **1991 年的 DOS 共享软件**——一个回合制坦克炮战游戏，被誉为"**所有游戏之母（The Mother of All Games）**"。两台坦克在随机生成的山地地形上互射，每发命中都会**炸出可形变的坑**，可以买上百种花式武器：核弹、激光、滚滚岩、岩浆雨、传送、护盾……一个庞大、混乱、上瘾的炮战宇宙，定义了一整个**"artillery game"**品类。

20 多年前作者 **meshko** 出于"想跨网络多人对战"的执念，做了一个 **Java applet 版的网页移植**，挂在 **scorch2000.com** 域名上——美国一票高中机房青少年在那里度过了无数节计算机课。后来 Java applet 在浏览器里被一脚踢死，整件事就此沉睡。

**这次的新闻**是：作者赶在大致 25 周年的节点，**vibecoded**（指完全或主要用 LLM 辅助写代码）把整个 Scorched Earth 重新移植到了 **纯 JavaScript** 上，又活过来了。技术上是浏览器端跑，**支持多人对战**——可以拉朋友进同一局打。HN 上**所有 80/90 后程序员**集体陷入回忆杀，并且开始考据这个品类的家谱：Scorched Earth 之前还有 Kenny Morse 1990 年的 **Tank Wars (BOMB.EXE)**，更早还有 Amiga 上的 Scorched Tanks 和 Atari 2600 的 Artillery Duel，再往前是 GORILLA.BAS。后世的 **Worms、Pocket Tanks、Hedgewars** 全是这个家族的延伸。

## HN 评论精华

- **meshko（作者）**：自荐说明——"为了 25 周年（大致），我**vibecoded** 了一个一直想做的项目：把原始 remake 移植到 JavaScript，又活了。" 一句话引出了所有人的回忆。

- **felooboolooomba**：报第一个**真用户 bug**——"很多人没意识到要按 Start 按钮"。**meshko** 苦笑回复："是啊这就是让 LLM 设计 UI 的下场，整个 flow 要重做。"——HN 上少有的 vibe coding 自嘲。

- **kylemaxwell + api + el_duderino + walrus01**：集体怀旧——1992 年高中机房、286 PC、Borland C++ for DOS 课、Roller、Lava……**api** 一句话总结整个游戏的气质——"作者像是写了一个简单的坦克战，然后**把他能编出的所有怪异效果都塞了进去**当武器。"

- **alterom**：考据派定锚——**Tank Wars by Kenny Morse (1990)** 是 Scorched Earth 的爹，archive.org 有原版。"有 AI 玩家 + 疯狂的武器商店，这才是**真正的炮战游戏之母**"。其他人在底下补 **Tanx、Ballerburg (Atari ST 1987)、GORILLA.BAS** 等更早的家族成员。

- **skeeterbug**：千禧年初**机房日常**复刻——他高中机房上玩的是 Java applet 版（2000/2001），后来 Java 死了游戏跟着死了。回头去试新版，遇到一个有意思的 bug："最后只剩两台坦克，但能量上限被卡在 235，谁都打不到谁"——**meshko** 一句话回："你可能 stuck 了，从菜单点 'mass kill' 重开就行。"——一个 25 年的老 feature 立刻被记起来。

- **vunderba**：贴上 web.archive.org 上 2014 年版的 scorch2000.com 截图，惊叹"网站长得和我十多年前看到的几乎一模一样"，问"你是当年那个 Java 版的作者吗？" meshko 回："是啊，域名一直在自动续费 :)"。

- **ChrisArchitect**：链接到 Hicken 本人的原始网站 whicken.com/scorch（《Scorched Earth: The Mother of All Games》）。**meshko** 公允评价："那个比这个 remake 更好，但**没有多人**。" 之后被 **krupan** 调侃："它有多人啊——只要你们坐在同一台电脑前 :)"。

- **rickcarlino**：突然意识到 **Pocket Tanks 原来是衍生作**。下面挂着 **LocalH** 补充：Amiga 上的 **Scorched Tanks** 才是 Pocket Tanks 作者的更直接前身；**Sharlin** 把这一切收编成一个名为 **artillery game** 的完整品类，附 Wikipedia 链接；**prmoustache** 安利开源的 Worms 替身 **Hedgewars**。

- **rhema**（**全帖最佳故事**）：他 9 岁时通过 Scorched Earth **第一次"hacking"**——shareware 版禁止真人选 ultra tank（可一次发 3 发的强力坦克），但 AI 玩家可以用。他的 hack：**开一局正常坦克 vs. ultra computer，存档，用 ASCII 编辑器打开，翻转 player 字段，读档**——拿到了 ultra tank。后世 **stackghost** 接力分享他在 C&C 原版里用 INI 文件给 Nod 小车装上方尖塔激光的童年。
