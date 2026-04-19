---
layout: article
title: "DaVinci Resolve 发布照片编辑器"
issue: 790
number: 28
category: design
original_url: "https://www.blackmagicdesign.com/products/davinciresolve/photo"
hn_url: "https://news.ycombinator.com/item?id=47760529"
date: 2026-04-17
---

## 文章摘要

Blackmagic Design 将其影视级后期软件 DaVinci Resolve 扩展到了静态摄影领域，推出了名为"DaVinci Resolve Photo"的全新照片编辑页面。这款产品的独特之处在于它把电影工业中沉淀多年的专业调色工具——一级调色、曲线、Qualifier 键、Power Windows、以及波形、矢量、直方图等示波器——原封不动地搬到了照片工作流中，并采用节点式（node-based）处理管线，而不是 Photoshop/Lightroom 那种基于图层的传统方式。

在摄影基础功能方面，Photo 原生支持 Canon、Fujifilm、Nikon、Sony 及 iPhone ProRAW 格式，可处理高达 32K 分辨率（4 亿像素以上）的图像，并提供非破坏性的变换与裁剪、白平衡、曝光、色彩、饱和度等标准调整。AI 和特效方面，它内置了 100 多个 GPU 加速的 Resolve FX，包括 Magic Mask 与 Depth Map（主体/背景分离）、Relight FX（后期重打光）、面部精修与美颜工具、AI SuperScale 超分辨率、UltraNR 降噪和 Patch Replacer 修补。工作流层面则支持相册管理、Sony/Canon 相机联机拍摄、Blackmagic Cloud 协作、以及 JPEG/PNG/HEIF/TIFF 快速导出。

商业模式延续了 DaVinci Resolve 的一贯做法：免费版几乎涵盖全部核心功能，Studio 版售价 295 美元买断，支持 Mac（Metal/Apple Silicon）、Windows 和 Linux，并充分利用 NVIDIA CUDA、AMD/Intel/Qualcomm OpenCL 进行 GPU 加速，还兼容 Blackmagic 的实体调色面板。相对 Lightroom 等基于调整参数的工具，Resolve Photo 把"电影级调色 + 节点工作流 + 实体控制面板"的组合带到了摄影桌面，定位明显不是替代 Lightroom 的图库管理，而是补全后期创作的调色深度。

## HN 评论精华

1. **免费版含金量惊人**：omgitsu 强调 Resolve 的免费版在核心功能上已足以对标 Adobe/Apple 付费产品，只有针对团队协作和影视工程才会锁到 Studio 版，这种定价策略对独立创作者几乎是降维打击。

2. **摄影基本功仍有欠缺**：jiggawatts 泼了冷水——Resolve 在摄影侧仍缺少一些基础能力，比如恰当的 HDR 导出、校准色彩输出，哪怕调色再猛，在摄影师看来也算"偏科生"，距离 Lightroom 的基本盘还有距离。

3. **UI 是视频软件长出来的**：jayphen 指出界面显然是按视频剪辑逻辑设计的，照片功能像"贴"上去的，缺乏蒙版等摄影师习惯的直观工作流，对从 Lightroom 迁移过来的人有明显学习成本。

4. **295 美元的学习投资**：PaulHoule 作为半专业摄影师认为 295 美元的 Studio 价格是学习系统性调色的划算投入，而且买一送一还赠送了强大的视频剪辑能力，对跨界创作者非常有吸引力。

5. **摄影市场远大于影视**：dbspin 反驳了"视频客户预算更大"的论调，指出照片市场体量远超视频市场，DaVinci 在专业院线调色之外的主导地位其实有限；arecsu 补充说摄影软件在 LUT、胶片模拟、光晕（halation）等创意特效上长期落后于视频工具，Resolve 入场正好补齐短板。
