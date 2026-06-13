---
layout: article
title: "Edsger——为 reMarkable 2 手写的 Clojure REPL"
issue: 797
number: 7
category: favorites
original_url: "https://handwritten.danieljanus.pl/2026-06-01-edsger.html"
hn_url: "https://news.ycombinator.com/item?id=48374552"
date: 2026-06-12
---

## 文章摘要

Edsger 是一个为 reMarkable 2 电子墨水平板打造的"手写 Clojure REPL"。有趣的是，这篇博文本身也是手写后扫描、再转换为 SVG 呈现的，与项目主题相得益彰。

其工作方式是：用户在 reMarkable 2 上用笔手写 Clojure 代码，系统识别手写输入，将其作为代码求值，并把结果回写到设备上——把纸笔式的书写体验与编程的"读取-求值-打印循环"连接了起来。手写识别依托手写转文本能力（借助 Claude 等模型进行转写）来解析所写的 Clojure 语法与命令。项目托管在 Codeberg 上，代表了一种让编程更具触感、更"模拟化"的探索。

作者也坦诚地讨论了其最大的局限——延迟：从笔停下到出结果约有 14 秒，其中大部分耗时来自设备本身把更新后的文件写入磁盘的延迟，而作者尚未找到更早/更频繁触发写盘的办法。整体上，这是一个"因为能做、而且好玩"的极客实验，而非追求实用的工具。

## HN 评论精华

**nathell** 表示赞赏，并分享自己也写过一个 reMarkable 上的"魔镜/神谕"式工具：可以向屏幕上的 VLM 提问并得到回答。他探索过几种降低延迟的方案，比如直接读写帧缓冲（framebuffer）以获得即时更新、或用设备的流式 API 把屏幕推流到性能更强的服务器处理。

**vessenes** 惊喜地表示自己之前根本不知道还能 ssh 进 reMarkable 折腾这种好玩的东西，并认同"因为能做、而且好玩"在这里永远是个站得住脚的理由。

**LandR** 对这个手写博客的概念印象深刻，好奇文章是否是在 reMarkable 上写出来的，以及作者是如何把超链接嵌进手写文字里的。

**hiepph**（原帖作者）回复说，这篇文章已从 PNG 改用 SVG，并找到了更简单的方式来确定链接坐标，但整体流程与早先的"手写超链接"做法基本一致，并附上了相关旧文链接。

**andersmurphy** 与 **xnorswap**、**embedding-shape** 等人则聚焦那 14 秒的延迟：好奇这段时间究竟花在哪——是 reMarkable 处理、Claude 转写还是启动开销；并指出老款 reMarkable 把笔记存盘本就要好几秒，作者也确认延迟主因正是设备写盘缓慢。
