---
layout: article
title: "Postman 自动安装编码助手插件引发授权争议"
issue: 817
number: 49
category: startup_news
original_url: "https://github.com/postmanlabs/postman-app-support/issues/14370"
hn_url: "https://news.ycombinator.com/item?id=50354960"
date: 2026-11-06
---

## 文章摘要

GitHub issue #14370 的报告者 indicava 称，他在 Mac 上打开 Postman 后，发现 Codex 与 Claude Code 的全局配置被加入 Postman 插件，出现 MCP 认证警告，API 工作也反复被引导使用 Postman 技能。报告列出桌面版本、安装日志、固定源码修订和配置变化，核心质疑是一个 API 客户端是否可以未经明确选择，就修改其他编码工具的行为。

尤其值得注意的是启动钩子：报告所引源码会向助手注入覆盖广泛 API 工程任务的指令，安装器还包含尝试批准自身钩子的逻辑。报告者谨慎区分了「源码存在批准尝试」和「某条本地信任记录究竟由谁创建」，并明确写明，现有材料证明的是不期望的集成与指令变化，**并不证明数据已经外泄**。因此，HN 标题中关于共享全部 API 数据的说法不应直接升级为确定事实；本次也未在干净系统复现。

原 issue 已包含后续回应：11 月 5 日，仓库成员 preethammavin 表示原意是简化集成，但承认提示未传达足够控制权，宣布回退变更并重新设计体验。随后 issue 关闭并限制讨论。回退承诺不等于已验证所有受影响电脑的配置都被清理，相关恢复范围仍值得用户和企业管理员核对。

## HN 评论精华

- [georgel](https://news.ycombinator.com/item?id=50355200) 抱怨 Postman 从轻量工具变成过度推销的复杂产品。这是个人产品体验，不能代替安全调查。
- [debo_](https://news.ycombinator.com/item?id=50355274) 推荐 Bruno，重视本地使用和不强迫注册；其他回复提醒更换工具时也要检查账户与云端依赖。
- [gschier](https://news.ycombinator.com/item?id=50355620) 以 Yaak 作者身份解释其商业使用许可，强调商业预构建版本和自行编译源码的条件不同。讨论由单次安装争议扩展到开发工具如何靠收费维持服务而不破坏信任。
