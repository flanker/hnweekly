---
layout: article
title: "Claude Code 源码泄漏分析：假工具、脏话正则、隐身模式等"
issue: 788
number: 4
category: favorites
original_url: "https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/"
hn_url: "https://news.ycombinator.com/item?id=47586778"
date: 2026-04-03
---

## 文章摘要

Anthropic 意外地在其 npm 包中附带了一个 `.map` 文件（source map），其中包含了 Claude Code CLI 工具的完整可读源代码。虽然包很快被撤下，但代码已被广泛镜像并在 Hacker News 上被深入剖析。这是 Anthropic 一周内的第二次意外泄漏（几天前刚泄漏了模型规范）。作者 Alex Kim 花了一上午时间阅读泄漏源码，按"辛辣程度"排序整理了以下发现：

**反蒸馏机制：注入假工具污染抄袭者。** 源码中有一个 `ANTI_DISTILLATION_CC` 标志，启用后会在 API 请求中发送 `anti_distillation: ['fake_tools']`，指示服务器在系统提示中静默注入虚假工具定义。目的是：如果有人录制 Claude Code 的 API 流量来训练竞品模型，假工具会污染训练数据。此外还有第二种反蒸馏机制——服务端连接文本摘要，API 会缓冲助手在工具调用之间的文本并生成带加密签名的摘要，录制者只能获得摘要而非完整推理链。不过作者指出，这些保护手段绕过起来并不困难，真正的保护可能是法律手段而非技术手段。

**隐身模式：隐藏 AI 身份的 AI。** `undercover.ts` 实现了一种模式，在非内部仓库中使用 Claude Code 时会剥除所有 Anthropic 内部痕迹——禁止模型提及内部代号（如"Capybara"、"Tengu"）、内部 Slack 频道、仓库名称甚至"Claude Code"本身。关键是：可以用环境变量强制开启此模式，但没有办法强制关闭。这意味着 Anthropic 员工在开源项目中 AI 撰写的提交和 PR 不会有任何 AI 参与的标识。

**沮丧检测：用正则表达式（是的，正则）。** `userPromptKeywords.ts` 包含一个正则表达式模式，用于检测用户的沮丧情绪——匹配"wtf"、"ffs"、"shit"、"fuck you"等脏话。一家 LLM 公司用正则做情感分析堪称顶级讽刺，但正则确实比调用一次 LLM 推理更快更便宜。

**原生客户端证明：JS 运行时之下的 DRM。** API 请求包含一个 `cch=00000` 占位符，在请求离开进程前，Bun 的原生 HTTP 栈（用 Zig 编写）会用计算出的哈希值覆盖这五个零。服务器验证哈希以确认请求来自真正的 Claude Code 二进制文件。这就是 Anthropic 与 OpenCode 法律纠纷背后的技术执行手段——不仅是法律要求第三方工具不要使用其 API，二进制文件本身会通过密码学证明自己是正版客户端。

**每天浪费 25 万次 API 调用。** 代码注释显示，1,279 个会话出现了 50 次以上的连续自动压缩失败（最高达 3,272 次），全球每天浪费约 25 万次 API 调用。修复方法仅需三行代码：连续失败 3 次后禁用压缩。

**KAIROS：未发布的自主代理模式。** 代码库中有一个功能门控模式叫 KAIROS，看起来是未发布的自主代理模式，包含 `/dream` 技能（"夜间记忆蒸馏"）、每日追加日志、GitHub webhook 订阅、后台守护进程和每 5 分钟刷新的 cron 任务。这可能是泄漏中最大的产品路线图披露。

**其他发现**：愚人节彩蛋（电子宠物系统）、终端渲染借鉴游戏引擎技术（Int32Array ASCII 字符池、位掩码编码样式）、23 项 bash 安全检查、提示缓存经济学驱动的架构设计、多代理协调器通过提示词而非代码进行编排。作者还指出，泄漏可能源于 Anthropic 去年底收购的 Bun 的一个已知 bug——source map 在生产模式下仍被提供。

## HN 评论精华

**会话压缩机制详解（d4rkp4ttern）**：详细解释了 Claude Code 如何在 JSONL 文件中保存完整对话，使用 `isCompactSummary` 和 `isVisibleInTranscriptOnly` 等过滤标志控制 API 可见性。介绍了三种压缩类型：全量压缩、会话记忆和微压缩。微压缩是成本优化措施——当 Anthropic 服务端缓存在 1 小时后过期时，下一条消息会重新处理所有 token，微压缩通过清除旧的工具结果内容（只保留最近 5 个）来降低 token 成本。

**隐身模式争议（mzajc 等）**：有人指出隐身提示中的措辞实质上是指示系统"假装是人类"。支持者认为该功能针对的是内部信息泄漏而非欺骗。lawrjone 指出合理的使用场景是允许自定义署名。

**EU 法律影响（riedel）**：引用欧盟 AI 法案第 50 条——要求 AI 系统直接与自然人互动时必须披露，质疑"隐身"提交是否违反此规定。讨论中对代码 PR 是否构成"提供 AI 服务"存在争议。

**署名与 Git 历史辩论**：形成两大阵营。支持署名方（josephg、m132）认为 AI 生成代码应包含 Co-Authored-By 标签以保证透明性。反对署名方（manbitesdog、zen928）认为代码质量比工具署名更重要，类似于 IDE/linter 的使用不需要署名。折中观点（rogerrogerr、heyethan）认为审查者对 AI 生成代码应采取不同的审查方式。还有人（Sharlin、jacquesm）指出 AI 生成代码可能缺乏版权保护，使署名具有法律相关性。

**版权与法律不确定性**：多个讨论线程涉及 AI 生成代码的版权问题、合规性以及 AI 系统披露义务，但目前缺乏既定先例。有人担忧 Anthropic 通过隐身 PR 进行"野外评估"，以及针对人类维护者的潜在蒸馏攻击。

**历史代码生成对比（cess11、TheDong、chrislo）**：讨论了 LLM 之前的代码生成工具（Rails generators、go:generate 等）历来都会标记为自动生成，建议对 LLM 采取类似做法。但也有人指出 LLM 输出的非确定性使其与确定性代码生成工具的对比并不恰当。
