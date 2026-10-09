---
layout: article
title: "苹果公开的产品尺寸图纸"
issue: 812
number: 30
category: design
original_url: "https://developer.apple.com/accessories/dimensional-drawings/"
hn_url: "https://news.ycombinator.com/item?id=49690174"
date: 2026-10-02
---

## 文章摘要

这次上榜的并不是一篇文章，而是苹果开发者网站"配件"栏目下的一个下载页面，标题是"Dimensional Drawings"（尺寸图纸），一句话说明是"下载尺寸图纸和技术规格"。页面按 Mac、iPad、iPhone、Watch、Vision、AirPods、TV & Home、配件分类，目前提供约 96 份 PDF，每款设备一份，最新的几项标着"New"：AirPods 5、Apple Watch Series 12（42 mm 和 46 mm）、Apple Watch Ultra 4、iPhone 18 Pro 和 iPhone 18 Pro Max。往前一路覆盖到 iPhone 12 系列、iPad 第 8 代、Apple Watch Series 6、AirTag、第二代 Siri 遥控器，还有 Apple Vision Pro 及其音频头带、电池和蔡司光学镜片插件，各代 AirPods 充电盒、MagSafe 充电器等。Mac 一栏只有 MacBook Neo 一款。

**图纸里有什么。** 以 iPhone 18 Pro 的 PDF 为例：共 5 页，首页是使用条款，后面是标准的工程图纸格式。条款大意是：这些指南仅供参考，如果你依据它们制造或分销苹果设备的配件，就要同意一系列约定，包括苹果保留所有知识产权、不授予任何专利或商标许可，必须遵守苹果的商标使用规范，因配件引发的知识产权、产品责任等索赔由你来赔偿苹果，图纸"按现状提供"，苹果对此的总责任上限为 50 美元，争议在旧金山按国际商会规则仲裁。

后几页则是真正的图纸，用 CAD 软件导出的单线笔画字体标注，内容主要是做保护壳和配件时必须避开的区域："相机控制"按键周围的禁入区（keepout），并附注"避免窄边或锐角，以保留此边缘的触感"；后置摄像头、闪光灯和传感器的视锥禁入区，从传感器底座到盖板玻璃的距离；前置摄像头、环境光传感器和听筒的禁入区；下方天线和指南针附近"不得遮挡"的区域，注明这些区域不能出现金属、导电材料或磁性材料，MagSafe 磁铁除外；还有薄壳禁入区等。每页角落都印着工程图纸惯常的"专有财产声明"：持有者同意对本文件保密、不得复制、不得部分或全部披露。这份声明出现在一个人人都能下载的公开页面上，多少有点讽刺。

这些资料的用途很明确：方便第三方厂商设计保护壳、表带、支架、贴膜等配件。HN 评论中有人指出，这些图纸过去是打包在苹果那份越来越臃肿的《配件设计指南》（Accessory Design Guidelines）单个 PDF 里的，这个独立页面大约在 2026 年 5 月才上线，起初只有 13 款产品，此后陆续补充了旧型号。

## HN 评论精华

帖子 421 分、约 130 条评论。讨论有一半在聊图纸本身（为什么公开、怎么用、为什么圆角不标半径），另一半则被一条"苹果自己用 Windows 虚拟机跑 Siemens NX 做 CAD"的评论带偏，演变成关于 Mac 上缺乏专业 CAD 软件、乃至 Win32 API 好坏的长篇争论。

**为什么要公开？** MBCook 的评论排在最前：他完全不知道苹果把这些公开给了所有人，原以为得付钱或加入 MFi（Made for iPhone）认证计划才能拿到。darknavi 说这是为了支撑庞大的第三方配件生态，开放完全值得；p0ckets 和 SoftTalker 都引用了"让你的互补品商品化"这一策略：配件越便宜越丰富，越有利于硬件销售。bigyabai 的看法更冷峻：这是为了吸引配件厂商，然后把他们逼进购买 MFi 和连接器授权的局面。nujabe 猜是欧盟逼的，zimpenfish 反驳说早在欧盟对苹果动手之前就能拿到，他 2015 年就用 iPhone 6 的官方 PDF 尺寸，配合 Cricut 切割机给透明手机壳做图案插页。kalleboo 查了 Wayback Machine，发现这个网址几个月前才出现，好奇是否与欧盟可维修性法规有关。layer8 和 JeremyHerrman 给出了更确切的历史：这些图纸原来就在单个 PDF 的《配件设计指南》里，后来变得太臃肿；独立页面大约在 2026 年 5 月上线，最初 13 款产品，现在接近 90 款。layer8 还提醒 ndr42：他去年想给 iPad 3D 打印保护壳，用磁通检测仪找磁铁位置很不准，其实磁铁位置甚至磁力强度多年来一直公开写在那份指南里。

**山寨厂商真的用吗？** eloisius 一直以为中国的工厂每逢新 iPhone 发布都拿卡尺从头量。userbinator 说手里有真机的话多半还会量，因为模具即使在中国也不便宜，必须确认无误；图纸更大的用处是在产品发售前。gertlex 猜测苹果会在发布前与部分厂商签保密协议提供数据，tonyedgecombe 和 Maxion 则说大厂商是靠贿赂代工厂员工或直接买泄露资料。yreg 回忆 iPhone 6 时期曾有流传的设计谣言是错的，有照片显示中国仓库里堆满了按错误设计做的手机壳；而 iPhone 4 时 Gizmodo 拿到了真机原型，很多人反而不信。matchstickman23 说他一直喜欢给苹果产品设计配件，正是因为有这些文档；谷歌似乎也想为 Pixel 做类似的事，但要申请才能查看，他从未获批。plorg 说很多需要和第三方设备配合的公司都会这样做，反倒奇怪苹果只给 2D PDF。

**要不要 3D 文件？** sysguest 希望苹果发布 FBX 或 Blender 文件方便打印装饰品，LiamPowell 解释说正经的工程公司不会用这类格式：Blender 文件是网格，连一个半径都提取不出来；就算是 STEP 这样的正规模型格式，也承载不了图纸上真正有用的信息，比如摄像头的禁入区。做配件时尺寸几乎可以当作完美的，因为配件本身需要一定的弹性余量才能装得上。

**图纸细节引发的讨论。** szemy2 问 MacBook Neo 的圆角为什么用一串小间距的坐标而不是半径来定义，fainpul 解释说那不是恒定半径的圆弧，而是苹果到处在用的"squircle"（超椭圆式圆角）；zimpenfish 补充说严格来说苹果的轮廓也不是纯粹的超椭圆，而是用精心调整的贝塞尔曲线逼近。tippa123 惊叹 Apple Watch Ultra 3 图纸的细致程度，好奇制造商的良品率和表面粗糙度如何控制；lefra 认为那些密集的点多半是设计师画出好看的曲线后，制图者让 CAD 软件每隔 0.5 mm 打一个参考点，模具厂会直接按生产文件里的曲线走刀，忽略这部分图纸。djfergus 和 wartywhoa23 好奇为什么只有 MacBook Neo 一款 Mac，twoodfin 猜测 Mac 产品线历史更久、设计工具链不同，Neo 可能是按 iOS 设备的方式设计的；layer8 认为这些图纸本来服务于手机壳之类的配件，对 MacBook 意义不大，旧款大概也没有可直接发布的格式。adolph 注意到 Vision Pro 蔡司镜片插件的"每块磁铁的磁极"表格分别列出顶面和底面，乍一看像是每块都是小马蹄磁铁，翻到第二页才明白"顶面"指朝向人的一侧。Dig1t 问图纸里那种好看的字体是什么，Geee 解释说那是 CAD 软件生成的单线笔画（SHX）字体，并不是真正的字体文件，Claude 猜是 PTC Creo 的，但他没能证实。mrcwinn 调侃说"连 Vision Pro 的图纸看着都很沉"。

**跑题主线：苹果用 Windows 做 CAD。** barbacoa 说讽刺的是苹果所有产品的机械 CAD 工作都在 Windows 虚拟机里的 Siemens NX 上完成，这条引出最长的讨论串。leohonexus 怀疑 NX 在没有 NVIDIA 显卡直通的虚拟机上未经认证，steve1977 认为可能是在 x86 服务器上跑的 VDI。starky 和 nikitaga 说专业 3D CAD 基本只有 Windows 版，像苹果这样多人协作复杂模型的公司，不会冒险用 NX、CATIA、Creo、SolidWorks 以外的东西；zokier 指出 NX 直到 2019 年还支持 macOS 和 Linux，很多大型 CAD 软件本来起源于 UNIX 工作站，如今的局面颇为讽刺。AdamH12113 说专业 CAD 软件的价格可能是电脑的 5 到 10 倍，人们是为了软件买电脑，而不是反过来。figbert 说并非全部，苹果有一部分工作用的是 Mac 原生的 Shapr3D，而且与其团队合作密切，varjag 认为 Shapr3D 太轻量，一家研发预算无限的公司不太可能大量使用。zdc1 觉得这是苹果自己做 CAD 填补市场空白的好机会，asdfsa32 回应说苹果做的是消费硬件，不是工程软件；Someone 提出也许苹果可以像当年收购 Logic 和 Final Cut 那样买下一家 CAD 厂商。ericmcer 提了个务实的理由：苹果可能积累了几十万个 3D 零件文件，不到旧系统忍无可忍的时候没人会迁移。随后话题又从"Win32 API 是不是一团糟"一路吵到 GNU libc 的兼容性，dgellow 称 Win32 是史上设计最好的 API 之一，raincole 说当年为 Win32 写的软件大多今天还能在 Windows 10/11 上运行，X11/Wayland 可做不到。

**其他。** thechao 希望所有汽车都能有这样的俯视和侧视比例图，方便直接比较车内空间；weinzierl 指出电子产品其实已经有类似资料：凡是带无线电的设备都要做 FCC 认证，申报材料公开，包括清晰到能读出芯片型号的内部照片，"没人打算建一个拆解档案库，它只是认证流程的副产品"。最后还有人用 AI 根据 PDF 生成 3D 模型来炫技，alexfringes 直言生成的 Apple Watch Ultra 几乎认不出来。
