---
layout: article
title: "面向 Web 场景的 JPEG XL 与 AVIF 对比"
issue: 815
number: 34
category: design
original_url: "https://jakearchibald.com/2026/jpeg-xl-vs-avif/"
hn_url: "https://news.ycombinator.com/item?id=50120224"
date: 2026-10-23
---

## 文章摘要

JPEG XL 如今已在所有主流浏览器落地，现就职于 Firefox 团队的 Jake Archibald（前 Google 开发者关系工程师、Squoosh 和 SVGOMG 的作者）借此机会做了一次详尽的基准测试，比较 JPEG XL 与 AVIF 在网页场景下的表现。他使用的编码器是 avifenc 1.4.2、libjxl 0.12.0（cjxl）、较新的 JPEG XL 编码器 jixel 0.4.2，以及做无损对比用的 cwebp 1.6.0，测试图片大多编码为 1598 像素宽（博客最大显示宽度的两倍，以适配高密度屏幕），在 M4 Pro 的 MacBook Pro 上运行。质量评估主要用 JPEG XL 团队开发的感知质量指标 SSIMULACRA2（50 为中等、70 为高、80 为很高、90 为视觉无损），并辅以 ColorVideoVDP 交叉验证。

**核心结论：**

- **有损照片：AVIF 几乎总是更小。** 默认编码强度下，最好的 JPEG XL 结果通常比同等质量的 AVIF 大 15% 到 25%；最高强度下差距缩小到 6% 到 20%。也有例外，比如一张黄鼠狼照片，JPEG XL 反而小 4% 到 27%，所以追求单张图的极致效果时值得两种都试。
- **编码器影响很大。** jixel 通常比 libjxl 产出更小的文件，但在 F1 赛车图和插画中，它在中等质量下出现明显噪点和色块，作者怀疑它在"迎合"指标，而视觉效果实际更差。
- **截图与插画：AVIF 大幅领先。** 同等质量下 JPEG XL 大 30% 到 90%。作者认为老派开发者觉得截图必须用无损格式，是因为 JPEG、WebP 处理这类图像很差，而有损 AVIF 处理得很好。
- **带透明度的图：AVIF 明显更好。** jixel 总是无损编码 Alpha 通道，文件是 AVIF 的 2 到 3 倍。
- **编解码性能：** 综合文件大小来看 AVIF 编码更快、内存更省；解码方面，JPEG XL 在中等质量下通常要花 2 到 3 倍时间，只在极高质量时差距缩小，Chrome 中偶尔反超。
- **编码强度：** 提高强度对 AVIF 和 jixel 值得，对 libjxl 不值得，后者最高强度慢得多、内存占用大、几乎没有收益，有时文件反而更大。
- **AVIF 不要默认用 4:2:0 色度抽样**，它很少能减小文件，还让高质量难以达到，默认应坚持 4:4:4。
- **质量参数可靠。** 有人批评 AVIF 同一质量设置在不同图片上效果飘忽，但测试发现三种编码器的波动方向一致且合理。
- **渐进式加载。** 两者都支持，但方式不同。JPEG XL 是先给低分辨率版本再逐步补细节；AVIF 则像视频帧，最多四帧，可以完全控制预览长什么样。作者给 F1 图做了一个 1/8 分辨率、略带模糊的 AVIF 预览帧，约 11.5 kB 就能认出是哪支车队的赛车；JPEG XL 约 2 kB 就有类似 BlurHash 的预览，但要到约 63 kB 才达到同样的预览质量。他还担心 JPEG XL 逐步变清晰的中间状态会被用户误以为是拍糊了的成品。渐进式 AVIF 目前仅 Chrome 支持，他本人正在为 Firefox 打补丁。
- **HDR：** JPEG XL 格式本身支持增益图（gain map），但浏览器解码器不支持、编码工具也很混乱，而 AVIF 支持。实测在屏幕亮度极高或极低时，带增益图的图像明显比 PQ 编码效果好。纯 PQ 对比中两种指标分歧很大，但都认为 JPEG XL 文件更小。
- **JPEG 转码与无损：JPEG XL 的强项。** 现有 JPEG 可以无额外损失地转为 JPEG XL，体积小 9% 到 20%，大图解码还更快。无损压缩方面 JPEG XL 明显胜出，无损 WebP 大 9% 到 17%，无损 AVIF 大 30% 到 120%，不过网页上几乎用不到无损图。
- **动图格式是个"蠢主意"。** 同等质量下动画 AVIF 725 kB，动画 JPEG XL 2.17 MB，因为后者逐帧独立编码。但动画 AVIF 在 Chrome 和 Safari 中播放很卡，直接用 `<video muted autoplay playsinline>` 就好。

作者的建议是：网页默认用 AVIF 4:4:4，尽量开高编码强度；如果 AVIF 文件异常大，再试试用 jixel 编 JPEG XL。他也指出两边近期都有巨大改进，结论可能会变，而网上关于这个话题的错误信息很多，"你的 AI 也会乐于把这些错误信息转述给你"，最好用自己的图片测试。文末他坦言有些紧张，因为围绕 JPEG XL 的讨论常常"有点怪"：有人认为 Google 在打压 JPEG XL，尽管项目的一位负责人就在 Google 工作；他甚至因为询问 JPEG XL 的用例被骂成种族主义者。他说这篇文章初稿对 JPEG XL 非常负面，后来了解到 jixel 后改变了看法，两个格式之间的激烈竞争最终会让所有人受益。

## HN 评论精华

本帖 330 分、约 216 条评论。作者 Jake Archibald（ID：jaffathecake）在评论区非常活跃。讨论的核心是：这次对比反映的是格式本身的优劣，还是两边编码器投入不同造成的阶段性差距。JPEG XL 的支持者和中立者争论不少，但整体比较克制。

**是格式不行，还是编码器落后？** F3nd0 是最积极的 JPEG XL 支持者，他认为 JPEG XL 刚推出时与 AVIF 不相上下，后来 Google 在 Chrome 中单方面叫停支持，让它的前途变得不确定，开发热情和人力随之流失；而 AV1 作为人人支持的视频编码一直在进步。所以这篇文章比较的是现有编码器，而不是格式潜力，可现在这些对比结果会在社交媒体上被引用成"JPEG XL 终究是更差的格式"。ksec 也认为 AVIF 这几年进步巨大，甚至可以说是 JPEG XL 的竞争逼出了 AVIF 的最佳状态，同时庆幸 JPEG XL 最终"主要靠 Safari"进入了所有浏览器。Klaus23 问 AVIF 的优势有多少来自编码器、JPEG XL 还有没有机会追上，作者给出一篇认为 JPEG XL 存在根本局限的文章；F3nd0 提醒那篇文章的作者参与过主流 AV1 编码器的开发，且只是基于推测。GrantMoyer 指出 2.5 年前 Cloudinary 的一项基准得出几乎相反的结论，作者回应那篇文章出自 JPEG XL 项目的负责人之一。linolevan 晒出他和朋友在开发的未发布编码器，把文中的 JPEG XL 无损截图和插画分别再压小 39% 和 30%。

**高质量场景与"指标作弊"。** adrian_b 认为 JPEG XL 适合存储和分发高质量图片，AVIF 适合压缩低质量网页图，作者回应文中默认比较的都是 JPEG XL 自家指标定义的"高质量"；JPEG XL 作者之一 JyrkiAlakuijala 则表示 SSIMULACRA2 85 才接近互联网上的中位质量，90 到 95 才算高质量。Gigachad 总结 JPEG XL 是理想的高端摄影格式，AVIF 是高效的网页格式。yhjc2692 建议像 LLM 竞技场那样大规模让用户盲测，因为指标未必代表人眼感受；作者说他在 Google 时曾坚持要求相关研究在高密度屏幕上进行，对方却回答"那样人们就看不出瑕疵了"，让他差点气得扭断脖子。他也承认 jixel 对 SSIMULACRA2 调得太狠，有些情况下人眼看起来比 libjxl 差。janwas 提到 JPEG XL 项目最初就是从开发 Butteraugli 感知指标开始的。yhjc2692 还分享了自己做的 GPU 加速 libjxl 移植版，能大幅加快高强度编码。

**页面本身的小插曲。** PaulHoule 说 F1 图正好证明 AVIF 的问题，车身上漂亮的高光被抹掉、换成"看似合理"的东西；其他人仔细看也没找到，他后来承认自己看的是作者 2020 年旧文里的另一张图。作者随后意识到，很多人用的浏览器还不支持 JPEG XL，页面会退回 WASM 渲染器，缩放方式不同，造成了错误印象，"这完全是我的锅"。有人问为什么不全部用 PNG 展示，作者说想保留体积优势；D2OQZG8l5BI1S06 指出 Firefox 三天前才刚加入支持，ESR 版本还没有。

**JPEG 无损转码。** adzm 认为这是 JPEG XL 最妙的功能，海量现存 JPEG 可直接受益。lern_too_spel 指出原理类似 Dropbox 的 Lepton，只是换了熵编码、保留 DCT 系数；CodesInChaos 问能否像 Lepton 一样还原出逐字节相同的文件，farlight、Broiler9437 等确认可以，用 cjxl 编码再用 djxl 解码，哈希与原文件一致，元数据也保留。Zardoz84 说他所在的图书档案数字化领域有几 TB 的 JPEG，转成 JPEG XL 会很有用。

**容器、动图与其他。** shmerl 认为 AVIF 基于容器，将来换 AV2 编码时网站无需重新适配；pornel 反驳容器毫无用处，新编码的文件旧解码器照样显示不了，ISO-BMFF 容器还臃肿复杂，他希望 AVIF 干脆跳过 AV2；作者也指出届时内容协商仍需新的类型参数。关于动图，作者澄清动画 AVIF 不只是关键帧，而是完整的 AV1 能力外加透明度；但性能太差，他宁可把 AV1 视频编成双倍高度、再用 WebGL 合成透明效果。fngjdflmdflg 则认为 GIF 式动图有不同的用户预期（没有控制条、在新标签页打开也能循环），值得单独成类。IshKebab 觉得 Google 当初不支持 JPEG XL 可能是对的，usef- 指出 PDF 已标准化采用 JPEG XL，浏览器反正都要带解码器；作者解释增加格式意味着更多维护代码、更大体积和更大攻击面，所以 Firefox 坚持用 Rust 写的解码器（jxl-rs，已被 Firefox 和 Chrome 采用，且比 Safari 用的 libjxl 更快）。ezst 以 MP3 时代 LAME 花多年才成熟为例，建议不要急于宣布赢家。theandrewbailey 说他的博客多年来都用 AVIF 压游戏截图，这篇文章证明他"不是疯了"。Theodores 问作者为何从 Google 跳到 Firefox，作者说他在 Google 待了十年、又去 Shopify 两年，但想念 Web；Google 在大部分时间里做的是对 Web 有益的事，后来开始更多推动自身利益，这是他离开的原因之一。
