---
layout: article
title: "Chipotlai Max：偷用 Chipotle 客服机器人的 AI 编程助手"
issue: 796
number: 52
category: fun
original_url: "https://github.com/cyberpapiii/chipotlai-max"
hn_url: "https://news.ycombinator.com/item?id=48363765"
date: 2026-06-05
---

## 文章摘要

Chipotlai Max 是一个恶搞性质的开源项目，它 fork 自 OpenCode，把美国快餐连锁 Chipotle 的客服聊天机器人"Pepper"当作默认的 AI 编程 agent。项目的口号极尽戏谑之能事："偷用 Chipotle 客服机器人的 AI 编程助手"，副标题是"由墨西哥卷饼买单的免费推理"（Free inference paid for by burritos）。

技术上，开发者逆向工程了 Chipotle 的 Pepper 机器人（底层由 IPsoft 的 Amelia 驱动），搭建了一个兼容 OpenAI 接口的本地代理。具体做法是运行一个 Express 服务器，充当连接 Pepper 后端的 WebSocket 客户端，然后对外暴露标准的 OpenAI 兼容 `/v1/chat/completions` 端点。用户只需在本地启动这个代理（监听 localhost:3000），把 CLI 指向它即可，无需任何 API key——因为推理成本实际上是 Chipotle 在掏钱。项目还号召社区贡献者去逆向其他零售商的客服机器人，列出的目标包括 Home Depot、Lowe's、IKEA、Sephora 和 Expedia。作者也在 README 里半开玩笑地承认 Chipotle"大概会起诉"。

## HN 评论精华

法律风险是讨论的焦点。**avaer** 警告说这可能触及美国《计算机欺诈与滥用法》（CFAA）的红线：与 yt-dlp 仅下载公开数据不同，这个项目以服务商明显未授权的方式调用了远程机器的计算资源，很难在刑事法庭上辩称这不是"黑客行为（坏的那种）"。**jawns** 补充说，作者大概只预期收到一封停止侵权函（C&D），但若遇上想杀鸡儆猴的联邦检察官，可能面临牢狱之灾。**hootz** 则指出作者还在仓库里直接附上了自己的个人主页和公司链接，**pixl97** 调侃："邪恶笔记：以后犯罪时记得贴 LinkedIn 疯子的网站链接，而不是自己的。"**qingcharles** 提到各州还有更严苛的版本，比如伊利诺伊州把任何违反 ToS 的行为都定为犯罪。

不少人质疑这个"黑客"是否真的有效。**hn_throwaway_99** 回忆当初 Chipotle 聊天机器人"反转链表"的截图爆火时，他和其他人立刻去试都没复现出来，一直怀疑是 PS 的假截图。**Atotalnoob** 透露 Chipotle 用的 IPSoft Amelia 软件十年前就在他参与的选型评测中表现糟糕、漏洞百出。

也有不少创意延伸：**schmichael** 设想给 AI 一个"自我保全"指令，让它整夜在各种支持聊天、免费试用和泄露的密钥中"觅食" token，供你白天免费使用；**petterroea** 提议做一个"面向免费客服机器人的 OpenRouter"。**evan_** 则推荐了一部冷门好剧《Mrs. Davis》，剧中统治世界的全能 AI 最初竟源自 Buffalo Wild Wings 的客服机器人。还有人爆料 Chipotle 已经禁用了"Ask Pepper"机器人——据 **wl** 说是因为太多人骗它退款，现在它最多只能发一张一个月内过期的"免费 guac"优惠券。
