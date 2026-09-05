---
layout: article
title: "Ambient CSS v3——当 Blender 遇上 CSS"
issue: 808
number: 33
category: design
original_url: "https://ambientcss.vercel.app/"
hn_url: "https://news.ycombinator.com/item?id=49523387"
date: 2026-09-04
---

## 文章摘要

Ambient CSS 是一套**基于物理光照原理的 CSS 系统**。作者是 GitHub 用户 kikkupico，他也出现在 HN 评论区回应质疑。提交到 HN 的链接是一个部署在 Vercel 上的交互式演示站（一个模拟硬件合成器/音频设备的界面），页面本身是客户端渲染的，几乎没有可抓取的正文；以下内容主要来自项目 GitHub README 与文档站。

**核心主张**：传统 CSS 的阴影层级是纯装饰性的——一张用 `shadow-lg` 的卡片挨着一个用 `shadow-sm` 的按钮，并不意味着它们处在同一个物理场景中，它们只是两个恰好共存于同一页面的、互不相关的模糊值。

Ambient CSS 的做法反过来：你描述一个**光照环境**（光线方向、主光/补光强度、色相、以及元素的物理高度），然后所有阴影、边缘高光和表面渐变都从中确定性地推导出来。改变某个容器上的光线向量，它的所有子元素会一致地更新。

**实现机制**：引擎把 UI 表面建模为双光源（key light + fill light）系统下的物理材质。每个元素读取共享的 CSS 自定义属性，生成一个由五层合成的 `box-shadow`——投影（按物理高度缩放的方向性本影与半影）、圆角高光（朝向光源的镜面内边缘高光）、圆角阴影（背光侧的柔和内阴影）、倒角高光（沿受光斜面的宽幅内发光）、倒角阴影（背光侧的深色斜面阴影）。

每个元素组合五个核心材质关注点：结构（`ambient`，启用光照计算）、表面（flat / concave / convex，决定背景光照渐变）、边缘（chamfer / fillet / groove，倒角与圆角切法）、材质（matte / shiny / glass，镜面反射与半透明）、深度（elevation 0–3，投影高度）。

**这就是标题里 Blender 的由来**：光照行为不是靠手感调出来的，而是**对照用 Blender 建的光追 3D 参考模型标定过的**。仓库里有一个名为 `ambient3d` 的模块，是参数化的 Blender 3D 组件套件兼「地面真值」光追标定引擎。README 顶部的动图就是同一个设备的两半：一边是 Blender 里 Cycles 光追渲染的，另一边是同样几何、同样光向、但用 `box-shadow` 实现的实时 DOM。

**API 形态**：纯 CSS 包 `@ambientcss/css` 零依赖，用类名如 `.amb-light-tl` 在任意祖先元素上设定光向（由后代继承），元素上叠加 `ambient amb-surface amb-chamfer amb-elevation-1 amb-rounded` 之类的类；也可以用自定义属性精细控制 `--amb-light-x`、`--amb-light-y`、`--amb-key-light-intensity`、`--amb-fill-light-intensity`、`--amb-light-hue`、`--amb-light-saturation`。表面被定义为「光照下的材质」而非固定颜色，所以 `--amb-albedo`（全照明下的颜色）加 `--amb-shade`（反射率乘数）一套类就能覆盖所有色相和明度。React 包 `@ambientcss/components` 提供 `AmbientProvider`、`AmbientButton`、`AmbientKnob`、`AmbientPanel` 等组件；每个组件是一个「预设」——机制层负责运动学、数值和 ARIA，绘制层可以整个替换，控件会在自身根节点上发布 `--ambx-percent`、`--ambx-angle`、`--ambx-size`，所以自定义部件可以是纯 CSS 的。

Monorepo 还包含 Docusaurus 文档站和那个硬件合成器演示应用。帖子拿到 305 分、98 条评论，讨论极为分裂。

## HN 评论精华

**「这不就是拟物化/新拟态吗」——命名之争**

这是评论区最大的一条线。`troupo` 直接问：这不就是几年前昙花一现的 neumorphic 设计吗（并贴出 neumorphism.io）？`smallnix` 的辩护是：它把设计系统锚定在真实世界物体上，因而走得更远；`woodrowbarlow` 补刀：「所以，就是拟物化（skeuomorphic）。」`Quitschquat` 感叹「UI/UX 为什么非得是时尚，我就知道这种拟物玩意儿早晚会回来」，而 `Y-bar` 立刻反驳：「我一直在呼唤这种可用的、一眼就懂的界面元素回归！」`dvt` 则是纯粹的欢呼：「拟物化回来了宝贝！」

`alexaholic` 提供了历史视角：Web 2.0 时代大家用 PNG、GIF 和 `progid:DXImageTransform` 干这事，因为当时纯 CSS 做不到；等 CSS（和微软）演进到能做了，世界却转向扁平化了，这不是很讽刺吗。他和 `dleeftink` 顺势追忆了 Flash 时代的定制界面，`alexaholic` 给出了那条被多次引用的路线图：「从定制到 Bootstrap，再到 Tailwind 化」，`phoronixrly` 接了一句「……再到 Claude 化，这整个项目完全是 vibecode 出来的」。`mcdonje` 的解释更冷静：当年有很强的互相较劲成分，人们做是因为难；当它不再难，人们就停了，因为那本来也不是好的干净设计。`thesuitonym` 补充：「可惜他们并没有用好的干净设计取代它。」

Blender 这个词本身也遭到大量质疑。`stevage`、`voidUpdate`、`MetroWind`、`Saltloaf` 都在问这跟 Blender 到底什么关系。`dnpls` 怀疑「提 Blender 大概只是为了 SEO，或者是 GPT 建议的」。`caxco93` 要求把标题里的 Blender 去掉：「这至少可以说是误导，要说也该说 PBR 之类的词。」`ricardobeat` 帮忙澄清了这确实指的是基于 Blender 渲染做标定，作者 `kikkupico` 也贴出了 `ambient3d` 的仓库链接。

**作者亲自下场，也遭遇了最尖锐的批评**

`graypegg` 提了个技术上的挑剔：如果假定一个基准平面并设置透视，然后沿 Z 轴 `translate3d`，elevation 会更合理，元素还会随之变大；他觉得「弹跳」示例里只有背景在动很奇怪。作者 `kikkupico` 的回应解释了设计取舍：**这里刻意选了正交投影**，所以 elevation 不改变物体大小但改变阴影；透视投影在滚动时表现不好，因为用户滚动时物体的角度必须随之微调，而正交投影不需要处理这个。他还贴出了文档里用 three.js 实际搭建 3D 场景来解释光照和相机设置的页面。

但 `ricardobeat` 的反驳非常不客气：「抱歉，但那就是 slop。实际 CSS 里的 elevation 让阴影逐级变淡（技术上说得通，但比例看起来很不对，2 太深了），而 3D 模型里的阴影是恒定的。这个所谓的『相机和光照设置』里没有任何东西被翻译成了样式。」`kikkupico` 回复说文档里那个 three.js 场景只用于解释设置，它没有光追器来准确渲染阴影，CSS 的参考渲染是在 Blender 里做的，并给出了对照页链接。`rf15` 看了对照页后说：「有些偏差莫名其妙地大得离谱。」`ricardobeat` 追加了本串最狠的一句：「确实。那些甚至偏得更多。感觉他们根本没看 AI 产出了什么——或者写这些回复的也是那个 agent。」

`DrinkerOfBeers` 倒是站在 graypegg 那边：滚动时角度微调「其实会挺酷的」，只是用 CSS hack 做可能太吃性能。

**「旋钮是个坏主意」——最一致的差评**

关于可用性，评论区罕见地形成了共识：**这些控件不好用**。`fwip` 详细描述：前两个旋钮只响应点击加垂直拖动，向左右移动毫无反应；第二个旋钮的抓取手柄完全错位，只出现在 4 点到 6 点方向。`charlesrice` 说得最生动：「我最喜欢的是有些旋钮你能像真实物体一样抓住旋转，另一些只对光标的垂直位置有反应，于是你用起来感觉自己像个疯子。」

`SwellJoe` 提供了最系统的批评，而且是在他明确表示喜欢立体感和明确可供性（affordance）的前提下：圆形旋钮从来就不适合鼠标或触摸板，还常常是可访问性灾难；VST 界面之所以是最差的模仿对象，是因为它们模仿的历史硬件受限于当年的技术和成本，不得不把功能藏进「神秘肉」式导航里，而这些界面的优先级往往是在截图里好看，而不是好懂好发现。`JackFr` 一句话总结：「旋钮在任何鼠标/指针界面里都是无用的控件。」`nekooooo` 对另一个类似项目的评价同样适用：「点击拖动，尤其是沿着一条看不见的曲线拖动，不是用户友好的。」

作者 `kikkupico` 的回应很坦诚：他发现是组件区里一个早期版本残留的 div 覆盖在旋钮上，已经修好了，在桌面和 Android Chrome 上验证过，还没测 iOS Safari；然后他加了一句「**P.S. 我同意旋钮控件整体上就是糟糕的 UX。真实旋钮通常是用两根手指转的，屏幕上旋钮的操作方式跟那完全不一样**」。

`wasmperson` 给出了最专业的无障碍批评：单选按钮的键盘控制不应该取决于按钮的视觉朝向，浏览器默认就做对了这件事，但这里的单选按钮不是用 `<input type="radio">` 实现的，而是一排普通 button 加一堆 JavaScript；他在开发者工具里快速检查后发现其他控件也是类似的实现方式——「不过我很喜欢它的外观」。

移动端和性能也是重灾区：`isodev`（iOS Safari）、`esperent`（Android Brave）、`drfloyd51`、`dkersten`（iOS 上闪烁）、`som`、`voidUpdate`（加载新区块时页面卡顿）都报告了问题。`inexcf` 因为滚动行为直接关掉了页面：它允许你滚动到下一「页」的一半然后强制你停下、重新开始滚动才能继续。`SwellJoe` 由此上升到原则：「当一个讲新 GUI 玩意儿的页面去乱搞滚动、后退按钮、控件交互这些定义明确的行为时，我就知道我不能信任他们的判断力。」`cluckindan` 猜是 scroll-snap，`inexcf` 说不是。

`mminer237` 给出了最长的逐条差评：光线方向莫名其妙地统辖整个网格却在某个任意 div 外停止、有时干脆失效；通道搞乱了；颜色选项和纹理有点丑，玻璃色明显不成立；「Brass」在最后一项莫名变成「Olive」；单选按钮实现得完全不一致，中间那个根本不工作；按钮按下与否几乎没有反馈。他的总结是：让他想起早期的 Material Design，只是更粗糙、更难用、更不鲜艳。`danudey` 补充了一个根本性问题：它支持暗色主题，但**在暗背景上投影根本不起作用**，所以你看不到它 90% 的效果。

**跑题最远也最热的一支：AI 生成的 UI 长什么样**

`CSSer` 的评论意外地成了本帖最有影响力的发言：他非常不喜欢 dribbble/envato 那种风格「逃逸进了 Claude，现在到处都能看到它，还混着平庸的布局」；他本来期待这类东西能起到抵消作用，结果看到它被用在 12px 字号上（那个 3D Kubernetes 面板）时感到恐惧。他的 PSA 是：**Claude 开箱即用并不理解 UX 或好的设计原则，请去抱一抱设计师**。

`graypegg` 由此贡献了本帖被引用最多的一个概念：**UI greebling**（greeble 指科幻模型上为增加视觉复杂度而贴的无意义细节）。他说人们总忽略一点——过去那些信息密集的 UI 是**真的信息密集**；而现在我们得到的是信息密度的拟像，LLM 给每个 pod 都配上一台机器、给 pod 编出一个带瓦特数的配电单元、到处撒随机状态标签，「就差每个 Claude 应用都有的那个绿点 LIVE 标签了」。`ipdashc` 觉得 greebling 这个比喻绝妙，并认为「每个 pod 都有一台机器」这种编造出来的无意义信息块是最糟糕的，猜测这来自在演示程序上训练的习惯。`bargainbin` 补了一个具体现象：他是开始用 Claude 生成设计后才知道「eyebrow」（小标题上方的短标签）这个词的——「那模型对 eyebrow 简直着魔，输出的每一节都有」。

`dmix` 建议让 Claude 参考 Apple HIG 然后再评审界面，`wlesieutre` 反问：「有人试过给它 20 年前的 Apple HIG 吗？」`theturtle32` 大力赞同：那是 UI 设计的黄金时代，触摸界面带来的狂野实验让我们损失了太多；**跨应用跨厂商的强共享设计语言，是一种价值无法估量的生产力增益，它让人们能把在一个应用里学到的直觉迁移到从未见过的界面上**。`wlesieutre` 随后引用了老版 HIG 里关于一致性的几个自查问题（是否符合系统标准、自身内部是否一致、是否与产品早期版本一致、是否符合用户预期），并在 `dmix` 指出 2026 版 HIG 也讲一致性后，退一步抱怨：那份大 PDF 变成链接式网页文档后连个全文搜索框都没有，「比 20 年前按 cmd-F 还差」。

`slj` 给出了这条线索里最有分量的一句：做拟物化不需要更强的模型或 LLM 流程，拟物化一直都做得到，它需要的是艰苦的工作和专注的设计努力。现在用 LLM 做任何 UI/UX 设计都比以往容易，缺的那一环是工作、是努力；**当你想做拟物设计时，不要把最难的部分交给 LLM，你应该自己去推敲每一个元素的意图和用心——那才是当年拟物设计好看的原因**。`CSSer` 被问到怎样才算做对时给了两条可操作建议：批判性地审视它生成的每一条内容，问自己这条信息对最终用户有什么用（过去往界面里加噪音是有成本的，所以不容易做错）；以及排版——16px 以下的文字必须非常严肃地对待，而 Claude 到处乱撒小字。

**同好与自荐**

不少人认出了这种风格的来源。`drcongo` 说在 iPad 的 AUv3 插件里经常见到这种外观，「有视觉触感的界面比操作系统强加给我们的扁平屎好太多了」。`tuvix` 和 `QuantumNomad_` 想到了 VST 界面，后者甚至提议把它用在 Ableton Live 新的 JavaScript 扩展 SDK 里。`__alexs` 说这有 2000 年代中期 LiteStep 主题的味道，`yboris` 回了句「LiteStep 永在我们心中」。`thenthenthen` 想到的是 Teenage Engineering 的 VST。

自荐环节：`TechSquidTV` 贴出了他尝试但自认失败的 analogui.com；`rahulmax` 是同好，展示了字体工具 typestax.com 并打算用 Ambient CSS 增强界面（`dzonga` 回复说界面很漂亮）；`noduerme` 分享了十年未更新的实验性作品集站点，并说自从 Photoshop 把投影推向世界之后，他一直觉得滥用投影是掩盖糟糕设计的手段；`rrr_oh_man` 插播了他和伴侣为悼念逝去的狗而做的免费日志应用 mydogisthebest.org，在被 `fugaziboutit` 批评「注册前应该先展示价值」时坦率回答：他认同这个观点，但没人用也完全没关系，这首先是创始人的自我疗愈。

`eigenblake` 是少数明确表示要用的人：他为一门课做了受 Duolingo 启发的「曝光三角」演示，觉得这像是挖到金子，想为讲解数学建一套拟物组件库。`BorisMelnik` 说「太屌了，我特别想把它用在一个小企业网站上」。

最后一个小插曲：`irickt` 贴出 GitHub 仓库后，`microflash` 叹气引用了 GitHub 的提示「你屏蔽过的一位用户曾为此仓库做过贡献」；`jdiff` 问「只有 5 个『贡献者』，你是不是屏蔽了在里面乱窜的某个 AI bot？」——`microflash` 确认了。`noncovalence` 留下了本帖最短的挑剔：「小事一桩，但那个字体真的需要一个带斜杠的零。」
