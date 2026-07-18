---
layout: article
title: "微软 Comic Chat 正式开源"
issue: 801
number: 6
category: favorites
original_url: "https://opensource.microsoft.com/blog/2026/07/16/microsoft-comic-chat-is-now-open-source/"
hn_url: "https://news.ycombinator.com/item?id=48936426"
date: 2026-07-17
---

## 文章摘要

微软宣布将 20 世纪 90 年代的经典聊天客户端 Microsoft Comic Chat 开源。Comic Chat 是一款别出心裁的 IRC（Internet Relay Chat）客户端：它不把文字消息以纯文本呈现，而是将聊天转化为漫画分格——参与者化身插画角色，配上对话气泡、面部表情和手势，全部根据输入的文字实时自动生成。程序会分析对话中的线索，自动选择合适的姿势、表情和分格布局，堪称一种真正的"编辑决策"能力，而不只是简单的文字显示。

历史方面，Comic Chat 由微软研究院 Virtual Worlds Group 的 David "DJ" Kurlander 于 1995-1996 年间构思并开发，Tim Skelly、David Salesin 参与了技术研究，漫画家 Jim Woodring 设计了独特的视觉风格。它用 Visual C++ 4.0 和 MFC 构建，随 Internet Explorer 3 一同发布。其一大历史意义是：它把如今臭名昭著的字体 Comic Sans 带给了世界——这款字体由 Vincent Connare 于 1994 年设计，其非正式、手写体的风格与对话气泡界面完美契合。Comic Chat 后来被本地化为 24 种语言，并随 Windows 98 捆绑分发。

微软表示此次开源的目的是保存这一段重要的软件史，让社区能够探索、学习和实验。发布内容还包括让这套 90 年代代码用现代 Visual Studio 工具构建、连接现代 IRC 服务器、并在高分辨率显示器上运行的现代化尝试。代码已托管于 GitHub 的 microsoft/comic-chat 仓库（文章正文未明确说明所采用的开源许可证）。

## HN 评论精华

- 一段关于"真正的作者是谁"的澄清成了讨论重点。**bdsa** 指出博客署名的 Robert Standefer 并非 Comic Chat 的创造者，真正的作者是 David Kurlander。随后 **outintospace**（即 Robert Standefer 本人）现身回应："没错，DJ Kurlander 才是创造者，博客里也致谢了他；我只是那个在 DJ 支持下促成这次开源发布的人，他 20 多年前就从微软退休了。" 这段本人下场的互动获得大量点赞。

- 不少人回忆 Comic Chat 是自己接触互联网/IRC 的起点。**superkuh** 讲述自己小时候在 system32 目录里翻到 mschat.exe，从此打开新世界、至今仍活跃于 IRC 社区。**z500** 则自认是当年那批"糊里糊涂"的 Comic Chat 用户之一，幸得有人温和地引导他换用真正的 IRC 客户端，此后成了多年的 IRC 狂热爱好者。

- 关于 Comic Chat 当年"人憎狗嫌"的原因，**Athas** 给出了技术解释：它扩展了 IRC 协议，在每条消息里塞入一段编码字符串来指示角色外观和表情（如 `# Appears as TIKI (#G010E010M1)`），对非 Comic Chat 用户而言就像垃圾噪音。**efdee** 补充说微软本有自己的专用服务器、本不该用在"常规"服务器上；**art0rz** 作为 IRC 管理员回忆自己专门设置了自动踢人规则来对付这些消息。多人评价这是"典型的微软式作风"（EEE，拥抱、扩展、消灭）。

- 一场对"Copilot"品牌泛滥的辛辣吐槽意外走红。起因是原作者的现任职位"Principal Program Manager, Copilot Acceleration Team"。**inigyou** 提供了一套"微软术语翻译器"："Copilot"就是别家所说的"AI"，"365"意为"在线"，"Azure"意为"云"，"Entra"意为"登录"，所以"Azure Copilot 365"翻译过来就是"在线云 AI"。

- **cube00** 从软件考古角度发现一个有趣细节：v1.0-pre 与 v1.0 共享同一内部版本号（rup 206，"Beta 2"），但在 111 个共享源文件中有约 99 个不同，令人惊讶微软当年（尽管已自研版本控制系统 SLM）竟没有更好的版本管理。这引出 **ndiddy** 对微软 SourceSafe 的著名声讨：不支持原子提交、"永久删除"会抹掉文件的全部历史、还会随机损坏文件，导致它无法可靠地还原任一时点的代码状态。

- 在轻松的一面，**unfunco** 声称 Comic Sans 是 Slack 里最好的字体选项、人人都该试试；**Cshaya** 当场把 Slack 改成了 Comic Sans"看能坚持几天"；还有人力挺 Comic Mono 是最好的代码字体。**HeliumHydride** 贴出的 bonequest.com（Jerkcity 漫画）则带出了一整段属于老网民的亚文化回忆。
