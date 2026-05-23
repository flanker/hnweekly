---
layout: article
title: "学 CUDA 该读哪些书：一份社区清单"
issue: 794
number: 37
category: books
original_url: "https://github.com/alternbits/awesome-cuda-books"
hn_url: "https://news.ycombinator.com/item?id=48168485"
date: 2026-05-22
---

## 文章摘要

**awesome-cuda-books** 是 Altern（altern.ai）和 Dariush Abbasi（HN 上的 dariubs）维护的 GitHub 仓库，**号称"每一本主流 CUDA 编程书"的精选清单**——从入门到进阶、从 C++ 到 Python、从 GPU 架构到性能优化、再到 2024–2026 年最新出版的几本，全部分门别类列出。MIT 许可证开源，欢迎 PR 增补。

清单的分组逻辑很清晰：**基础概念入门**、**GPU 架构与并行计算理论**、**实战开发指南**、**进阶优化与性能调优**、**Python 路线（CuPy、Numba、PyCUDA）**、**最新出版（2022–2026）**。每一项都有简介、出版年份、读者画像。

**几本被反复推荐的经典**：
- **CUDA by Example（Sanders & Kandrot, 2010）**——"永恒经典"，最适合零基础入门，但**有评论说它过度抽象了硬件，不够深**。
- **Programming Massively Parallel Processors（第 3 版，Kirk & Hwu, 2022）**——清单里描述为"**GPU 架构圣经**"，但 HN 讨论里几乎所有人都说**这本书写作风格糟糕、代码示例有错**。
- **CUDA Programming: A Developer's Guide to Parallel Computing with GPUs**——评论区共识里**真正最适合入门**的那本。
- **Programming in Parallel with CUDA（Ansorge, 2022）**——现代写法。
- **Learn CUDA Programming（Han & Sharma, 2019）**——中阶过渡。
- **CUDA for Deep Learning（Arledge, 2025）**——AI 工作流向的最新方向。

清单同时建议**配合 NVIDIA 官方 CUDA Programming Guide（PDF）**——HN 上有人反驳"那不是 guide 那是 documentation"，但**所有人都同意这份官方文档必读**。

## HN 评论精华

- **somethingsome / dariubs**（高赞且实用）：读过或浏览过清单里大部分书，**真心推荐的入门书是 "CUDA Programming: A Developer's Guide"**。明确"差评"了两本经典——**Massively Parallel Processors 写作不好、代码错误多**；**CUDA by Example 对硬件抽象太严重**。他自己**计划明年开始写一本新书**——从硬件工程开始讲、一直讲到针对该硬件（基本就是 Nvidia 卡）的优化、覆盖主要算法（除了图算法）；这是他在大学里教学已经验证过的方法。

- **Aurornis**（共识）：感谢这条评论——"**比起一长串书单，一个有针对性的小列表更实用**。"反映了 HN 用户对"awesome-X"清单普遍的疲劳：清单越全越没用，**真正需要的是带个人判断的过滤**。

- **bobmarleybiceps**（PMPP 受害者）：希望有比 PMPP 更好的选择——**它是最新的书，但写作真的烂、有些代码示例完全错**。"我至少是一个愿意为更好的书付钱的读者。"

- **boomzilla / KeplerBoy**（关于官方文档）：boomzilla 推荐 NVIDIA 官方 CUDA Programming Guide PDF，KeplerBoy 立即指出"那不是 guide 那是 documentation"——但反过来也强调**"defacto documentation，你必须读"**。

- **synergy20 / jpgvm**（学习路径建议）：把 GPU 学习分成三层——**baseline**（设备/主机边界、SIMT 编程，10 年没怎么变）；**intermediate**（kernel 架构、CUDA graphs vs persistent kernels、warp specialization、避免发散，最近 10 年也基本稳定）；**advanced**（架构相关：tcgen05、TMA、SMEM/HBM、内存带宽 vs 计算偏置、GEMM、Flash Attention 这种现代融合 kernel 的全部技巧）。**老书学前两层完全够，第三层只能靠论文和博客**。

- **namibj**（极硬核硬件细节）：从 Volta 开始的"async load-from-global-to-shared-memory DMA memcpy"是少数真正改变核心模型的进展——之前 shared memory 只是 L1d$ 的一个分区，新增的硬件单元允许"显式 prefetch into user-managed slice"，这才让现代 fused kernel 成立。这条评论也说明**现代 CUDA 已经超出大多数书的覆盖范围**。

- **KeplerBoy**（缺口反馈）：**目前没有任何关于 GPU Direct RDMA 和 GPU NetIO 的好书**——这是个真实的文献空白，"清单里没有，因为根本没出版过"。

- **整体氛围**：所有人都同意——**CUDA 这门技术的核心思想 10 年没变，但前沿一年变三次**，所以书永远只能教你 baseline 和 intermediate，advanced 只能跟着 NVIDIA 的发布会、论文和 blog 走。
