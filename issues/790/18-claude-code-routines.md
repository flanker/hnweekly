---
layout: article
title: "Claude Code Routines：让 Claude Code 在云端自动运行"
issue: 790
number: 18
category: code
original_url: "https://code.claude.com/docs/en/routines"
hn_url: "https://news.ycombinator.com/item?id=47768133"
date: 2026-04-17
---

## 文章摘要

Claude Code Routines（路由例程）是 Anthropic 新推出的一项研究预览功能，让 Claude Code 以"自动驾驶"模式在云端运行。一个 routine 由一段提示词、一个或多个 GitHub 仓库、一组 MCP 连接器组成，Anthropic 把这套配置打包后托管在云端基础设施上运行，即使开发者合上笔记本也能继续工作。

每个 routine 可以挂载三类触发器并可组合使用：**Scheduled（定时）**——按小时、每日、工作日或每周的节律运行；**API**——为 routine 分配独立 HTTPS 端点与 bearer token，任何能发起 HTTP 请求的系统都可按需触发；**GitHub**——订阅指定仓库的 Pull Request 或 Release 事件，事件匹配即启动一次新会话。PR 事件还支持按作者、标题、分支、标签、是否草稿等过滤条件精细匹配。

文档列举了若干典型场景：**Backlog 维护**（每晚读取新 issue 并打标签、分派负责人，汇总到 Slack）；**告警分流**（监控系统通过 API 触发，让 Claude 分析堆栈并提交修复 PR 草稿）；**定制化代码审查**（在 `pull_request.opened` 时跑团队自定义的审查清单并留下行内评论）；**部署校验**（CD 流水线部署完成后调用 API，执行 smoke 检查并向发布频道回报结果）；**文档漂移检测**（每周扫描合并过的 PR，自动发现需更新的文档并开 PR）；**跨语言库端口同步**（一个 SDK 仓库合并 PR 后，自动在另一个语言的姊妹仓库生成对应 PR）。

routine 以完整的云端 Claude Code 会话形式运行，没有权限弹窗与审批中断。能力边界由所选仓库及分支推送设置、云环境的网络访问与环境变量、以及挂载的 MCP 连接器共同决定。默认情况下 Claude 只能推送 `claude/` 前缀的分支以防误改主干。API 触发使用 `POST /fire` 接口，可在 body 的 `text` 字段传入运行时上下文（如告警正文）。目前该功能面向 Pro、Max、Team、Enterprise 订阅且需启用 Claude Code on the web，按账户计入每日运行配额，超出后可启用 metered overage。

## HN 评论精华

1. **厂商锁定疑虑**：joshstrange 表示对依赖 LLM 厂商专有特性构建工作流持明显戒心。他强调 LLM 厂商并非稳定的商品级供应商，Anthropic 产品和条款随时可能变动；因此他坚持只使用模型与标准 API，避免把关键流程绑死在某家平台特有的"routine"功能上，以维持切换成本可控。

2. **把上下文放回本地**：Nevermark 提出一种更可移植的做法——把 Claude 的默认记忆与提示存到项目本地的 Markdown 文件中，纳入版本控制。这样跨机器、跨工具链都能延续，模型独立性与 workflow 可复现性都能兼顾，比依赖平台侧的记忆存储更稳健。

3. **锁定的真实代价**：ElFitz 分享了亲历的"粘性"体验：他曾尝试在虚拟机里用 Claude Code CLI 做自主调试，结果撞上服务端对自动化场景的限制与 ToS 约束，不得不放弃。这类案例说明，一旦工作流围绕某个厂商的非标能力构建，后续转向或自托管将非常困难。

4. **抽象为工作流引擎**：rbalicki 建议把复杂任务拆成更小的、由工作流引擎编排的子任务，而不是交给某个厂商的 orchestrator。这样既能降低对具体 LLM 实现的依赖，又能提升 token 效率与调试透明度——每一步都能观察、替换、重跑。

5. **云时代的历史回声**：freedom 把这波 AI 平台的策略类比早期 AWS：看似便利的"托管编排"本质上是在构建技术护城河。他提醒社区保持警惕，正面的反例是 MCP 协议保持开放——这说明 AI 基础设施完全可以选择不走封闭路线，问题只在于厂商是否愿意。
