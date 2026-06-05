---
layout: article
title: "VoidZero 加入 Cloudflare"
issue: 796
number: 51
category: startup_news
original_url: "https://blog.cloudflare.com/voidzero-joins-cloudflare/"
hn_url: "https://news.ycombinator.com/item?id=48398055"
date: 2026-06-05
---

## 文章摘要

Cloudflare 宣布收购 VoidZero——这家由 Vue.js 创始人尤雨溪（Evan You）领导的公司，专注于打造现代 Web 生态的高性能 JavaScript 工具链。VoidZero 旗下汇集了五个重量级项目：极速构建工具与开发服务器 Vite、单元测试框架 Vitest、JavaScript 打包器 Rolldown、高性能解析器与 linter 生态 Oxc，以及把这些工具统一进单一 CLI 的集成工具链 Vite+。

Cloudflare 给出的收购理由是投资于基础性的开源设施。博客写道："Vite 是整个 JavaScript 生态少数能达成共识的基础工具之一。"通过将 VoidZero 收入麾下，Cloudflare 获得了推进这些关键工具的资源，同时也服务于其开发者平台战略，尤其面向 AI 生成应用和全栈开发。

文章着重强调了开源承诺：Vite 仍保持 MIT 许可证、开源、厂商中立，用这些工具构建的应用可以运行在任何地方而不局限于 Cloudflare；此外 Cloudflare 还承诺向 Vite 生态基金注资 100 万美元，用于支持维护者和贡献者。战略目标包括：将 Vite 演进为带有厂商无关原语的全栈框架、把 Cloudflare 的 CLI 体验统一到 Vite 工作流之上、为 AI agent 开发循环优化工具，以及最终开源 Void 部署平台。尤雨溪将继续主导 Vite 及相关项目的技术方向。

## HN 评论精华

不少人对开源组织被收购感到担忧。**karpetrosyan** 说"看到开源组织被收购总是令人害怕"，引出了关于开源可持续性的核心辩论。**pjmlp**、**rvz** 等人认为，正是因为没人愿意为这些工具付费，公司才会接管它们、团队才会失去独立性——"开源单凭自身是不可持续的，尤其在 AI 时代"。**bakugo** 则提出不同看法：即使开发者付费，像 Cloudflare 这样的千亿美元公司也总能出更高的价，这与可持续性无关，而是套现一笔巨额报酬已成为当下一切事物的终极目标。

**LoganDark** 引用了官方"Vite、Vitest、Rolldown、Oxc、Vite+ 将保持开源、厂商中立、社区驱动"的承诺，但悲观地表示，"鉴于近来每一桩此类收购的下场"，他倒要看看这些产品多快会因团队转向 Cloudflare 的项目而被弃置。维护者 **TheAlexLichter** 回应说 Vite 是多方利益相关者团队，不会轻易出现这种情况。

**embedding-shape** 注意到博客中"foundation（基金会/基础）"一词出现了 10 次，本以为会宣布把 Vite 捐给 Linux 基金会之类的独立机构，结果并未提及，颇感失望。**pier25** 点出 Vue 生态的微妙处境：Nuxt 团队和 vue-router、pinia 作者 Eduardo 在 Vercel，而尤雨溪现在去了 Cloudflare。关于替代品，有人提到 esbuild、Jest、Web Components；**bakugo** 调侃"还要多久 Vite 会被 vibe coding 的 Rust 移植替代"，随即被纠正——Vite 8.0 早已由庞大的 Rust 代码库驱动。围绕 Cloudflare 是促进还是破坏互联网去中心化，**gonzalohm** 与 **ocdtrekkie**、**hombre_fatal** 等展开了长篇争论，涉及西班牙足球联盟封禁 Cloudflare IP 导致大量合法网站在比赛期间无法访问等案例。
