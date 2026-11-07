---
layout: article
title: "十年逆向工程，让 DOS 版 Wing Commander 支持合作飞行"
issue: 817
number: 56
category: fun
original_url: "https://fireflyk64.github.io/wcmulti/"
hn_url: "https://news.ycombinator.com/item?id=50369895"
date: 2026-11-06
---

## 文章摘要

作者 danielrh 与兄弟小时候只能共用一架飞船的操作，如今终于能作为僚机一起执行 Wing Commander 剧情任务。项目从 2015 年的 DOSBox 内存实验开始：比较静止与移动时的内存快照，逐渐定位位置、速度和方向向量，再通过写入实验确认含义。数据规模虽小，摄像机副本、定点数表示和临时槽位仍使分析充满陷阱。

仅同步变量很快不够用了，开火、伤害和导弹等行为需要调用原游戏函数。作者们追踪 Borland 覆盖段的跳转入口，用跳板保存寄存器、传入参数并返回原执行位置，后来在主循环的稳定位置处理调用队列。网络另一端相同实体可能处在不同槽位，于是还要维护主机编号到本地编号的映射；任务胜负、爆炸和通信文字也分别经过逆向与同步。

项目 2020 年已能玩，却因需要特殊 DOSBox、游戏文件、交换 IP 与穿越防火墙而难以分享。2026 年借助 WebAssembly、WebRTC 和作者的 lobbylink 房间服务，使用门槛进一步降低，AI 主要参与浏览器移植、联网与剩余问题处理。作者明确说网站不托管 Wing Commander 游戏文件，拥有游戏者需提供自己的安装文件。本次核读文章与作者评论，未运行代码或实际联机验证。

## HN 评论精华

- [daniel_rh](https://news.ycombinator.com/item?id=50370118) 说明玩家需自行拖入所拥有游戏的安装文件，否则页面会使用作者早期的 DOS Vega Strike；这与免费分发商业游戏是两回事。
- [daniel_rh](https://news.ycombinator.com/item?id=50371512) 强调，早期多年数据段分析和原函数调用主要靠手工完成，AI 用于最后阶段的移植与更新，不能把十年探索概括为一键生成。
- [cadr](https://news.ycombinator.com/item?id=50371476) 认为长期反复回到同一个项目的坚持，与最终效果一样令人佩服。
