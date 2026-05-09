---
layout: article
title: "可玩的 DOOM MCP 应用：让 ChatGPT 和 Claude 跑起 DOOM"
issue: 793
number: 61
category: fun
original_url: "https://chrisnager.com/blog/doom-runs-in-chatgpt-and-claude/"
hn_url: "https://news.ycombinator.com/item?id=47939079"
date: 2026-05-08
---

## 文章摘要

Chris Nager 利用 **Model Context Protocol（MCP）** 的 App 扩展，把经典的 *DOOM* 嵌进了 ChatGPT 和 Claude 的对话窗口里。这并不是让 LLM "幻觉出"游戏帧，而是借助 MCP 现在支持的 "MCP Apps" 能力，把一个真实的网页应用作为对话内的可交互组件渲染出来——也就是说，AI 客户端本身变成了游戏的运行环境。

技术栈分两部分。**浏览器侧**基于 Cloudflare 公开的 [`doom-wasm`](https://github.com/cloudflare/doom-wasm) 运行时，默认加载 Freedoom Phase 1 资源，部署在 `/doom/play` 路由，通过签名 token 鉴权。**MCP 侧**是一个 TypeScript 编写的 MCP server，向 AI 客户端暴露两个工具：`create_doom_session`（在支持 inline 渲染的宿主里直接拉起游戏）和 `get_doom_launch_url`（在不支持时退化为浏览器跳转链接）。两条路径共用同一个签名 token，因此服务器无需保存会话状态。

作者在博客里着重讲了一段架构上的"踩坑"：他最初想用嵌套 iframe 把游戏页面塞进 MCP App 的 iframe 里，结果撞上 frame-src/CSP 的安全限制。**最终的破局思路是"把 MCP App 本身当作浏览器页面"——也就是直接由 MCP 启动一个完整页面，而不是嵌一个进去**——这一下解决了 CSP 冲突。资源文件（WAD、贴图等）也从 blob 预加载改成直接打包进 Emscripten 文件系统，提升了跨环境的稳定性。早期版本里还有存档/读档和持久化适配层，后来全部砍掉，只保留稳定 + 好玩这两个目标。

## HN 评论精华

1. **firasd**：高度认同 MCP 被低估了。大家都把 MCP 当成"接 Google Drive"那种 RPA 工具，其实它完全可以承载**对话内的微型应用**——他自己做的 Liveclip 项目就是把"表格"作为一种聊天 native 控件，他还顺手部署了一个零鉴权的 MCP 时钟（`mcpclock`）演示在所有主流 AI 客户端里都能跑。

2. **avaer**：吐槽标题有点误导——DOOM 并不"运行在 ChatGPT/AI 上"，而是运行在外部网站上，AI 客户端只是把它的 iframe 渲染了出来。但他承认很多人确实不知道 MCP 现在已经支持 web resource 渲染（指向 `modelcontextprotocol.io/extensions/apps/overview`），这种澄清本身就有科普价值。

3. **_pdp_**：补充技术细节——只要注册一个 MCP resource 并在工具调用里返回，宿主就能把任意 HTML 渲染进 iframe。本质上不复杂，但能开出的玩法非常多。

4. **JambalayaJimbo**：作为正在折腾 MCP Apps 的开发者，问到他自己在 VSCode Copilot 里渲染图片时遇到的 CSP 与静态资源服务问题，希望作者能分享更多具体的踩坑记录——这一类提问也反映 MCP Apps 当前的开发者文档还不够完善。

5. **eigenblake**：晒出两周前他用 MCP App 跑过 *Bad Apple*（YouTube 链接）。说明这种"在 AI 客户端里嵌任意 web 内容"的玩法已经形成了一波类似"can it run DOOM?"的社区文化。

6. **alach11**：说他们公司在内部上 Open WebUI 时，最早做的两个测试也是贪吃蛇和 DOOM——**用游戏来摸清新技术栈的边界**几乎是行业惯例。

7. **throwawayk7h**：略遗憾这次不是"让 AI agent 自己玩 DOOM"——他原本期待的是反过来的方向：MCP 把游戏画面回传给模型，让模型用工具调用控制角色。这两种方向其实都有人在做，未来"双向"或许都会成立。

8. **phodo**：提了个有趣的元问题——下一个"Can it run DOOM?"是什么？是"能不能跑 AGI"或者"能不能跑在生物细胞里"？还是 DOOM 永远是那个 DOOM？
