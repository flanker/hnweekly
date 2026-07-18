---
layout: article
title: "我是如何在 Go 中使用 HTMX 的"
issue: 801
number: 26
category: code
original_url: "https://www.alexedwards.net/blog/how-i-use-htmx-with-go"
hn_url: "https://news.ycombinator.com/item?id=48912175"
date: 2026-07-17
---

## 文章摘要

Alex Edwards（《Let's Go》系列作者）分享了他在 Go Web 应用中搭配 HTMX 的一套完整实践。他喜欢 HTMX 的理由是：能以极少的 JavaScript 给页面加上"类原生应用"般顺滑的交互，同时保留服务端用 Go 的 `html/template` 渲染 HTML 所带来的一致性与安全性。文章重点不在 HTMX 本身，而在 **Go 这一侧的工程结构**：模板组织、部分响应与整页响应的返回策略、重定向与错误处理，以及他常用的 HTMX 配置项。

**模板结构**上，他把 `assets/html` 分为三层：`base.tmpl`（所有页面共用的布局）、`pages/`（各页面专属内容）、`partials/`（可复用片段）。他坚持给每个模板用 {% raw %}`{{define "name"}}`{% endraw %} 显式命名（如 `page:title`、`partial:image:gopher`），冒号只是任意的命名空间分隔符。静态资源与 HTML 用 Go 1.16 的 `//go:embed` 嵌入二进制，并用 `fs.Sub` 拆成 `HTMLFiles` 和 `StaticFiles` 两个子文件系统以做隔离。

**渲染核心**是一个 `htmlRenderer` 类型：启动时解析共享模板集，其 `render()` 方法会 `Clone()` 一份共享模板、按需追加页面模板、执行指定的命名模板并写入响应。关键技巧在于——同一个 `pages/users.tmpl` 里既定义整页 `page:content`，又单独定义 `users:rows` 片段；于是列表页执行 `base` 返回整页，而搜索接口只执行 `users:rows` 返回表格行片段，交给 HTMX 局部替换。

文章还讲了几个容易踩坑的细节：(1) 用 `isHTMXRequest()` 检查 `HX-Request: true` 头，据此决定返回整页还是片段，避免用户直接访问 `/users/search?query=x` 只看到裸片段；(2) 相应地要设置 `Vary: HX-Request` 响应头，告知中间缓存响应会因该头而不同；(3) 后退按钮问题：HTMX 缓存未命中时会重发请求，需把 `historyRestoreAsHxRequest` 配置为 `false`，否则会错误地返回片段；(4) 重定向不能用普通 3xx（浏览器会抢先拦截），要返回 2xx 加 `HX-Redirect` 头，并对非 HTMX 请求回退到标准 3xx（渐进增强）；(5) 4xx/5xx 响应默认不会被 HTMX 交换，需另行处理。整体思路是：用少量结构性思考，让同一套 Go 代码优雅地兼顾"整页"与"片段"两种输出。

## HN 评论精华

- **alexedwards**（作者本人）现身澄清：`render()` 里对模板做 `Clone()` 主要是为了方便与（可能的）性能收益，并非严格必需，而是他这套模式下的务实选择。
- **nzoschke** 介绍了所谓 "GUS stack"（Go、Unix、SQLite）搭配 HTMX、templ 等工具，强调其开发效率以及与 LLM 辅助编程的良好契合。
- **yawaramin** 主张用"语言内嵌 HTML"的库（Go 里的 gomponents）替代传统模板，认为这样能获得 HTMX 动态需求所需的更好的组件化与灵活性。**dhamidi** 也认同抽象很关键：缺少像样的组件构建器和路由体系，HTMX 项目维护起来会很痛苦。
- **overflowy** 提出反对意见，认为 HTMX 的复杂度会以"2:1 的速率"膨胀，对于带筛选、虚拟滚动的数据网格这类复杂 UI，Mantine 加内嵌资源反而更合适。
- **asdfsa32** 进一步指出 HTMX 只适合"狭窄场景"，认为 React 的成熟度使其在复杂场景下更优（尽管更复杂）。**arch1e** 则分享了亲身经历：HTMX 在处理彼此关联、共享状态的组件时力不从心，最终他改用了 SvelteKit。
