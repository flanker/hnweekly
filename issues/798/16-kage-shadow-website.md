---
layout: article
title: "Show HN：Kage — 把任意网站打包成单个二进制供离线浏览"
issue: 798
number: 16
category: show_hn
original_url: "https://github.com/tamnd/kage"
hn_url: "https://news.ycombinator.com/item?id=48529990"
date: 2026-06-19
---

## 文章摘要

Kage 是一款开源工具，能够把网站克隆成可离线浏览的快照，并在过程中剥离所有脚本。它的工作流程很直接：用无头 Chrome 渲染页面，等待 JavaScript 执行完毕后捕获最终的 DOM，删除全部脚本，再把资源链接重写为本地路径。这样得到的快照不带任何跟踪代码，也不依赖网络。

工具用 Go 编写，采用广度优先抓取并遵守 `robots.txt` 与 `sitemap.xml`，配有专门的资源下载池和确定性的 URL 到路径映射。抓取过程可恢复、可设定深度与页数上限，还能通过自动滚动触发懒加载内容。

Kage 支持多种输出形式：可用 `kage serve` 在本地浏览的镜像文件夹、与 Kiwix 生态兼容的 ZIM 归档、内嵌整站的自包含单一可执行文件，以及原生桌面应用（macOS 的 `.app` 包或 Linux 的 AppImage，无控制台窗口）。它可以在一台机器上为多个平台生成二进制，并保证确定性打包（可复现校验和）。安装方式包括 `go install`、预编译二进制、容器镜像或各类包管理器。

## HN 评论精华

- **simonw**（704 赞）：研究了演示用的 GIF 是如何制作的，发现作者用了自己的 "ascii-gif" 项目，它封装了 charmbracelet/vhs 来做终端录制。
- **wolttam**：建议把它用于无蜂窝信号区域的公司 wiki 离线访问，提出用单个 HTML 入口内嵌归档，而不必单独跑一个服务进程。
- **tamnd**（作者回复）：感谢反馈，并提到计划把 HTML 转成 Markdown，将内容作为带版本的文件存进 Git 仓库。
- **ninalanyon**：既然输出是静态的，为何还要一个单独进程来 serve，质疑用户为什么不能直接在浏览器里打开文件。
- **maxloh**：推荐 SingleFile 作为更稳健的替代方案，它能把一切打包进单个 HTML，资源用 base64 编码内嵌。
- **telesilla**：分享了用 HTTrack 下载 wiki、在飞行途中离线阅读的经验，表示有兴趣试试 Kage。
- **d3Xt3r**：希望有折中方案——自解压归档调用系统默认浏览器，而不必内嵌整个浏览器引擎。
