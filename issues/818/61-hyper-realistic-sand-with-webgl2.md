---
layout: article
title: "把手机当沙盘：WebGL2 沙粒交互演示"
issue: 818
number: 61
category: fun
original_url: "https://av.github.io/safe-sand/"
hn_url: "https://news.ycombinator.com/item?id=50453288"
date: 2026-11-13
---

## 文章摘要

Safe Sand 是一个用 WebGL2 与 JavaScript 制作的浏览器玩具。页面说明可以用手指推动沙粒、倾斜手机使它们滑动，或摇晃设备让颗粒散开，也提供重新倒沙、声音和触觉反馈等控制。它更接近把手机屏幕当成俯视的开放沙盘，而非侧视的落沙解谜游戏；这一视角差别也解释了为什么一些用户刚打开时以为沙子消失是故障。

本次读取了公开界面与 HN 评论，没有在手机上授权传感器、触摸试玩或进行帧率测试。标题的「超写实」是项目描述，不是本文验证过的物理准确性。评论者对沙粒堆积、石子、残留痕迹和不同材质的视觉表现印象不一，有人感觉很逼真，也有人指出停止过快、弹回与摩擦不自然。视觉感染力和严格遵守颗粒力学仍是不同标准。

设备差异是讨论重点：有人在手机上顺利体验，有人遇到低帧率、屏幕旋转重置或浏览器权限限制。不能仅由某个机型的成功或失败推断所有同类设备表现。它展示了浏览器图形与移动传感输入结合后的表达空间，也说明一个无需教程的直觉玩具，仍可能需要清楚交代视角、初始摆放和控制方式，才能让用户理解它模拟的对象。

## HN 评论精华

- [winkelmann](https://news.ycombinator.com/item?id=50458636) 提醒这不是 Powder Toy 式侧视落沙，手机屏幕朝上更容易理解场景。
- [ChrisMarshallNY](https://news.ycombinator.com/item?id=50455486) 观察到旋转检测会重置布局，建议锁定方向；这属于用户排查经验。
- [techpression](https://news.ycombinator.com/item?id=50454971) 对真实感与性能提出批评；[momojo](https://news.ycombinator.com/item?id=50456661) 则喜欢这种只为乐趣而存在的小玩具。两类评价关注的目标不同。
