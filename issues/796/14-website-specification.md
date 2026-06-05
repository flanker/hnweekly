---
layout: article
title: "网站规范（The Website Specification）"
issue: 796
number: 14
category: show_hn
original_url: "https://specification.website/"
hn_url: "https://news.ycombinator.com/item?id=48343683"
date: 2026-06-05
---

## 文章摘要

《网站规范》（The Website Specification）是一份开源的技术参考指南，自我定位为"一个平台无关的、关于一个好网站应当具备哪些技术特性的完整规范"。它试图把现代 Web 开发的最佳实践系统化地汇编成一份可供人类和 AI 共同查阅的文档。

该规范将建议归纳为十大领域：一是**基础规范（Foundations）**，涵盖 doctype、lang 语言属性、字符集、viewport 视口设置、标题等核心 HTML 元素与元数据；二是**SEO**，包括 robots.txt、站点地图、结构化数据和 URL 架构；三是**无障碍（Accessibility）**，遵循 WCAG 指南，确保键盘导航、色彩对比度、替代文本等对残障用户友好；四是**安全（Security）**，包括 HTTPS、内容安全策略和安全响应头；五是 **Well-Known URI**，即 /.well-known/ 路径下用于发布站点元数据的标准化路径；六是**面向 Agent 的就绪度（Agent Readiness）**，主张通过结构化数据、markdown 端点和 MCP 服务器让网站对 AI 系统更易读；七是**性能（Performance）**，涵盖 Core Web Vitals、图片压缩、缓存策略和字体加载优化；八是**隐私（Privacy）**，包括同意机制、数据最小化以及尊重 Global Privacy Control 等用户信号；九是**韧性（Resilience）**，涵盖优雅的错误处理、离线支持和监控；十是**国际化（Internationalization）**，包括 hreflang、RTL 布局和本地化格式。

值得一提的是，该规范本身就身体力行地展示了它所倡导的理念——它以多种格式提供内容：HTML、markdown 端点、XML 站点地图、一个 MCP 服务器以及可被发现的 Agent skills，从而让指南本身既对人类友好，也对 AI 系统友好。不过这一点也恰恰成为 HN 社区争议的焦点。

## HN 评论精华

1. **"Agent 就绪"会不会沦为新泡沫**：高赞评论者 Latty 尖锐地指出，"Agent Readiness"这个章节最终的命运很可能和当年的"Web 4.0 区块链集成"一样。他澄清这并非因为 Agent 不会成为重要技术，而是因为：即便 Agent 真的重要，要求网站做出"特殊照顾"本身就违背了 Agent 的初衷；这类机制最终只会被坏人利用，让 Agent 看到的内容与人类看到的内容产生错配，因此会被刻意忽略。

2. **质疑这是"AI 流水线产出的劣质内容"**：评论者 selfhoster1312 直言这看起来像是"slop 工厂里产出的 slop（AI 垃圾）"，并讽刺"SEO"和"Agent 就绪"恰恰是一个好网站不该做的事。他还扒出作者背景——一位使用 Claude LLM 的 WordPress "SEO 专家"兼私人投资者，并讥讽这位"靠广告 slop 毁掉我们曾经热爱的互联网而积累财富的人，如今又在用 LLM slop 摧毁所剩无几的部分"。Kwpolska 也翻看了 Git 提交历史，认为内容"大多是 slop"，并好奇为何这些"slop 搬运工"从不关掉 Claude 的自我署名。

3. **真正想要的是登录表单这类实用规范**：用户 fmajid 表示，比起这些宏大条目，他更希望看到关于登录表单的实用最佳实践，例如：使用密码管理器能识别的标准输入框名称、在登录框上禁用自动补全和自动大写、邮箱字段使用正确的 HTML5 input 类型、不要把邮箱和密码强行拆成两步、遵循 NIST SP 800-53（不用短信二次验证、不强制密码轮换和组合规则），以及让只有一个输入框的表单自动聚焦。

4. **llms.txt 被指"有害无益"**：用户 franze 批评 llms.txt 目前没有任何一家主流 AI 提供商支持，应被视为"有害"——因为站长会误以为它有作用（虚假的影响力），实则毫无效果，净收益为负。他强调一个好网站应当服务于网站所有者及其目标用户的目标，而不是"为所有人做所有事"。

5. **也有人觉得实用**：用户 rsolva 表达了感谢，说他本打算自己做一个类似的 skill，结果发现把这份规范直接粘贴进任意 Agent 对话框就效果极佳。他让本地模型（Qwen3.6 27B）列出自己一个旧 Hugo 站点缺失的所有标准、生成待办清单并逐项执行，Agent 甚至从 logo 中裁剪出缺失的 favicon。资深开发者 baliex 也称这是个好资源，做了 30 年网站仍能从中捡到一些基础知识，但他也细致地区分了"规范强制要求"（如 title 标签）与"基于观点的要求"（如 HTTPS）。
