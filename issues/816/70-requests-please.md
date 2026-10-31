---
layout: article
title: "Requests, Please：把防火墙值班做成游戏"
issue: 816
number: 70
category: fun
original_url: "https://morronel.github.io/requests_please/"
hn_url: "https://news.ycombinator.com/item?id=50241195"
date: 2026-10-30
---

## 文章摘要

Requests, Please 借用 Papers, Please 的检查与裁决玩法，让玩家扮演 Web 应用防火墙：阅读逐条 HTTP 请求，结合端点说明、解码工具和来源信息，决定放行或拦截，并选择原因。游戏把业务增长与安全要求设成相互拉扯的目标，模拟判断中的误报和漏报压力。

根据[源码 README](https://github.com/morronel/requests_please)，它包含多种攻击类别、看似可疑但合法的请求、逐步增加的规则以及修复方案选择。实现为无需后端的 PWA，游戏逻辑与 Preact 界面分离，支持用固定随机种子生成请求。这里没有实际试玩，功能说明来自项目仓库。

这是一种安全概念练习，不是生产防护指南。识别可疑字符串并不等于修复漏洞；真正的应用仍需要正确解析数据、按上下文编码输出，以及使用合适的数据访问接口。评论区也特别提醒不要把游戏中的拦截规则照搬成唯一防线。

## HN 评论精华

- **题材契合玩法。** nticompass 很快把它概括成“扮演防火墙的 Papers, Please”，welldoneator 等人表示玩得开心。
- **误导性的安全直觉需要纠正。** athrowaway3z 指出，合法内容也可能包含看起来像攻击的文本，安全不能只靠拒绝这些字符串；核心在于不把数据错误地解释成代码。
- **反馈应帮助学习。** deadbabe 希望漏掉的攻击继续留在屏幕上，便于理解错误，而不是立即跳到下一条。
- **交互与协议语义。** justusthane 建议跨设备继续进度及更顺手的快捷键；cryptonector 认为一个格式错误的请求应对应 400 响应。这些反馈显示游戏的趣味性与技术准确性需要分别打磨。
