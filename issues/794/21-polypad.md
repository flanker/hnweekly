---
layout: article
title: "Show HN：Polypad——浏览器里的数学教具实验室"
issue: 794
number: 21
category: show_hn
original_url: "https://polypad.amplify.com/"
hn_url: "https://news.ycombinator.com/item?id=48166744"
date: 2026-05-22
---

## 文章摘要

**Polypad** 是一个免费的、跑在浏览器里的**虚拟数学操作教具（virtual manipulatives）**平台。无需登录、无需安装、所有设备和浏览器都能用、跟任何课程都兼容——它的定位是"互联网上**最全的虚拟操作教具库**"。

它把过去那些只能用塑料和木头做的实物教具全部数字化了——**分数条**、**天平**、**3D 多面体**、**函数机**、**骰子 / 硬币 / 旋转盘**、**数据科学工具**、**数字方块**、**逻辑门**。学生可以在一个无限大的白板上自由拼接、拖拽、缩放，老师可以做布置作业、实时监控学生进度。平台还有声音表征——一些数学概念可以"听"出来，甚至可以用方程作曲。

技术上有一个让 HN 程序员特别兴奋的细节——Polypad 这种重度交互的图形界面**没有用任何主流前端框架**。核心是它的母公司 Mathigon 自己造的一个小库 [boost.js](https://github.com/mathigon/boost.js)——把 jQuery 风格的 DOM 包装、Vue 风格的响应式、模板化、Web Components 揉到了一起。换句话说，作者主动放弃了 React / Vue 的便利，换来了对 DOM 的**直接控制权**——对于需要大量 SVG、Canvas、自定义命中测试、低延迟拖拽的可视化工具来说，这个 trade-off 反而是赢的。

母公司 Mathigon 由 Amplify（脱胎自 Desmos 的教育科技公司）几年前收购，因此这个产品背靠的是美国 K-12 数学教育里非常成熟的一条产品线。

## HN 评论精华

- **ajithranka**（最有信息量的一条）：Polypad 本身不开源，但**它底下的库都是开源的**，发布在 [Mathigon 这个 org](https://github.com/mathigon) 下。最有意思的是它**没有用传统前端框架**——核心是一个小型库 [boost.js](https://github.com/mathigon/boost.js)，混合了 jQuery 风格 DOM 包装 + Vue 风格响应式 + 模板 + Web Components。他感慨："**我希望这种做复杂 UI 的风格更常见一些。对高度交互的图形和视觉工具来说，对 DOM 有直接控制权效果非常好。**"

- **tieandjeans**（教育学视角）："**Amplify（前 Desmos）做的工具和教具在课堂使用和好奇心驱动玩耍上都很棒。**"他链接了 Dan Meyer 在 Mathworlds 上写的教育法博客，说明 Polypad 背后是一整套有研究基础的教学理念。

- **gabesullice**（同行回忆）：分享了自己做类似项目的经历——一个浏览器版的**七巧板（tangram）**教育游戏。最有意思的工程问题不是图形，而是判定"拼图完成了吗"——因为同一个轮廓有多种合法拼法，他没法穷举所有解。最终用启发式规则：所有块必须互相相邻、不能重叠、必须覆盖目标轮廓、不能跨出轮廓。"我被 JavaScript 浮点数搞到拔头发"——9 年前的最后一次 commit 还是"修复重叠 bug"。

- **aaronharnly**（带身份披露的内部人）：作为 Amplify CTO，他贴了 [Polypad Art Contest](https://polypad.amplify.com/contest) ——年度作品比赛。从去年得奖作品里他挑了一个用教具拼出的"**月光奏鸣曲**"（12-15 岁组）和一位教育者的作品。这个比赛侧面说明 Polypad 不只是工具，已经发展出一个**创作社区**。

- **mosselman**（家长视角）：被产品的 landing page 完全征服——"展示了它能做的一切，我试着创建了一个，效果就像介绍页承诺的那样好。"承认自己读起来像广告但确实就是兴奋，准备让自己的孩子玩。

- **samth**（教师反馈）：一句话——"**这是我二年级儿子从学校带回来的最喜欢的教育产品**。"短，但是含金量极高。

- **turtlebits**（UX 抱怨）：操作不够顺手——希望支持滚轮缩放、右键/中键拖拽平移，目前的画布交互方式让他有点抓狂。

- **WillAdams**：感慨——他在找一个能给他"咔嗒一声"的 **2D 几何教学**站点，目前只有 BlockSCAD3D 给过他这种体验。Polypad 看起来还差一点点能扣上他心里的位置。Processing 和 Nodebox 都试过但没成。

- **yanis_t**：欣赏一个产品细节——每个小部件右上角有一个问号，点进去左边面板会显示对应教程。**"也谢谢你不要求注册就能用。**"

- **BeaverGoose**（小但真实的抱怨）："**别把我的浏览器后退按钮搞坏来做你的菜单。**"——典型的 SPA 路由问题。

- **pocksuppet**：直接拿到 403 Forbidden，访问受限。
