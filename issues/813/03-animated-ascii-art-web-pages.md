---
layout: article
title: "为网页准备的动态 ASCII 艺术"
issue: 813
number: 3
category: favorites
original_url: "https://ascii.rest/"
hn_url: "https://news.ycombinator.com/item?id=49993857"
date: 2026-10-09
---

## 文章摘要

ascii.rest 是开发者 Shubham（GitHub/HN 账号 @bas3line）做的一个免费开源动态 ASCII 艺术库，采用 MIT 协议，可以在任何免费或付费项目中使用和修改。目前收录了 217 个动画，项目里称为「pieces」，分为 15 个类别：

- **场景（scenes）**：15 幅全彩动态风景，如夜晚海岸的灯塔、东京雨夜、京都黄昏的五重塔、恒河畔的晚祷、孟买海滨大道的夜景、峡湾极光等；
- **UI 组件**：开机日志、各种边框样式、日历、数字时钟、文件树、表单控件、404 页、进度条、骨架屏、加载动画、终端；
- **数据可视化**：柱状图、K 线、CPU 仪表、均衡器、热力图、雷达图、迷你折线图、可用性条等；
- **文字特效**：打字机、翻牌式显示板、跑马灯、故障风、摩尔斯电码、波浪字等，都可以换成自己的文字；
- **Logo**：29 个编程语言 logo、21 个公司或产品 logo（如 Vercel、Cloudflare、Supabase）、23 个 Linux 发行版 logo；
- **其他**：3D 几何体（经典的旋转甜甜圈、超立方体、DNA 双螺旋）、太空（黑洞、三体）、物理模拟（双摆、落沙、牛顿摆、洛伦兹吸引子）、自然（篝火、盆景、极光）、动物、日常物件、生成艺术（曼德博集合、迷宫、生命游戏、Voronoi 图），以及 Doom 火焰、黑客帝国字符雨、合成波等经典 demo 特效。

**接入方式。** 每个 piece 都是一个零依赖、带类型定义的 TypeScript 小模块。最简单的用法是在 HTML 页面里引入一个 script 标签，再写一个 `<ascii-art piece="donut">` 自定义标签，不需要安装和构建。React/Next.js 可用 `ascii.rest/react` 中的 `<Ascii>` 组件（已经是客户端组件）；Astro 组件会在服务端渲染第一帧，保证脚本执行前页面就是完整的；也可以用 `mount()` 把动画挂到任意 `<pre>` 或 `<canvas>` 上。终端里运行 `npx ascii.rest donut` 即可播放，也可以作为 CLI 的启动画面。此外还有横幅生成器（把文字变成方块字，可用于 README）、「图片转 ASCII」工具、导出 SVG（用于不能运行脚本的 GitHub README），以及一个 shadcn 组件仓库。

**两种渲染方式。** 129 个「文本型」piece 用页面本身的字体和颜色画在 `<pre>` 中；场景、logo 等 88 个「彩色型」piece 画在 `<canvas>` 上。动画只在元素可见、标签页处于激活状态时播放；对于系统设置了「减少动态效果」的用户，只显示静止的第一帧。

**自制 piece 的工具包。** 文档中有一套「kit」，可以用一行代码生成新动画，例如 `sea({ palette: "ocean" })`、`spinning(torus())`、`clockFace()`，选项用 `speed: "slow"` 这样的词语描述；需要更多控制时，可以一路深入到数学场、3D 场景、粒子系统、特效和 SVG 绘制。站点还专门为 AI 编程代理准备了 `llms.txt` 和 `llms-full.txt`，并列出一份给代理的使用规则，比如颜色必须是六位十六进制、彩色 piece 只能设宽度不能设高度等。

## HN 评论精华

帖子获得 466 分、75 条评论。整体反响是惊艳，作者 bas3line 也在帖子里积极回复、当场修 bug。另一条主线是「这还算不算 ASCII 艺术」的定义之争，少数人还质疑作品是 AI 生成、制作粗糙。

**「怎么没在动？」** kirth_gersen 等多人反映看不到动画。crisnoble 指出原因是站点尊重系统的「减少动态效果」设置；作者随后加了「play anyway」按钮。BugsJustFindMe 认为这还不够，用户自己往往不知道开了这个设置，应该在显眼位置提示。mapshog 说 Mac 上三个浏览器都不播放动画，作者修复后他回复「修好了，谢谢」。ZeWaka 指出 spinners 页面在宽度变化时「愤怒地抖动」，cromka 说在 Android 版 Brave 上一半动画的行序是乱的。

**这算 ASCII 艺术吗？** 这是讨论最多的话题。boguscoder 指出那些大小不同的圆点并不是 ASCII 字符，更像「Unicode 艺术」；elevaet 回应说也有真正只用 ASCII 的作品，比如 julia-set 页面。gavmor 的评论被多人点赞：作品很美，但它是在「借用一种受限媒介的可信度，却不接受它的限制」，好比把特斯拉改装成雪铁龙 2CV 的样子。graypegg 附和说，由于对比度主要靠颜色，它更像一个把画面量化成小圆点的着色器。wuliwong 反问：不叫 ASCII 艺术还能叫什么？换个陌生的名字，根本不会有人点进来。ghurtado 则批评这等于为了推广而歪曲事物本身。ape4、moritzwarhier 指出页面上很多内容实际是 canvas 渲染，不是文本。5-0 给年轻读者贴了 asciiarena.se，展示「过去 ASCII 艺术的意思」。

**关于质量和 AI 的质疑。** sosodev 认为它既不是 ASCII，很多作品看起来也不对劲、动画有问题，「我不认为所有 AI 艺术都是垃圾，但这里花的功夫太少了，为什么这么多人在夸？」katzenq 附和说，夸一个显然没做什么工作的人「干得漂亮」很奇怪。这类声音是少数，但确实存在。

**技术与趋势。** karim79 问这些场景是怎么从照片生成的。zeroq 解释了基本原理：把图片缩放到字符网格大小，按每个像素的亮度换成笔画多少不同的字符；动画部分则让人联想到老式的「调色板循环」技术。marinhero 问为什么 ASCII 风格现在又流行起来了。moritzwarhier 认为，文本艺术借用了「精心手工制作」的外观，实际却是程序生成的；如今 CLI 和纯文本又受欢迎，聊天界面本身就像 CLI，Markdown 也在取代二进制 Office 文件。frabia 认为这是当下 UI 设计的潮流：用这种视觉风格向产品投射「慢工细活」的价值。

**相关作品与怀旧。** robbomacrae 分享了自己为 Claude Code / Pi 做的 ASCII 工作状态动画 flow，conesus 表示想给它贡献 PR。smusamashah 推荐了「真正的 ASCII」动画库 play.ertdfgcvb.xyz。bredren 回忆当年在拨号 BBS 上花整天做动画 ASCII 序列。otistropo 受启发用纯 C 实现了一个类似的屏保。davidcollantes 希望有创作工具，作者表示会考虑。burmask 则说想买个数码相框把这些自然场景当墙上装饰。
