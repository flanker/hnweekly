---
layout: article
title: "Conductor 加入 Meta，云端用户面临迁移期限"
issue: 817
number: 54
category: startup_news
original_url: "https://www.conductor.build/announcement"
hn_url: "https://news.ycombinator.com/item?id=50369503"
date: 2026-11-06
---

## 文章摘要

Conductor 宣布团队加入 Meta，并计划在 2027 年初推出新的界面产品。公告回顾其 2025 年 7 月上线以来，多编码代理协作从新鲜尝试变成常见工作方式。团队表达对后续产品的期待，但没有公布足以评估新产品的完整规格或承诺无缝兼容，因此现有用户更需要关注当前服务的去留。

本次直接读取公告 HTML 中的时间表与 FAQ，补足正文抽取遗漏的信息：11 月 4 日起停止新用户注册和应用下载；Conductor Cloud 在 11 月 13 日进入只读，11 月 20 日完全关闭并开始删除 Cloud 用户数据，公告要求在此日期前导出。只读阶段可以查看旧对话并导出，但不能继续发送新消息。公告称已提供把云工作区转移到本地的方式。迁移不只是保留代码文件，也需要确认工作区、聊天记录与日常依赖是否仍可用。

本地 Conductor 暂时继续运行，并获得新模型与关键错误修复，直至 2027 年初逐步结束。FAQ 还表示，应用关闭时会删除 Conductor 用户数据；这仍是公司的未来承诺，不是已经完成删除的证据。对使用者而言，这次变化凸显开发工具的持续性风险：熟悉的多代理界面即使很好用，其云服务、商业归属和退出路径仍会影响长期投入。

## HN 评论精华

- [ayayronf](https://news.ycombinator.com/item?id=50369704) 祝贺团队，同时认为 Cloud 的迁移时间很紧，希望了解替代方案。
- [phildenhoff](https://news.ycombinator.com/item?id=50369888) 肯定产品体验，却遗憾结束方式，并希望能开放桌面版源码以便继续使用。
- [mksadoughi](https://news.ycombinator.com/item?id=50373638) 分享自己的替代做法：Claude Code 会话各用一个 Git worktree，再由协调会话分派任务。这只是个人工作流，不能视为与云端隔离环境等价的直接替代品。
