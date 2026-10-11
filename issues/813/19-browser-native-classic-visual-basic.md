---
layout: article
title: "一个在浏览器里原生运行的经典 Visual Basic 6 IDE"
issue: 813
number: 19
category: code
original_url: "https://wieslawsoltes.github.io/VB6/"
hn_url: "https://news.ycombinator.com/item?id=49956681"
date: 2026-10-09
---

## 文章摘要

这是开发者 Wiesław Šoltés 发布的「VB6 Studio Web」：一个完全用 HTML、CSS 和 JavaScript 写成、在浏览器中运行的经典 Visual Basic 6 风格开发环境。打开网页就能看到熟悉的 VB6 界面——工具箱、窗体设计器、属性窗口、工程资源管理器、代码窗口，按 F5 直接运行工程，按 Shift+F5 停止。项目首页本身只是一个加载 `studio.js` 的空壳单页应用，详细说明来自它在 GitHub 上的 README。作者在 README 里反复强调一点：这**不是**完整、原生、像素级一致的微软 VB6 替代品，兼容范围和限制都另有文档说明。

从功能上看，这个项目的野心远超「怀旧皮肤」：

- **自研编译器和虚拟机**：VB 源码由项目自己的编译器编译成指令，在协作式 VM 中执行，而不是把 VB 文本翻译后丢给 JavaScript 的 eval。支持过程、类、属性、命名参数和可选参数、ByRef、数组与用户自定义类型、On Error 错误处理、事件、精确的 Currency 和 Decimal 类型，以及 Empty/Null/Nothing/Missing 等 Variant 的各种特殊值。
- **完整的 IDE 体验**：对象浏览器、IntelliSense（列出成员、参数提示、快速信息）、书签、拆分代码视图、MDI 文档管理、可自定义工具栏；调试器支持条件断点、逐语句执行、Watch、Immediate 窗口、Run to Cursor、Set Next Statement，甚至有限度的「暂停时修改代码继续运行」。
- **可打开原始工程**：能读取 `.vbp` 工程和 `.vbg` 工程组，保留原始字节、编码和未知附属文件；不安全的修改会直接报错，而不是悄悄丢数据。
- **37 种浏览器控件**，包括 TreeView、ListView、RichTextBox、表格、图表等；数据层提供 ADO 风格接口，可以接 SQLite、REST、OData、GraphQL、JSON/CSV，或通过本地网关连 PostgreSQL、MySQL、SQL Server。
- **多种导出目标**：「File → Make 工程名.html」能把工程导出为自带运行时的单文件 HTML 应用；还有实验性的直接 Win32 AOT 编译（用 JavaScript 写的 PE 链接器生成 x86 EXE）、Electron 打包，以及调用正版 VB6 编译器的经典构建，另外提供迁移到 VB.NET / .NET 10 的导出。
- **一层 Win32 API 兼容库**（`@vb6/win32-browser`），模拟 GDI、注册表、INI 文件、定时器等常用 API，让 `Declare` 声明能在浏览器里跑起来一部分。
- **新潮附加功能**：内置连接 OpenAI/Anthropic/Gemini 的 AI 编码代理窗口，以及把 IDE 暴露为 MCP 服务器、供外部编码代理调用的 125 个工具；主题方面除了 Windows 经典外，还有 Fluent、macOS 26 和 X11 风格。

整个项目以 MIT 许可证开源，构建不依赖任何 npm 包。

## HN 评论精华

这条帖子拿到 400 多分、130 多条评论，讨论主要分成三条线：怀旧、「这是不是 AI 做的」、以及「为什么今天的开发工具反而退步了」。

**作者回应与 Bug 反馈**

- **vunderba** 试着导入高中时写的 VB6 地图编辑器，因为用了 BitBlt 没跑起来，建议至少实现一部分 Win32 API。作者 **wiso** 回复说正在加基本的 Win32 API 兼容层（README 中后来已有相应内容）。
- **badsectoracula** 认为功能上做得很到位，能「编译」成 HTML 正是应该有的做法，但外观有不少瑕疵：窗口按钮歪斜、缺少立体边框。他还报告了几个 Bug，比如点不中 Timer 这类不可见控件、新加控件后代码窗口的控件列表不更新、键盘和鼠标移动事件尚未实现，所以他想写个乒乓球游戏最后只做出一个「会弹跳的单选按钮」。他猜测这是 AI 做的，建议开个 Win95/98 虚拟机截图做像素级参考。作者承认这是个有很多 Bug 的趣味项目，正在持续修复。
- **jordanscales**（98.css 的作者）指出用 `border-color` 画的边框会产生 45 度斜角，看着有「恐怖谷」感，欢迎直接借用 98.css 中用 box-shadow 模拟「阶梯」边框的写法。

**AI 生成之争**

- **truetraveller** 问是不是 LLM 做的；**ranger_danger** 指出作者的 GitHub 账号最近 4 天就有 5000 多次提交、1500 多个 PR。**tech234a** 补充说作者过去一周还发布了浏览器版的 Windows Vista/XP/95、Mac OS 9、Word、Excel、Visio、Illustrator 等大量复刻项目。
- **bitwize** 一度兴奋地说仓库里没有 CLAUDE.md、AGENTS.md，「难道是纯手工？」，随即被 **drnick1** 一句「没提交到仓库而已」浇灭。
- **notpushkin** 举例说「打印代码」对话框里那句说明文字一看就不像人写的——它描述的是实现细节，正常人根本不会把这种话放进界面。**functionmouse** 则干脆说「LLM 缺乏品味」。
- **avallach** 惊叹作者有 500 多个开源仓库；**rwmj** 回应：只要肯花钱，AI 能产出海量垃圾，真正的考验是这些东西到底好不好用。作者 **wiso** 的态度是：这是新时代，几小时就能做出可用原型，有些项目会失败、有 Bug，但探索速度和乐趣无可比拟。

**怀旧与对现代工具的反思**

- **hagbard_c** 感叹它在浏览器里启动之快，应该让当今的 IDE 开发者汗颜。
- **Dwedit** 怀念 VB5 按 F1 即时弹出光标处帮助的体验（VB6 改成了慢吞吞的 MSDN）；**sssilver** 和 **EdgarVerona** 都说 QBASIC 的帮助系统改变了自己的人生，后者 12 岁时靠它自学编程，45 岁已退休。
- **waldrews** 认为属性表（property grid）是「史上最伟大的通用 UI」，此后 Web 和移动端一路走下坡；**franga2000** 也怀念那种「拖一个按钮、点一下就能看到所有属性、双击就生成事件处理函数」的可发现性。
- 反方意见同样有力：**booty** 说 VB6 只适合简单 GUI，复杂一点就是一团乱，而且控件面板的模式在元素多了以后无法扩展；**danielheath** 认为 VB6 语言本身很糟，Delphi 有同样的窗体设计器但语言好得多，而窗体设计器被行业抛弃的根本原因是无法适应响应式布局；**ndiddy** 也认为一旦窗口要能缩放，拖放设计就会变成没完没了地调整锚点。
- **jit_tech** 设想：如果编码代理足够快，也许能做出一个 VB6 式的 RAD 工具，抽象失效时再深入到代理生成的真实代码里。
- 也有人泼冷水：**fg137** 本以为是在浏览器里模拟 VB6，结果发现更像「复古版 Wix」，生成 HTML 和 JavaScript，并不兴奋；**webprofusion** 提醒使用微软仍在使用的商标存在风险。
