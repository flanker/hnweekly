---
layout: article
title: "Pele：把 Mermaid 图表渲染成可主题化的 SVG"
issue: 815
number: 5
category: favorites
original_url: "https://pele.run/"
hn_url: "https://news.ycombinator.com/item?id=50132193"
date: 2026-10-23
---

## 文章摘要

Pele 是一个轻量的开源库，作用只有一件事：把 Mermaid 语法写成的图表渲染成 SVG，并用 CSS 变量控制配色和字体。它由 Obsidian 团队开发，作者是 Obsidian 的 kepano（Steph Ango），最初是为了让 Obsidian 能快速渲染图表、并让图表自动匹配用户的主题。名字取自夏威夷神话中的火山与火焰女神佩蕾，采用 MIT 许可，源码在 GitHub 的 obsidianmd/pele 仓库。

**卖点：小、快、零依赖。** 官网给出的对比是：gzip 后体积约 230 kB，而 Mermaid.js 约 1.6 MB；单张图渲染耗时 0.05 到 0.3 毫秒，Mermaid.js 则要 6 到 82 毫秒，号称最多快 500 倍；Pele 没有任何直接依赖，Mermaid.js 有 23 个。在小屏幕上，Mermaid 只会把整张图等比缩小，Pele 则会重新调整布局，比如把过宽的流程图改成纵向排列。渲染是同步的，`render()` 直接返回 SVG 字符串，`mount()` 则在浏览器里渲染到某个元素，从 CSS 读取字体，并随容器宽度自适应。这意味着它不需要像 Mermaid 那样借助完整的或无头浏览器来做布局，可以在服务端、构建脚本里直接生成静态 SVG。体积敏感的项目可以只注册需要的图表类型，或者用 `pele/lazy` 按需异步加载，也可以直接从 jsDelivr 引入。

**兼容性：只学语法，不学布局。** Pele 声称支持 Mermaid 12.1.0 内置的全部图表类型，从流程图、时序图、类图、状态图、ER 图、甘特图、Git 图、饼图、思维导图、看板、时间线，到桑基图、C4、架构图、象限图、雷达图、韦恩图、鱼骨图、Wardley 地图等三十多种，但不支持 ZenUML 等外部插件。解析器用 Mermaid 自己的解析器测试用例和差分模糊测试来验证。与 Mermaid 的主要区别在于：布局引擎是自己写的，节点位置、间距和连线路径都不同；Mermaid 的主题和 themeVariables 配色不生效，统一改用 CSS 变量；流程图中的子图即使与外部节点相连，也会保持自己的方向；标签使用 SVG 文本，支持换行、粗体和斜体，但不支持任意 HTML；`click` 只支持打开链接，JavaScript 回调会被忽略；数学公式需要应用自己传入 KaTeX、MathJax 等排版器。

**主题。** 文档列出了 `--pele-bg`、`--pele-fg`、`--pele-line`、`--pele-surface`、`--pele-accent` 等一组变量，以及给图表分类着色用的 `--pele-series-1` 到 `--pele-series-8`。切换浅色和深色模式只需改变量，不必重新渲染。SVG 中各元素都有类名，也可以用 `data-id` 精确定位某个节点单独设置样式。由于 Pele 需要测量文字来决定节点大小，渲染前必须拿到字体，换字体后要调用 `refresh()`。

**安全。** 文档专门有一页讲如何渲染不受信任的图表：Pele 会转义图中文本、过滤链接和样式，默认只允许 http、https、mailto、tel 链接，`javascript:`、`data:` 等协议一律拦截；源码长度、SVG 输出大小和流程图边数都有上限。错误信息里可能包含图表原文，因此应用应当用 `textContent` 显示；图标和数学公式的标记由应用提供，Pele 不做过滤，需要应用自己把关。

## HN 评论精华

本帖获得 593 分、约 56 条评论，整体评价积极。作者 kepano 在评论区回应，讨论集中在 Mermaid 布局糟糕、Mermaid 依赖浏览器渲染、替代工具推荐，以及名字的各国含义。

**Mermaid 的布局才是痛点。** lukeify 说每次用 Mermaid 画稍复杂一点的图，都会被那种晦涩、拓扑上很差的布局搞崩溃，相比之下样式简直是小问题。tptacek 提醒图布局本来就是出了名的难题；wongarsu 认为大多数自动布局都很糟糕，用 SAT 求解器可以做到没有交叉线，却很少得到符合直觉的图，好的布局更像写作，要考虑读者先注意什么。kepano 回应说正因如此，他设计 Pele 时只沿用 Mermaid 的语法，不沿用它的布局，并指出 playground 里有对比模式。不过 nkmnz 试了几张图没看出明显差别，WhrRTheBaboons 发现时间线示例的前两张图一模一样。Ancapistani 则说光是「子图保持自身方向」这一条，就足以让他换过来，哪怕 Pele 更慢也愿意。

**为什么不贡献给 Mermaid。** CrimsonRain 问为何不把 Pele 并入 Mermaid，blazarquasar 引用 kepano 在 X 上的说法：Mermaid.js 已经转向 mermaid.ai，偏离了初衷；12.1 版又加了一个布局引擎（现在已有三个），安全记录也有问题，Obsidian 1.13 甚至不得不加了一个针对潜在不安全 Mermaid 图的开关和警告。

**不用浏览器就能渲染。** nickmain 问官方 Mermaid 是否仍需要完整或无头浏览器来布局，如果是，Pele 对在其他地方嵌入 Mermaid 渲染是个好消息。dont__panic 说至今想不通 Mermaid 开发者为何认为渲染一张 SVG 需要整个浏览器。bilibali 的静态站点构建脚本原本为此要启动整个 Chromium，已经换成 Pele，而且更喜欢 Pele 的布局；9dev 把自己项目文档里的图迁移过来，省掉了 Mermaid 加 panzoom 加各种暗色主题变通方案的组合，kepano 称赞了他的效果；haburka 也说用 CSS 变量给 Mermaid 做暗色模式、在服务端运行它简直是噩梦。falsaberN1 推荐了另一个输出 SVG 的项目 Merman，yencabulator 却泼冷水说那个项目「就是 Claude 失控的产物」。

**替代工具。** thegagne、m-p-3、burgeonlab 推荐 D2，配合 ELK 或 dagre 布局效果不错，jhoechtl 补充 D2 现在已包含 TALA 布局引擎；burgeonlab 换到 D2 的另一个原因是 Mermaid 在力推 AI。也有人推荐 TikZ、GraphViz。另一些人（giancarlostoro、jd3、kylepomykala）干脆让大模型生成 Mermaid 或 HTML；dbg31415 认为这类工具让 Claude 能在单个 Markdown 文件里画图、编辑和保存，对版本控制很友好。DenisM 希望能有图表的可视化 diff。

**反馈与杂谈。** kinduff 报告 playground 里的 Mermaid 预览坏了，kepano 跟进排查；shellerik 觉得示例对暗色模式不太友好。Imustaskforhelp 建议页面顶部放一个 GitHub 图标，方便找源码和点星。名字引发了一串接龙：拉脱维亚语里是「老鼠」，葡萄牙语里是「皮肤」，还有球王贝利，几位巴西和非巴西读者都以为是在致敬贝利，看了说明才知道是火山女神。
