---
layout: article
title: "Encarta 95 Web：在浏览器里重新打开百科全书光盘"
issue: 816
number: 20
category: show_hn
original_url: "https://benwinding.github.io/encarta_95_web/#/article/Art"
hn_url: "https://news.ycombinator.com/item?id=50218975"
date: 2026-10-30
---

## 文章摘要

这个项目不是重新制作一个 Encarta 风格的网站，而是用 v86 在浏览器中运行 Windows 95 和真正的 Encarta 95。官方仓库说明，网页恢复预先启动的系统快照，硬盘和光盘按 256KB 分块按需读取，因此可以作为静态网站托管，不必让用户先下载整张光盘再开机。

作者为网页环境补了鼠标捕获、触屏定位、屏幕键盘和文章的哈希路由。桌面端通过 Pointer Lock 控制模拟系统的鼠标；移动端则需要在宿主网页中追踪光标位置，把点击转换为相对鼠标动作。工程细节也包括兼容旧系统驱动的磁盘读取方式。原站抓取仅得到应用外壳，以上机制来自项目 README，没有实际运行模拟器验证。它的价值是保存旧软件的交互环境，而不只是保存里面的文字；同时，可在网上找到旧光盘资料，并不自动解决所有再分发授权问题。

## HN 评论精华

**读者很快从百科跳到小游戏。** sucrose 说自己退出 Encarta 后玩起桌面的游戏演示；Brendinooo、rahidz 则直接寻找 Mind Maze。多媒体百科给人留下的记忆，显然不只有词条检索。

**模拟与重写的区别影响体验。** notpushkin 解释背后运行的是 v86 与 Windows 95；ale42 指出这不是网页重制，所以鼠标行为会显得奇怪。est 和 Brendinooo 都提到鼠标捕获或灵敏度问题。

**旧内容的细节得到保留。** tadfisher 找到了 Rock Music 词条里的音乐片段，gitgud 分享相应链接。这些反馈说明文章定位有实际用途，但不构成对全部媒体、设备和浏览器兼容性的验证。
