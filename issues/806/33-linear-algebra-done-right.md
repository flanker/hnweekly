---
layout: article
title: "《线性代数应该这样学》（Linear Algebra Done Right）"
issue: 806
number: 33
category: books
original_url: "https://linear.axler.net/"
hn_url: "https://news.ycombinator.com/item?id=49326816"
date: 2026-08-21
---

## 文章摘要

这次上榜的链接是 Sheldon Axler 教材《Linear Algebra Done Right》的官方站点 linear.axler.net。最重要的信息是：**这本畅销线性代数教材的第四版已经是开放获取（Open Access）图书，采用 Creative Commons BY-NC 许可，可以完全合法免费下载**。站点上目前提供的语言包括英文、中文、波斯语、希腊语和葡萄牙语，并注明其他语言正在准备。具体的电子版链接与日期为：英文第四版 PDF（免费，2026 年 8 月 16 日更新）、英文第四版 Kindle 版（免费，2024 年）、中文第四版 PDF（由 Oliver Wu 和 Yang He 翻译，免费，2026 年 6 月 1 日）、波斯语第四版 PDF（由 Alireza Takrimi、Rabert Khamounejad 和 Illatra Khamounejad 翻译，免费，2026 年 6 月 20 日）。印刷版由 Springer 出版，精装版可在亚马逊购买；希腊语和葡萄牙语的第四版印刷本可在相应国家的书商处买到；第三版（2015 年）的巴斯克语和中文印刷译本也仍在亚马逊上销售。

关于第四版的改动，站点给出的数字是：**新增了 250 多道习题和 70 多个新例子**，另有若干新主题以及全书范围内的多处改进；主要改进与新增内容的完整清单在英文版 PDF 的第 xvi 页。

这本书最著名的特点，也是它书名里那个「Done Right」的全部含义，就是**把行列式（determinants）放逐到全书末尾**。站点的介绍写道：本书面向数学专业本科生和研究生的第二门线性代数课程，这种新颖处理聚焦于线性代数的核心目标——**理解有限维向量空间上线性算子的结构**；作者在动机说明和证明简化上下了非同寻常的功夫。除了通常意义上要求的数学成熟度，本书不假定任何先修知识：从向量空间、线性无关、张成、基和维数讲起，然后处理线性映射、特征值与特征向量，接着引入内积空间，导向有限维谱定理及其推论（例如奇异值分解 SVD），再用广义特征向量来揭示线性算子的结构；**行列式最后才通过交替多重线性形式（alternating multilinear forms）被干净地引入**。

站点还披露了这本书的思想来源：它部分基于 Axler 发表在《American Mathematical Monthly》上的论文《Down with Determinants!》（打倒行列式！），该文获得了美国数学协会（MAA）颁发的 Lester R. Ford 说明性写作奖。

页面上摘录了大量书评。《zbMATH》称「总的来说，这个文本是一部教学法上的杰作」；《Choice》的评价最长也最切题：Axler 把行列式（在有限维情形下通常是相当核心的技巧，尽管在无限维中处于边缘）降格为次要角色，「如此一贯地不用行列式，是一次服务于简洁与清晰的高难度技艺展示；Axler 文笔的整体精确性同样服务于此……这是多年来出现的最有原创性的线性代数书，它当然属于每一个本科图书馆」；《American Mathematical Monthly》称「无行列式的证明优雅而直观」；《Mathematical Reviews》称赞它强调「通过例子达到清晰」，认为文本非常适合课堂习题；《Monatshefte für Mathematik》则说「如果你喜欢之前的版本，你会更喜欢这个新版」。

站点上还有若干实用资源与自夸：**采用本书作为教材的大学与学院名单达 431 所**；配套视频；勘误表；作者的 X（Twitter）账号 @AxlerLinear 用于发布本书更新。作者还写道，《Linear Algebra Done Right》通常是这一层次所有线性代数书中亚马逊销量排名最好的，而其精装本价格通常显著低于竞争对手的精装本。读者的问题或意见可以发到 linear@axler.net。

## HN 评论精华

这条帖子拿到 279 分、116 条评论。讨论几乎完全没有停留在这本书本身，而是变成了一场**大规模的线性代数教材推荐与互喷大会**，主线有两条：一是 Axler 到底该不该作为第一本线性代数书（几乎一致的结论是不该，它是第二门课的书），二是「Done Right」这个书名以及作者对行列式的敌意到底站不站得住脚（分歧很大）。

- **wodenokoto** 的开场帖决定了整个讨论的走向。他说自己前一晚正在找看完 3Blue1Brown 线性代数系列之后（或同时）该读什么，候选是四本：Axler 的《Linear Algebra Done Right》、Sergei Treil 的《Linear Algebra Done Wrong》、Gilbert Strang 的《Introduction to Linear Algebra》、以及 Stephen Boyd 与 Lieven Vandenberghe 的《Introduction to Applied Linear Algebra: Vectors, Matrices, and Least Squares》。这条帖下面挂了几十条回复。
- **dash2** 给出了被反复引用的定位：Strang 更简单更清晰，Axler 因为不把内容绑到矩阵上而更进阶；「Strang 是一本『第一门课』的书，Axler 是第二门课的书」。**SpiralSource** 补充说 Axler「以吝惜矩阵著称」。**ivankra** 的建议是：如果你喜欢 3B1B、偏好直觉和应用，那绝对是 Strang 而不是 Axler，并特别推荐 Strang 更新的教材《Linear Algebra and Learning from Data》；Axler 更像纯数教材。**porridgeraisin** 从另一个角度说明：教材的本性就是每一本适合某一类读者，取决于读者此前学会的学习方式；Axler 和 Treil（后者程度更甚）着力于呈现抽象的优雅和数学专业学生喜欢的那种严格性。
- **aureate** 提供了完全相反的个人经验：「我觉得 Strang 无法卒读，而 Axler 简单明晰。有些人觉得抽象向量空间在没先玩过一堆数字列表和网格之前是怪异且缺乏动机的；我觉得行列式在没先学外代数之前才是怪异且缺乏动机的。我希望 Axler 是我的第一门课。」**epgui** 说他非常喜欢 Axler 的路径，「因为它真的抓住了事物的（抽象）本质。当我真正开始『懂』这个路径时，它让线性代数成了我思考方式的重要部分」。
- **_jcrossley** 以数学教育者身份表态：作为第一门课，他强烈不喜欢 Strang 和 Axler 两者——听说 Strang 的讲座很好，但他的书组织混乱且过重计算；Axler 的书很棒，但封底就明确写着是为第二门课、主要面向数学专业的。他自己教的是 Fraleigh，可惜已绝版，Lay 似乎是不错的现代替代。**selimthegrim** 更直接：「我同意 Strang 很糟」，他任教的系过去用 Fraleigh/Lay。
- **Tomte** 提出了一条技术性批评：Axler 把自己限制在实数域和复数域上的向量空间，「这没问题，但我本来希望有提示说明哪些证明和定理在一般情形下不成立」。**seanhunter** 用原文反驳：作者恰恰做了他想要的事，还在前言里给了教学法上的辩解——书通常用 F 同时表示实数或复数来同步发展两者，如果你和学生更愿意把 F 想成任意域，见 1A 节末的评注；「我在这个层次避免任意域，因为它们引入额外的抽象却不带来任何新的线性代数」。而 1A 节末的评注是说：除了内积空间的章节，你可以把 F 想成任意域；在给定域为 C 的地方，通常也可以用任何其他代数闭域。
- 关于书名和「打倒行列式」，**traes** 是主要批评者：「Done Right」意味着按 Axler「完全主观且不寻常的对行列式的憎恶」来做，「它绝不是在某种确定的、严格的意义上『做对了』；我谈过的大多数数学教授要么强烈不同意这种呈现方式，要么根本没有特别的偏好」。**renyicircle** 附议并给出了一个刻薄的类比：这个「done right」大概是这本书流行的主要原因，「让读者以为自己一直学错了。就像那些标题党视频：『你这辈子叠衣服的方法一直是错的！』」**inigyou** 接梗：「或者『这一个简单技巧让大线性代数恨你』。」**contubernio** 的评价最负面：「被高估且有偏见的书。有很多更好的线性代数文本。他对行列式的论战动机不足、方向错误且令人分心。文笔相当形式化，也不太鼓舞人。覆盖面够用，但仅止于够用。」**LZ_Khan** 更极端：「我真的把这本书扔进了垃圾桶，因为它太密太自负。」
- **qsort** 则为这本书辩护：「什么论战？把行列式定义为满足某些性质的唯一交替多重线性形式非常正常（事实上这是对有限维和无限维向量空间都真正说得通的唯一方式）。在我看来这本书没有任何不寻常之处。」**fn-mote** 给出了他认为的真正流行原因：一是习题极好、有挑战性，真正迫使你把概念组装起来；二是有观点的良好教学法——如果你同意这套哲学（其中包括「行列式不是初学者的工具」），解释就很好。他还指出 LADR 绝不是唯一长期回避行列式的书，Lang 也走类似路线但没那么好消化。**bawis** 从数学专业角度说：对数学专业学生 LADR 远优于 LADW，大多数线代书都用行列式去证明对角化之类的进阶内容，而 LADR 的特别之处正是**有意推迟引入行列式**，以便真正证明（因而理解）对角化这类概念背后的目标。
- 关于行列式的直觉，**ak_111** 的论述最完整：行列式易用但很难直观把握，这不是少数派观点，看看 StackOverflow 上无数在求概念性解释的问题就知道；最容易上手的概念抓手是几何的——体积膨胀——但要看清它如何与「对所有排列求和」的组合定义相关联，或者这两种视角如何与代数视角（一组方程有无解）相关联，即使在二维情形也不容易。**traes** 反驳说体积解释是「现存数学中最直观的东西之一」：不可逆变换对应体积为零，因为该变换必然把两个维度压到一起，使它们无法区分；det(AB) = det(A)det(B) 因为连续施加两个变换就是连续施加它们的缩放；det(A⁻¹) = 1/det(A) 因为反转变换就得撤销缩放；他认为排列定义甚至不是严格必需的，Linear Algebra Done Wrong 就是从几何定义出发、再从必须满足的性质推出公式的。**abecedarius** 说问题往往在于行列式被糟糕地引入了，他记得 Apostol 的《Calculus》的处理是：「一个体积倍数会非常有用；为了线性它必须是带符号的体积，这蕴含反对称性。这里是收集这些要求的公理。它们被行列式唯一地满足。证明如下……」**impossiblefork** 则指出这套公理化证明的漏洞：它并没有证明行列式真的就是那个带符号的体积倍数，「你需要真正证明行列式是 N 维平行体的体积，而公理化证明没有做到这一点」；这正是他大一时对行列式感到不满的原因。
- 教材推荐清单在评论区被扩充得极长。被点名的还有：**qsort** 抱怨「你们 Z 世代列线性代数书单居然不提 Lang？滚出我的草坪 ;)」；**impossiblefork** 推荐 Lorenzo Sadun 的《Linear Algebra: The Decoupling Principle》（补上行列式部分就够了）；**geokon** 推荐 Carl Meyer 的《Matrix Analysis and Applied Linear Algebra》，称它「既简洁又全面」，**pdhborges** 立刻反问「991 页你管这叫简洁？」，**jdreaver** 则建议买第一版并贴出一篇关于第二版的书评（第二版是完全重写），**fn-mote** 感谢这个链接：「普通读者绝不会想到一版到二版会是彻底重写。」**laichzeit0** 推荐 Friedberg、Insel 和 Spence（FIS），「非常清晰、记号现代、习题极好」，并指出这也是陶哲轩在 UCLA 115A 课上用的教材；**richard_chase** 力挺 David Lay 的《Linear Algebra and Its Applications》，「最好的入门线性代数教材，没有之一」。围绕 Lay，**tptacek** 提问说他把 Strang 从头到尾读了几遍、Axler 消化了大概 20%（遇到问题集里的困惑就跳进 LADR 的随机章节），很好奇为什么 Lay 的口碑这么好；**BeetleB** 的回答很坦白：「如果你已经学完 Strang，从 Lay 那里得不到任何新东西。它是本基础教材，不涉及任何进阶内容。它只是本好教材。」**derangedHorse** 的回答不同：Lay 的特别之处在于清晰的解释和它引入的用例——从化学计量学到 SVD 算法的一系列问题，不仅提供了理解概念的语境，也让数学与现实世界相关，「激励读者去寻找更多能用这套工具解决的问题」。
- 其他被推荐的还有：Jim Hefferon 的《Linear Algebra》（免费在线，多人称赞）、《No Bullshit Guide to Linear Algebra》（作者 **ivansavz** 本人现身贴出 PDF 预览和可打印的概念图，并谈了他做家教时如何用空白概念图让学生边上课边填、最后让学生用自己的话解释每一个「箭头」来揭示误解）、《The dark art of linear algebra》、Finkbeiner 的《Introduction to Linear Transformations and Matrices》（Dover 廉价重印，先讲线性变换理论再展示矩阵只是选定基之后的编码方式，「感觉去掉了矩阵的魔法」）、Winitzki 的《Linear Algebra via Exterior Products》、Kostrikin 与 Manin 的《Linear Algebra and Geometry》、Nicholson 的《Elementary Linear Algebra》、Shilov（因为是 Dover 所以「又好又极便宜」）、Mike Cohen 的《Linear Algebra: Theory, Intuition, Code》，以及 **j2kun** 自荐的 pimbook.org。**sn9** 推荐 Math Academy 的线性代数课程作为第一次接触，理由是它会诊断你所有的弱点（包括你可能缺失的先修知识），并利用间隔重复和主题关系图处理所有排程，「你只需要每天出现并做 30 分钟以上的功课」。
- **thelaxiankey** 给出了整场讨论里最好的元层面总结：线性代数的问题在于它并不像微积分那样是一门内聚的学科，「这就是为什么你会得到和评论者数量一样多的视角」。他把核心视角列为四种——矩阵/向量作为数字网格与计算工具；作为位置及其变换；作为更抽象的几何对象；作为代数对象——并断言「不存在『done right』」：LADW 大概最适合有动力的荣誉课程数学生（因为不像 Axler，作者不知为何并不憎恨行列式），Strang 适合工程师，而 Axler 主要关心后两种视角，「在我看来这让他相当小众」。
- 全帖最高赞的玩笑来自 **emil-lp** 与 **h_mirin** 的接力。emil-lp 列出这本书在 HN 上的历史：2023 年 7 月 58 分 4 评论、2023 年 10 月「第四版」631 分 294 评论、2024 年 9 月 85 分 39 评论。h_mirin 接道：「之前在我的 ~/Downloads 里：linear_algebra_done_right.pdf，已读 0 页，2023 年 7 月；linear_algebra_done_right (1).pdf，已读 0 页，2023 年 10 月；linear_algebra_done_right (2).pdf，已读 0 页，2024 年 9 月。正在下载 (3)。」**nvlled** 认领了这种心情：「很高兴在这里看到同为电子书囤积者的人。我总有那种喜鹊冲动，看到有趣的书就下载，尽管可能永远不会读。」
- 一些零散的实用信息与吐槽：**Tomte** 警告「别买最新版，排版和版式糟透了」；**netfortius** 报告 Kindle 格式链接 404；**inigyou** 报告下载遇到「Human verification failed」，链接坏了；**ivansavz** 说他借助读书会做完了一半习题，「很难，但很能引发思考，所以我绝对推荐；卡住时别灰心，也尽量别马上看答案」；**maxwell_smart** 提到第 196 页有一首由 ChatGPT 写的、关于柯西—施瓦茨不等式的莎士比亚式十四行诗；**dhruv3006** 说「我这门课能过全靠这本书这么好」；**ouz-a** 声称「这是很多游戏开发者的圣经」，**ksd482** 表示怀疑：「真的吗？为什么？它重理论和证明啊。」**dominotw** 抛出一个引战观点：「在 AI/ML 里你只需要大概 5 个简单概念就能理解线性代数，用 ChatGPT 一周就能学会……很多人以为必须先上完长课程才能开始碰 AI，这正是很多人还没入门就退出的原因。」**lokimedes** 的批评则是元层面的：「我的锦囊比你的锦囊好。行吧。」他认为和大多数教材一样，这本书没能说明为什么读它值得投入，「严格先于价值——是的，这是我的一个心结 :)」。
