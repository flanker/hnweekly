---
layout: article
title: "我们让 Playwright 快了 2 倍、token 效率提升 80%"
issue: 811
number: 28
category: code
original_url: "https://github.com/browserbase/stagehand"
hn_url: "https://news.ycombinator.com/item?id=49756671"
date: 2026-09-25
---

## 文章摘要

这是 Browserbase 团队为其开源浏览器自动化 SDK **Stagehand v4** 发的 Show HN 性质帖子，链接指向 GitHub 仓库。Stagehand 给自己的定位是"浏览器 agent 的 SDK"，口号是"像人一样使用浏览器的 AI"：登录一次、保持会话，再从另一端拿出结构化数据。项目采用 MIT 许可，Copyright 属于 Browserbase, Inc.。

**发帖人的自述交代了 v4 的动机。** 发帖人 wittydeveloper 说，Stagehand 两年前发布，目前有 2.4 万星、每月 400 万次 npm 下载，最近修复了它最大的缺陷：往返延迟。每执行一个动作，脚本和浏览器之间都要往返一次——本地运行时很短，放到云端就明显变长。他们还看到很多帖子抱怨 Playwright MCP 胃口太大、吃 token 太凶。于是团队从头重建了 Stagehand，发布 v4：**Stagehand 改为从一个浏览器扩展里控制浏览器**，这个扩展在浏览器启动时自动加载。v4 支持批量命令、提供专门的省 token 方法 `act()` 和 `extract()`，新架构"比 Playwright 快 2 倍、token 效率高 80%"。团队还公开了一套评测，在十几个模型（前沿模型和开放权重模型）与工具（Codex、Claude Code 等）上做对比。

**README 展示的核心 API。** 官方示例是一个典型的登录并抓取账单的流程，同时提供 TypeScript、Python、Go 三种 SDK：

- 用 `localBrowser.launch({ userDataDir })` 启动本地 Chrome，Cookie 持久化到本地目录，下次运行时直接处于登录状态；
- `observe("find the email input")` 返回真实的选择器，然后用 Playwright 风格的 `page.locator(...).fill()` 填入账号密码——**这样凭据永远不会发给模型**；
- `act("click the sign in button")`、`act("open the billing page")` 用自然语言执行操作，号称在网站改版表单时能"自愈"；
- `extract("extract every invoice in the table", schema)` 配合 zod（Python 用 pydantic、Go 用结构体）返回经过 schema 校验的结构化数据。

**"为什么选 Stagehand"的六条卖点**：熟悉的 Playwright 风格 API（`goto`、`click`、`locator`、`screenshot`）；混合式可访问性树裁剪，让 agent 只拿到恰好需要的页面上下文；作为扩展运行在浏览器旁边，削减每个动作的往返延迟；`act`/`observe`/`extract` 在网站底层变化时自动更新操作方式；面向 agent 的功能，包括 WebMCP、剪贴板、批量命令、穿透嵌套 iframe 和封闭 Shadow DOM 的深度定位器、OTel 追踪；以及三种语言共用一个完整的浏览器驱动。

**与 Browserbase 云服务的结合。** 同一个脚本指向 Browserbase 云浏览器，号称比同类 Playwright 云浏览器快 2 倍；可以配置 Model Gateway，由它为每个动作挑选最便宜的模型，无需自己接入模型提供商；开启服务端缓存后，相同的调用直接从 Browserbase 返回、不花 token。云端还附带验证模式、住宅代理、持久化上下文和会话录像。此外，托管的 Browserbase MCP 服务器能把 `navigate`、`act`、`observe`、`extract` 直接交给任何 MCP 客户端（包括 Claude Code、Cursor、Codex），不用安装、也不需要本地浏览器；还有无需浏览器的 Fetch（把任意 URL 抓成 Markdown）和 Search（省 token 的网页搜索）两个附加功能。README 提供了从 Playwright 迁移的指南，并列出与 CrewAI、Mastra、Deep Agents、Vercel AI SDK 等框架的集成。

## HN 评论精华

约 40 条评论，155 分，讨论规模不大，以向作者提问为主，作者回复较积极。

**"往返延迟"到底省在哪？** 这是被追问最多的问题。cl685 问改用扩展后相比 CDP 失去了什么（比如跨域 iframe、无法加载扩展的环境里的下载），作者解释："它仍然使用 CDP，只是从浏览器内部的扩展发起通信，而不是从一个独立运行时里的脚本发起，更糟的情况是从另一个地区发起。"vishalanton 问一家有 1000 个 Playwright 测试的企业是否能因此把 CI 时间缩短约一半，作者说"正是如此"，开启批量操作还能再加速。但 sa46 质疑："测试通常就在同一台机器上驱动浏览器。你说的是哪种往返？进程间走 CDP 那种吗？"这个问题没有得到回答。pixelstack 从文档判断，承诺的提速似乎只有用他们的主力产品才能实现，作者否认，称只要浏览器接受自定义扩展，你完全可以自己部署和管理。dot_louis 问是否必须付费使用 Browserbase，作者回答不需要，Stagehand 开源、默认就能用本地浏览器。

**缓存与自愈的细节。** ulrikrasmussen 觉得缓存的想法让它适合做能自愈的 CI 测试，但追问：怎么判断缓存的 `act()` 失效、需要让 LLM 重新推理？缓存放在云端会不会在 CI 里被不同版本的网站反复冲刷？为什么缓存不能是一个和脚本一起签入仓库的本地文件，这样开发者可以在本地用 LLM 修复失败的测试，而 CI 完全确定性地运行？作者的回答是：缓存是 Stagehand API 中托管的服务端层，以指令、页面内容和传入选项为键，刻意排除了模型配置，所以换模型不会让缓存失效；页面结构一变就不会命中缓存，回退到完整推理；缓存的选择器如果不再能解析也会回退，且重放期间关闭自愈。至于本地文件，目前缓存键基于 Browserbase 会话，所以纯本地浏览器下缓存选项不起作用。每次调用都会返回元数据（状态、未命中原因、阈值、次数、节省的 token），方便观察缓存波动而不是靠猜。

**为什么不用"电脑操作"（computer use）方案？** cl685 问为什么不直接用基于截图的电脑操作。smpandya 说自己试过，结论是解析截图、生成动作、被限制在有头模式，这套开销远不如读可访问性树再生成 CDP 命令高效："浏览器自动化是比电脑操作更贴近底层的抽象，更灵活，规模化后也便宜得多。"vishalanton 也说电脑操作方案烧 token 太凶。ChemSpider 指出，如果 agent 需要在桌面上运行或登录用户真实的浏览器，可以考虑 Ui.Vision MCP，但 Stagehand 是为后端场景设计的，二者不好直接比较。

**v4 为什么删掉了 `agent()`？** ishankunam 注意到 v4 完全移除了 `agent()`，只保留 `act()`、`observe()`、`extract()`。作者解释，生态里已经有太多优秀的 agent 框架，与其保留它，不如让 v4 更好地集成主流框架，包括编码 agent 层面的 Codex、Claude Code，以及框架层面的 Eve、Deep Agents、Mastra 等。alyssamaru 问评测里框架（harness）到底有多大影响，作者说影响很大，尤其是性能：以 Claude Opus 5 为例，用 Deep Agents、Eve 还是 Fx，准确率差距可达 3%、性能差距可达 200 毫秒，所以他们做了同时考虑模型和框架的基准测试。团队成员 MiguelG719 补充了刚合并的 Jev 支持在评测上的数据：act 快 4.3 倍、LLM 调用减少 97%，通过率从 97.5% 升到 98.3%；observe 快 11.1 倍、调用减少 69%，通过率从 75% 升到 83.3%；extract 快 8.7 倍、调用减少 75%，通过率持平于 92%，"成本几乎为零"。

**悬而未决的问题与怀疑声音。** 一些技术性较强的问题没有得到作者回复：bradleyy 用 Playwright 诊断客户网站，问是否完整支持 Chrome DevTools（比如追查某个 fetch() 从哪发起、哪个脚本设置了 Cookie），以及在 AWS Lambda 上部署难不难；Phemist 希望能把网络请求和 WebSocket 消息的过滤函数直接放在浏览器扩展里执行，因为 Playwright 要把完整响应传给过滤函数，而他往往只关心约 1% 的请求，读前 100 个字节就能做出判断，这种设计极其浪费；squidproquo 问"混合式可访问性树裁剪"具体怎么工作。shashanoid 泼了盆冷水："不管你让它快多少，Playwright 就是 Playwright，一眼就能认出是机器人。"swingboy 回应说很多人用 Playwright 是做 UI 测试的。bensyverson 顺带推荐了自己做的一个专为 agent 设计的小型无头 WebKit 浏览器 Sleepy Hollow。throw03172019 问能否在客户电脑上做浏览器自动化（他们本来就装了软件控制老旧 Windows 应用的 GUI），唯一明显的问题是 LLM 的 API token 会暴露在客户端。
