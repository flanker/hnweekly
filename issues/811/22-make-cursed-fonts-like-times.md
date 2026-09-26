---
layout: article
title: "制作像 Times New Bastard 那样的「被诅咒」字体"
issue: 811
number: 22
category: show_hn
original_url: "https://bastardica.mitpit.com"
hn_url: "https://news.ycombinator.com/item?id=49823738"
date: 2026-09-25
---

## 文章摘要

Bastardica 自称"一家生产杂种网页字体的铸字厂"（A foundry for bastard web fonts），口号是"混合、拉伸，和/或挤压它们"。作者 MitPitt 在 Show HN 里只写了两句话：这是一个恶搞工具，滥用 OpenType 的连字（ligature）特性来混合字体；它通过在 WebAssembly 里加载 Python，在浏览器端就能跑得很快。这个帖子拿到了 846 分，是本期 Show HN 里最火的一个。

**灵感来自 Times New Bastard。** 网站页脚写明，Bastardica 受 Times New Bastard 和 Easy Pete 启发。Times New Bastard 是设计师 Wei Huang 多年前做的一款恶搞字体，创意最早来自 Tumblr 用户 ommanyte：它就是 Times New Roman，但"每第七个字母都会突兀地变成无衬线体"。它的实现方式是 OpenType 连字：字体按上下文把特定位置的字形替换成另一套字形。Bastardica 把这个技巧做成了通用工具，你可以用任意两种或多种字体造出自己的"杂种字体"。

**使用方式。** 界面分为"基础字体"和"混入字体"，两边都能从内置列表里选（也有随机骰子按钮），或者上传自己的字体文件。核心参数是"步长"（stride）：每隔多少个字形换成混入字体。混入字体还可以叠加效果，比如缩放、Y 轴偏移、倾斜、平移。高级模式支持加入更多字体。生成后可以下载 TTF，或构建 OTF、WOFF2。FAQ 解释说，每个下载的文件都是标准的 OpenType 字体，替换规则注册为对所有文字系统生效的 liga 上下文替换，所以浏览器默认就会开启；只要是做 OpenType 文本排版的地方（浏览器、设计软件、印刷）都能用。如果字体看起来没变化，检查一下应用里是不是关掉了连字。所有处理都在浏览器本地用 Pyodide 和 fontTools 完成，字体不会上传到任何地方。

**作者的专业小贴士。** Bastardica "能让简单的字体感觉更丰富一点"；用 Y 轴偏移和缩放效果可以让字形完美对齐。混合三种以上字体时它们会相交，比如每第 5 个和每第 7 个会在每第 35 个字形处撞车，此时排在前面的字体优先，但两者的步长都不会被打乱。所以步长最好用质数，这样混入字体撞车的机会更少。关于授权，FAQ 提醒混合两种字体会产生衍生作品，商用前要检查两种源字体的许可证；Bastardica 本身不附加任何条件，署名"感激但非必须"。

**预设字体是笑点的集中地。** 页面上有一组预设，点一下就能加载参数：

- **Sneaky Bastard（阴险杂种）**：Times New Roman，但每第 7 个字形换成 Arial，并缩小到 90% 以匹配大小。
- **Impacter**：说明文字全用大写，"IMPACTER 就是 Impact，但每第 2 个字形被拉伸了"。
- **Nervous Sans（神经质无衬线）**：Arial 叠了五层，按伪随机的方式偏离基线、抽搐倾斜。
- **Cartoon Papyrus（卡通 Papyrus）**：同样的抖动技巧用在 Papyrus 上，"适合网络漫画"。
- **Royal Ransom Note（皇家勒索信）**：Times New Roman、Arial、Comic Sans MS、Impact、Gothic 和 Papyrus 全部混在一起，就像电影里用杂志剪字拼成的勒索信。
- **Temu Sans**：Arial 的各个字形被左右错位，说明是"值得信赖的电子产品的包装美学"。

## HN 评论精华

123 条评论，几乎全是欢乐的气氛：大家在比谁造出的字体更恶心、谁想到的恶作剧更阴险，Temu Sans 这个名字收获了一大片"笑出声"。欢乐之外也有几段严肃的讨论，涉及可访问性、字体版权，以及"展示比说明更重要"。

**Temu Sans 是全场最佳。** philjohn 说"Temu Sans 精准到位"，thecaio、repple、samteeeee、micheljansen 等人都说被这个名字逗笑了。kstrauser 说"这是最近几次少有的让我在 HN 上真的笑出声的时刻。完美无瑕"。thenthenthen 从中国给出了一个真实的解释：直接用"中文字体"打西文也会有这种效果，因为大多数中文字体给中文字形和西文字符都用固定宽度，所以 i、l、f 这些窄字母周围会多出空白，形成"Temu 效果"，"这在中国随处可见"。tuetuopay 说终于明白了为什么东亚国家印刷品里的拉丁文排版总是那样；Sharlin 补刀说"CJK 字体里的拉丁字母部分经常是随手塞进去的劣质 Times 仿制品"；thenthenthen 替设计师说了句话："画五千多个字形可不容易。"jurf 则想起了中欧的童年经历："那时候所有字体都长这样，因为几乎没有字体支持 ďíáčríťíčš 这些字母，文字里到处是回退显示的、格格不入的字符。"

**设计师的邪恶快感。** mortenjorck 的评论获得了热烈回应："作为设计师，我从没像刚才这样觉得自己如此邪恶，我小心翼翼地调整垂直缩放和偏移，让 Papyrus 的 x 字高和基线在视觉上精确匹配 Comic Sans。"大家纷纷要求他分享，danielheath 给出了参数（Y 轴缩放 81%、Y 偏移 2%），Vinnl 贴出截图，Obscurity4340 评价"意外地好读"，Barbing 说"其实一点也不差，人们做过很多就长这样的字体"。coffee-on-thurs 直接把字体文件传了上去，"它会显示为'Papyrus and Comic Sans MS Bastard'。现在的问题是你为什么想要这个"。schiffern 贴出了那个经典的"Papyrus 和 Comic Sans"恶搞视频。

**更阴险的点子层出不穷。** krelas 说："如果你想让一个设计师精神崩溃，就上传一份 Helvetica，然后每隔两三个字符换成 Arial。"FuriouslyAdrift 回："冷静点，撒旦。"ayaros 也提了同样的想法，并自嘲"不是第一个这么说的"。Someone 更进一步：用连字可以做一款 Helvetica，让"Helvetica""beautiful""aesthetics"这几个词以 Arial 显示。dvh 提议做一款看起来和原版一样的字体，只是加上连字，让"red"这个词显示成"green"。dspillett 建议用连字引入错别字或者擅自"纠正"拼写（color→colour）。6thbit 问能不能做一款等宽字体，让每行超过 80 个字符的部分渲染得尽可能难看。Sharlin 希望加入更微妙的"keming"调整（故意把 kerning 写成 keming，这个梗来自字距调整失误让 rn 看起来像 m），让字体看起来"稍微有点不对劲"；他还从 Nervous Sans 的示例文字得到灵感，想出了一个名字："Anal"，一款字距很烂的 Arial。

**可访问性之争。** someonebaggy 由 dvh 的点子引申：可以用在文档上，查出谁在复制粘贴或者用 AI 读文档；amenghra 说还可以拿来做验证码。smeehee 提醒这类做法会给 TTS 等辅助技术带来麻烦。servo_sausage 认为"可访问性只应该是附加项，永远不应该成为限制"，理由是"提议限制某件事，需要比'某个本来就受限的群体用不了'更好的理由"，他称之为"不合时宜的表演式道德主义"。Semkas 反驳："我们每个人一生中都会在某些时候、或长或短地变得'本来就受限'……为理想用户构建东西很容易，软件工程的挑战在于做出适合各种人、适合人生不同阶段的方案。拒绝面对这个挑战，是一种骄傲的平庸。"hypfer 后来也提到想用自定义字体和连字做一种人类好解、LLM 难解的验证码，一位使用屏幕阅读器的用户 ctoth 只回了一句："我用屏幕阅读器。请不要这么做。"

**字体律师的前车之鉴。** tholman 讲了一个真实故事：八年多前他做了一款字距乱调的 Helvetica 恶搞版，叫 Smelvetica，最后收到了律师的来函："我们请您停止宣传 Smelvetica，并停止向所有人提供该字体软件。下次如果您对如何让我们的经典作品现代化有更多想法，请告诉我们，我们一直欢迎创意意见。"cluckindan 也在想把它用进游戏的 ZeWaka 那条下面提醒"注意授权"。

**"展示，别只是说"。** ycombiredd 分享了自己一年多前做的工具 fambigen：它最初是个旋转对称字（ambigram）生成器，后来能组合多种字体的矢量路径，比如把 Arial 的上半部分和 Times New Roman 的下半部分拼在一起。他说当时发到 HN 几乎无人问津，排版社区的反应还偏负面，有人把它说成近乎盗版。amoss 的解释是：他的 README 大段都是文字、示例太少，而这个工具的网站一眼就能看出它是做什么的，"这是'展示而不是讲述'的经典案例"。dostick 说得更尖锐：今天这个工具有一个一看就懂的演示网站，而你的是一个"自己琢磨去吧"的代码仓库。xp84 认为这样说太打击人了，amoss 那种说法才算建设性。ycombiredd 很有风度地接受了："如果我真想把给我带来快乐的东西分享给别人，可能不得不去打磨呈现方式……就像想把一块看起来像大便的巧克力蛋糕分享给陌生人。"preommr 补充说这不是推销，而是一种信号，说明你对产品理解得足够透彻，能讲清它的要点，并引用了"我本想写一封短一点的信，但没有时间"。Abstract_Typist 纠正说这句话通常被归于帕斯卡（1657 年，法语原文），而不是马克·吐温。

**技术角度的讨论。** california-og 指出其中一些效果用 CSS 的 @font-face 加 unicode-range 其实很容易实现。xrd 问能否用纯 CSS 或 JavaScript 达到类似效果。peter_d_sherman 由浏览器端即时生成字体联想到"字体即服务"：把字体渲染放到 Cloudflare Worker 之类的无服务器函数里，既不用在软件里捆绑渲染引擎，又能把本地字体渲染这个攻击面（他认为字体渲染器通常是图灵完备的）移出本机。furyofantares 发现它甚至支持 COLR 和 SVG 彩色字体。NietzscheanNull 报告了 iOS WebKit 上切换混入字体时的 Pyodide 内存越界错误。

**其他笑点。** htrp 说这看起来就像电影里用杂志剪字拼的勒索信；dyingkneepad 下载了"皇家勒索信"，"以后再也不用剪杂志了"，sasaf5 接话"还有涂胶水那部分，我最讨厌了"。qurren 贴图说他的 LinkedIn 界面已经长这样了。cengizIO 说它"让我同时因为厌恶、愤怒、痛苦和接受而哭了出来"，并把 Sneaky Bastard 设成了自己 Obsidian 的默认字体。hackeryojo 宣布今后所有工作邮件都用这些字体写，"（顺便说一句，我是自雇的）"。electric_mayhem 说"这简直是恶魔的造物，不该交到人类手里……看，我们这儿出了下一个奥本海默"。phren0logy 用了网上的经典句式："谢谢，我讨厌它。"
