---
layout: article
title: "KetchUp：用 Rust 与 WebGPU 构建的开源建模工具"
issue: 818
number: 32
category: design
original_url: "https://ketchup3d.github.io/app/"
hn_url: "https://news.ycombinator.com/item?id=50418176"
date: 2026-11-13
---

## 文章摘要

这个项目希望把简单的三维建模重新交给需要画家具、木工构件或小型外壳的人，而不是先要求使用者掌握复杂的参数化 CAD 工作流。原投稿称为 KetchUp；本次访问其页面及官方仓库时，项目已经改名为 BenchUp。名称变化不是另一个产品，阅读旧评论时应注意这个时间差。

根据[官方 README](https://github.com/BenchUp3D/editor)，程序以 Rust 编写，提供桌面版本和浏览器版本，图形层使用 wgpu；网页端优先采用 WebGPU，也有 WebGL2 回退路径。基本交互围绕画线、画面、推拉成体展开，并配合捕捉、分组、组件和尺寸标注。它试图延续 SketchUp 对非专业用户友好的直接操作方式，而不是简单复制专业机械设计软件的功能列表。项目还说明了本地保存、自动保存和模型交换格式，表明设计对象不仅是展示用的图片，也包括可继续编辑的模型。

开源和免订阅并不能自动解决建模工具最难的问题：选择面、输入尺寸、理解当前操作状态是否足够顺手。当前文档也在持续演进，不能把抓取时的所有新增功能都视为投稿当天已有。本摘要补读了改名后的官方文档，但没有运行程序、导入模型或验证制造尺寸；移动端、云协作和专业 CAD 的完整替代能力也不应由标题推断。

## HN 评论精华

- [xlayn](https://news.ycombinator.com/item?id=50418793) 担心名称与既有产品过于接近，讨论的是潜在品牌风险，并非法律认定。

- [yboris](https://news.ycombinator.com/item?id=50418852) 指出画矩形时要求命名会打断操作，认为默认流程不该让简单建模变得繁琐。

- [LinguaBrowse](https://news.ycombinator.com/item?id=50420631) 认为选面再推拉等细节还不够直观，强调交互打磨会直接决定这类工具能否留住用户。
