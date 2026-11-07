---
layout: article
title: "Florilegium：用 LaTeX 程序化生成花卉页边"
issue: 817
number: 31
category: design
original_url: "https://github.com/kevinraymond/florilegium"
hn_url: "https://news.ycombinator.com/item?id=50305637"
date: 2026-11-06
---

## 文章摘要

Florilegium 将书页留白变成程序生成的花园。项目不依靠贴入花朵图片，而是在排版时计算花瓣、叶片和茎的曲线，再以近似水彩的笔触直接输出到 PDF。随机种子控制结果，同一个种子可以重现同一版面，不同种子则生成不同植物与构图。作者说明项目受到一则 LaTeX 装饰提问启发，并公开承认使用 Claude 辅助开发。

当前 README 区分了两层程序化：较早版本把手工设计的形状随机变化，再安排到留白中；2.0 的生长式页边进一步从植物性状组合中采样物种，让「长在哪里」和「长出什么」都由程序决定。布局使用贪心填充，先寻找较大的空隙，再逐渐填小空隙。相同文档中的物种可以重复出现，形成连续的视觉语言，而不是每页完全独立的随机拼贴。

完整的自动页边依赖 LuaLaTeX，也提供纯 TikZ 图形入口、SVG 输出和浏览器演示。作者公布了特定硬件与 TeX 版本下的耗时、文件大小对比，说明华丽装饰有明确成本，不能把它视为免费的排版附加项。移植时应固定发行版本：README 提醒内部模块可能重命名，稳定入口与内部实现并非同一回事。本摘要介绍抓取时的 README，版本细节可能比最初 HN 投稿时更晚。

## HN 评论精华

**美感获得认可，制作过程引发分歧。** [b19n](https://news.ycombinator.com/item?id=50307313) 说这种美丽的小项目缓解了自己对软件的失望。[bonzini](https://news.ycombinator.com/item?id=50310201) 并不反对用 AI 解决个人需求，但希望公开发布时仍能清楚解释动机与价值。

**传统是否排斥新工具？** [forgetfulness](https://news.ycombinator.com/item?id=50308377) 怀念 TeX 装饰的手工技艺；[amoshebb](https://news.ycombinator.com/item?id=50315188) 反驳说，尝试前沿工具来制作漂亮文档本身也属于这条传统。

**名字并非只是漂亮拉丁语。** [epihelix](https://news.ycombinator.com/item?id=50313269) 指出 florilegium 有采花与选集的双重含义，恰好呼应植物装饰和书籍。
