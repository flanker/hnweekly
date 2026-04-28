---
layout: article
title: "Tiao：双人回合制策略棋盘游戏，跳棋遇上围棋"
issue: 791
number: 52
category: fun
original_url: "https://playtiao.com"
hn_url: "https://news.ycombinator.com/item?id=47914369"
date: 2026-04-27
---

## 文章摘要

Tiao 是一款双人回合制策略棋盘游戏，由开发者 trebeljahr 把朋友 Andreas Edmeier 多年打磨的实体桌游规则数字化后开源上线。规则上它的定位作者形容为"跳棋（Checkers）遇上围棋（Go）"——既有跳棋那种通过跳跃吃子的清晰即时反馈，又像围棋那样追求形状、轮次节奏与全局压制，关键策略是把对手逼入类似国际象棋术语 Zugzwang（无步可走的死局）那种被迫劣手的局面。

技术栈相当现代：核心代码两周内用 TypeScript + Next.js + Express + WebSocket + MongoDB 写成，全 docker 化部署在 Hetzner VPS 上配合 Coolify。多人实时对战、ELO 匹配、成就系统、OpenPanel 数据分析、better-auth 鉴权，外加 AI 对手与"线下双人"模式（OTB），整个项目在 GitHub 上以 AGPL 开源（github.com/trebeljahr/tiao）。游戏完全免费、浏览器即玩、无需安装，桌面与移动端都能跑。

围棋玩家试玩后给出了非常有趣的观察：在围棋里那些经典"好型"（眼位、连接）放到 Tiao 里几乎都是"最差形状"——这种逆围棋直觉的设计正是 Tiao 区别于其他抽象策略游戏的趣味所在。多名 HN 评论者（包括围棋玩家与桌游爱好者）反馈即便玩"简单"AI 也会被反复教做人，体现出 Tiao 的策略深度。

作者的初衷不是商业化，而是希望让 Tiao 这套朋友十年间反复打磨的规则被更多人玩到，"哪怕只是哪天有人用普通围棋盘 19×19 自带玩 Tiao"也好。教程被多人盛赞做得极其清晰，是"我玩过最容易上手的桌游介绍之一"。代码层面，作者透露多人状态同步是采用"传输 move 事件、客户端各自重放"的轻量方式而不是每回合同步完整棋盘，这也是大多数回合制网游的标准做法。

## HN 评论精华

- **Rendello（同道中人）**：自己也尝试过实现古老的 Konane 和现代的 Shōbu 这两款抽象策略游戏，过程中学会了 Erlang 的 property-based testing。鼓励作者去 BoardGameGeek 上传一个介绍视频，能极大扩展受众。

- **simplify（围棋玩家视角）**：玩 Go 多年，对 Tiao 最大的震撼是"围棋里所有好型在 Tiao 里全是最差型"，让人非常错乱。作者表示同感笑死。

- **WillMorr**：高度赞扬教程做得"几乎是我体验过最容易上手的游戏入门"。作者透露自己花了大量精力打磨教程，但承认还有粗糙边缘——人们带进游戏的预设非常多元，要在不让新手不知所措的前提下覆盖所有情况非常难。

- **rytill**：建议先期减少时间控制队列数量。多个 queue 会让初期匹配池被进一步打散，玩家很难找到对手；先期只放一种（比如 10 分钟）能更容易凑出实时对局。作者已采纳。

- **gammalost**：调侃"现在我反而对一个不是回合制的桌游感兴趣了"。oniony 顺势列出了一批非回合制桌游推荐：Galaxy Trucker、Captain Sonar、Sidereal Confluence、Kitchen Rush、Magic Maze 等。

- **mylifeandtimes**：建议加一个"学习模式"，允许在 AI 跳吃 5 子之后悔棋；并希望非法落子位置在悬停时就用更柔和的颜色高亮，避免点下去才发现违规。gurjeet 回复说"Undo move"按钮已经能做这件事，建议是把法/非法落子区分做得更明显。

- **smlavine**：在 9×9 棋盘上 simple AI 试了 10 局才赢一次，觉得想稳定取胜必须把对手逼入 Zugzwang，这种"压制选择空间"的取胜哲学是 Tiao 的核心乐趣。
