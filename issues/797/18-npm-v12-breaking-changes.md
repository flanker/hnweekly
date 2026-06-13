---
layout: article
title: "npm v12 即将到来的破坏性变更"
issue: 797
number: 18
category: code
original_url: "https://github.blog/changelog/2026-06-09-upcoming-breaking-changes-for-npm-v12/"
hn_url: "https://news.ycombinator.com/item?id=48467705"
date: 2026-06-12
---

## 文章摘要

GitHub 在变更日志中预告了 npm v12（预计 2026 年 7 月发布）将引入的一系列破坏性变更，核心是一组以安全为导向的默认值调整，目标是遏制日益猖獗的供应链攻击。

主要变化包括：

- **默认关闭安装脚本（`allowScripts` 默认 off）**：`preinstall`、`install`、`postinstall` 等脚本不再自动执行，同时也会阻止隐式的 `node-gyp` 构建，以及来自 git/file/link 依赖的 `prepare` 脚本。用户需通过 `npm approve-scripts` 显式批准受信任的包。
- **Git 依赖默认不解析（`--allow-git` 默认 `none`）**：无论直接还是传递依赖，引入 Git 依赖都需显式加 `--allow-git`。此举堵住了一个 `.npmrc` 文件可覆盖 Git 可执行文件、进而导致代码执行的漏洞。该能力自 npm 11.10.0 起可用。
- **远程 URL 依赖默认不允许（`--allow-remote` 默认 `none`）**：HTTPS tarball 和远程 URL 依赖需显式加 `--allow-remote`，自 npm 11.15.0 起可用。本地文件与目录依赖不受影响。

迁移建议：尽快升级到 npm 11.16.0 或更高版本，在常规安装时审阅警告，运行 `npm approve-scripts --allow-scripts-pending` 审计含脚本的包，批准受信任的包并将更新后的 `package.json` 提交入库，从而平滑过渡到 v12。

## HN 评论精华

**bastawhiz**（477+ 赞）：调侃自己居然没注意到 npm 已被 GitHub 收购，"一下子很多事情都说得通了"，对微软/GitHub 主导下 npm 的种种问题表示担忧。

**atraac**：postinstall 脚本早就该被移除，"它是 npm 包生态的毒瘤"，认为安装脚本是一个本该多年前就消除的重大安全隐患。

**VMG**：质疑禁用 postinstall 脚本是否真能提升安全性，因为代码迟早还是会运行，认为这一改动更像是把问题往后拖延而非根治。

**hinkley**：认为 npm 在微软介入之前就已经"一团糟"，微软只是让情况雪上加霜，这也解释了 Yarn、pnpm 等替代品为何兴起。

**efortis**：指出这次发布修复的是一个"十年前就被报告的漏洞"，凸显了 npm 在已知安全风险上长达十年的拖延。
