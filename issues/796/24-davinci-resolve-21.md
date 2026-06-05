---
layout: article
title: "DaVinci Resolve 21 发布"
issue: 796
number: 24
category: design
original_url: "https://www.blackmagicdesign.com/products/davinciresolve/whatsnew"
hn_url: "https://news.ycombinator.com/item?id=48384482"
date: 2026-06-05
---

## 文章摘要

Blackmagic Design 发布了视频后期制作软件 DaVinci Resolve 21，带来了大量新功能，覆盖照片、AI、剪辑、调色、音频等多个方面。

**Photo 照片页**：DaVinci Resolve 引入了专门的照片编辑功能，配备好莱坞级别的调色工具。用户可以对静态图像应用基于节点的调色、一级校色、曲线和 Power Window。新的 LightBox 视图能实时展示整个相册套用调色后的效果。

**AI 工具**：21 版大幅扩展了 AI 能力。IntelliSearch 让你能在媒体库中"用 AI 即时搜索人物和内容"；Face Tools 提供年龄变换、面部重塑和瑕疵去除，并自动追踪；图像增强方面有 UltraSharpen 和 Motion Deblur 用于提升素材质量；Slate ID 能自动从场记板提取元数据，"节省数小时的元数据录入时间"；Speech 语音生成可以用自定义语音样本从文本创建配音。

**剪辑改进**：Cut 和 Edit 页面增强了关键帧功能，支持 loop 和 pingpong 动画，并加入贝塞尔缓动以实现复杂变速。可以"直接在关键帧和曲线编辑器里为 Fusion 特效做动画"，无需切换页面。原生支持 Lottie 动画和 HTML 图形，可以拖放集成动画。

**调色**：新的 MultiMaster Trim Manager 能从单条时间线生成多个 HDR 和 SDR 交付版本。Layer List View 提供更清爽的节点界面，群组版本管理则允许同时跨片段管理多套调色方案。

**Fairlight 音频**：轨道管理新增文件夹功能，用于组织和折叠音频通道。片段 EQ 扩展到六段，与轨道 EQ 能力对齐。新的 EQ 和 Level Matcher 工具能自动把目标片段对齐到参考音频的规格。

**其他亮点**：支持 VR180/VR360 沉浸式工作流、Apple 平台上的注视点渲染（foveated rendering），以及对索尼和佳能设备的直接相机连线（tethering）。

## HN 评论精华

讨论几乎完全聚焦在"AI 营销"这件事上。`Lalabadie` 开门见山：第一部分的 9 个功能，9 个标题里都带着"AI"，"我不觉得他们用 AI 不好，我只是累了"。他进一步阐述：对终端用户而言，如果你要靠"AI"这个词去做营销，其实就已经没读懂受众了——你是在写给 VC 看，却指望它能说服客户；应该直接说功能能做什么、能带来什么收益。他把这比作 2010 年那条安卓平板广告："你老婆会爱上全新的双核 Tegra 芯片！"

`Sohcahtoa82` 完全赞同，认为消费者对 AI 的情绪极度负面，标榜"AI"功能更可能丢掉销量而非创造销量。但 `scottyah` 反驳说他们的目标受众是创作者而非消费者，"创作者热爱 AI"。这又引发了关于"创作者到底爱不爱 AI"的激烈分歧：`rezonant` 指出创作者对内容被无偿抓取、对 AI 行业压低优质作品价值并不买账；`embedding-shape` 则认为创作者群体太庞大、动机各异，不能一概而论，他个人作为程序员兼业余视频剪辑爱好者，喜欢能替自己干无聊活的 AI 功能，但想亲手做有意思的部分。

`zuminator` 说，把每个功能标题里的"AI"去掉，描述照样准确，如今它"只是营销噪音，干扰大于信息量"，就像 1990 年代的"cyber"。`dist-epoch` 则站在另一边：以前没有 AI 的"背景去除""人脸变老"功能都很烂，标题里加上"AI"意味着"这次是真能用了"。还有一条很长的支线讨论：`wavemode`、`swatcoder`、`paulluuk` 等人把生成式 AI 类比为电影工业里的 CGI——最好的用法是让人察觉不到，但它也可能像 CGI 一样从根本上改变主流影像美学。
