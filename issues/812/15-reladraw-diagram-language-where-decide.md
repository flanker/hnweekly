---
layout: article
title: "Reladraw：一种由你决定元素摆放位置的图表语言"
issue: 812
number: 15
category: show_hn
original_url: "https://github.com/reladraw/reladraw"
hn_url: "https://news.ycombinator.com/item?id=49858513"
date: 2026-10-02
---

## 文章摘要

Reladraw 是一个用纯文本描述图表的语言和渲染工具，作者 jpwalsh234 在 HN 上发布后拿到 400 多分。它想解决的问题，很多画过架构图的人都遇到过。

**两头都不顺手。** 作者在自述中说，他很喜欢用图表来理解问题、做规划，但现有工具大致分两类：一类是 Mermaid、Graphviz、D2 这样的「自动布局」语言，你声明方框和连线，位置由引擎决定，如果你脑子里已经有一幅具体的画面，它们就不合适了；另一类是 draw.io、Excalidraw 这样可以随意摆放的绘图软件，表现力强，但无论是人拖拽节点，还是 AI agent 重新计算坐标、编辑冗长的 XML 源文件，都很费工夫。Reladraw 想站在两者之间：保留图表语言「写文本即可生成图」的好处，同时拥有 draw.io 那样的自定义摆放能力。README 里展示了一张作者先在 draw.io 手绘的架构图，再用 Reladraw 写出几乎一样的版本。

**相对位置，而不是坐标。** Reladraw 的核心是用关系描述位置，不用填坐标。README 中的例子：

```
node app "Web app"
node app.ui  "Interface"
node app.api "API"  below app.ui

node store "Database"  right of app  level with app

edge app.api -> store  "queries"  from: right  to: left
```

这里 `app.ui`、`app.api` 用点号表示它们包含在 `app` 容器里，容器尺寸随内容自动调整；`below`、`right of`、`level with` 声明相对位置；连线可以指定从哪一侧出发、到哪一侧结束。根据语法文档，节点文字中用「空格/空格」表示换行，`TCP/IP` 这种不带空格的斜杠照常显示；长语句可以通过缩进续行；没有嵌套块，缩进只表示续行，从不表示包含关系。文字的颜色、大小、换行宽度、对齐等属性写在文字后的括号里，还可以定义样式复用，用透明容器把一组节点当作整体来定位。

**冲突时报错，不替你猜。** 如果所有约束能同时满足就全部生效，比如「在 A 右边且在 B 下面」，或者「在 A、B、C 右边」会自动避开伸得最远的那个；如果自相矛盾（既在 A 左边又在 A 右边），就报错并指出冲突的语句，没有哪条会悄悄胜出。多条连线进入同一个节点的同一侧时，会自动间隔开以免交叉，空间不够时会向外弯曲让路；它不做绕障碍物的自动寻路，但可以指定「这些线从这两个节点之间穿过」。

**使用方式。** 解析器、布局引擎和 SVG 渲染器都用 TypeScript 写成，零运行时依赖。可以 `npm install -g reladraw` 后用命令行把 `.reladraw` 文件转成 SVG；也可以在网页中引入一个脚本，直接写 `<reladraw-diagram>` 标签；仓库还提供在线 playground 和一个 agent skill（`npx skills add reladraw/reladraw`），让 Claude Code、Codex、Cursor 等编码助手学会使用它。作者特别强调它对 agent 友好：agent 只需说「这个节点放在那个的右边」，坐标算术交给工具，不必在「渲染、看图、微调」之间反复烧 token。

**项目状态。** 当前版本 0.16.0，作者称处于早期阶段但可用，语法还会变动。采用 Apache-2.0 许可（不含名称和商标权利）。项目欢迎提 issue，尤其欢迎「用 Reladraw 画不出来的图」，但不接受 PR。值得一提的是，HN 讨论期间作者根据评论快速迭代，加入了直角连线、虚线/点线、多套主题、续行等功能。

## HN 评论精华

评论整体非常正面，很多人表示被 Mermaid 或 Graphviz 的自动布局折磨过。作者几乎回复了每一条评论，讨论集中在与同类工具的比较和功能建议上。

**和 Pikchr、pic、TikZ 有何不同？** **v9v** 说它让人想起 Pikchr。作者解释，Pikchr（以及更早的 pic）是「海龟绘图」：一个光标边走边画，必要时用坐标偏移。Reladraw 则是声明关系。**gandreani** 追问具体差异，作者认真比较后承认自己最初举的容器例子不恰当，Pikchr 也能处理；真正的区别在于表达意图的方式。比如想表达「A 在左边，B、C、D、E 都在 A 右边」，Pikchr 要写成「建一个组、向下画 B、移动 0.1、画 C……再把组的西侧对齐到 A 东侧偏移 0.5」，而 Reladraw 只需逐行写 `right of a`。他说两者都能到达同一个目的地，区别在于口味和表达方式。**lastscattering** 指出这基本就是 TikZ 的 positioning 库（`right=of`、`below left=of`），只是不用碰 LaTeX；他说 TikZ 里真正痛苦的不是节点而是连线，两条线想用同一侧就会变得很难看。作者回答了连线间隔和冲突处理的机制（见上文）。**light_hue_1** 希望有人干脆实现一个脱离 LaTeX 的 TikZ，作者说 Reladraw 选择了更窄的任务：方框、分组、箭头。

**「Nothing silently wins」引发的小争论。** 作者说矛盾的约束会报错而不是悄悄选一个，**drfloyd51** 大赞：「永远别改。会有人抱怨，那些人也喜欢类型不安全的语言。」**asqueella** 则认为应该是警告，画个图还要和编译器较劲有点过了。作者的回应比较平衡：真正无法渲染的矛盾（B 同时在 A 左边和右边）应该报错；但如果只是没说明连线从上方还是下方绕过，工具拒绝工作就太烦人了，这种情况应该给一个可以覆盖的合理默认值。

**和 D2/TALA 比较。** **zikohh** 问 D2 不是挺好用吗？**Footprint0521** 说 Reladraw 的优势是在「图即代码」的同时能自己决定位置。**atombender** 补充，D2 的布局引擎 TALA 已经开源，支持按容器控制方向、指定哪些形状要彼此靠近，效果比 Mermaid 好得多。作者认为 TALA 本质上还是自动布局加一些「引导」手段，精神上更接近 Mermaid，对他想画的图来说控制力仍不够。**tonnydourado** 说 Graphviz 的 rank 和端口、PlantUML 的相对位置都只是给布局引擎的提示，不是确定性声明，他为了让 PlantUML 按想要的方向布局浪费的时间，足够让他离退休更近一些。

**为什么不直接给 Mermaid 加功能？** **wrs** 这样问。作者认为在自动布局引擎上叠加位置约束是个很难的问题，若做成 Mermaid 的一种独立图类型，实际上也就是一门新语言了。

**对 agent 的意义。** **aktenlage** 说让 Copilot 写 .dot 图时，AI 遇到的摆放问题和人工写一模一样。作者解释，用 .dot 时 agent 不知道渲染结果长什么样，只能「渲染、看、微调」循环。**apinstein** 认为在 AI 编程时代这很有必要，图是对齐人和 agent 心智模型的高带宽手段。**Towaway69** 则提出不同看法：与其在文本和图像之间来回转换，不如直接用 UML 之类的可视化语言和 agent 沟通。**fallingmeat** 说现在让前沿模型直接画 SVG 已经很好用了，**kinduff** 也说 agent 往往直接写 SVG。作者的看法是，复杂起来 SVG 就会乱，与其让 agent 烧 token 去算坐标，不如让工具负责算术。

**功能建议与快速迭代。** 评论区几乎成了需求收集现场：**Jailbird** 想要直角连线，作者当天就加上了，还附带实线/虚线/点线和粗细设置；**patriciobcs** 想要主题，作者很快推送了一批亮色和暗色主题，**rramon** 和 **s1mon** 报告的主题切换后文字不可读的问题也被修复；**recroad** 发现一条从左侧连到右侧的边没有正确弯曲，作者确认是 bug 并修好。**threecheese** 建议输出布局后的绝对坐标 JSON，这样就能接各种渲染后端，作者说逻辑已经有了，只差 CLI 开关。**Garlef** 希望把拓扑（连线、分组）和布局分离，作为 C4 架构模型的布局层，他们公司生成过一个全公司的 C4 模型，信息很全但画面不好看。**whizzter** 提议从 CSV/JSON 数据源批量生成节点，比如服务器拓扑图。**tamimio** 想要和 draw.io 互转，作者说 Reladraw 转 draw.io 容易，反过来要把绝对位置推断成相对关系，只能尽力而为。

**语法设计的批评。** **tonnydourado** 写了一条详细反馈：赞赏用一套通用语法而不是像 Mermaid、PlantUML 那样每种图一套，但这既是优点也是缺点，那些专门语法正是因为通用的 DOT 不够用才演化出来的，建议加入复用机制（例如画时序图时只定义一次参与者泳道）；他还强烈反对「不允许多行语句」的设计，以及用常见的 `/` 作换行符，认为长标签只能写在一行会严重影响体验。作者表示这些只是早期为了简单而采用的原则，并不执着。从当前的语法文档看，续行和带空格才生效的斜杠换行规则都已经实现。

**生态与分发。** **einpoklum** 希望依赖更少，作者说本身零运行时依赖，只是需要 Node。有人建议用 scriptc 编译成二进制，作者试了不行，用 Deno 可以，但可执行文件有 70 到 105MB。**_kidlike** 问能否像 Mermaid 那样在 Markdown 里用代码块直接渲染，作者说 GitHub 对 Mermaid 是原生内置支持，Reladraw 暂时只能导出 SVG 再引用，IDE 和 Obsidian 则可以通过插件实现。**picardo** 随即做了一个 VS Code 扩展，可以在 Markdown 中渲染并高亮语法。

**关于 AI 生成的争议。** **boblehest** 说 README 看起来是 LLM 生成的，他就此打住。作者回应已重写 README。**Kwpolska** 接着说「现在把剩下的代码也重写吧」，并质疑一个看起来完全由 Claude 写的项目是否配得上 Show HN；**oa335** 等人反问，它有用又有趣，为什么非得不用 LLM 重写？这段支线争论最终没有结论。
