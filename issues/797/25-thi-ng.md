---
layout: article
title: "Thi.ng——面向计算设计与艺术的开源构建模块"
issue: 797
number: 25
category: design
original_url: "https://thi.ng"
hn_url: "https://news.ycombinator.com/item?id=48437743"
date: 2026-06-12
---

## 文章摘要

Thi.ng 是由 Karsten Schmidt（网名 toxi）打造的大型开源项目，定位为面向计算设计、生成艺术与数据可视化的"构建模块"集合。它以一个庞大的 monorepo 形式组织，包含数量众多、互相解耦、零依赖、用 TypeScript 编写的通用模块，覆盖几何、颜色、向量数学、像素操作、ECS、状态管理、解析器、WebGL/着色器等方方面面。

项目的一大特点是**原子化**：每个包都可独立使用，并且都能通过形如 `https://thi.ng/PACKAGE` 的短链接直达。整体风格强调可组合性，鼓励用一组小而专注的工具拼装出复杂的创意编程与可视化应用。

Thi.ng 还是一个跨语言的"多语种"项目，作者在不同语言中都有相关实现。它脱胎于作者早年的 Clojure 代码库（以及更早的 Java 项目 toxiclibs），后来迁移到了 TypeScript。

## HN 评论精华

- **lioeters**：指出主仓库本质上是"一大批实用的、零依赖的通用模块，全部用 TypeScript 写成"。
- **guidoschmidt**：盛赞其原子化结构，特别喜欢"每个包都能通过 thi.ng/PACKAGE 短链访问"这个便利设计，并分享了自己用它做的创意作品。
- **kkukshtel**：对其外部采用率之低感到意外——明明每个库都倾注了如此多的用心、细致与思考，却没有获得应有的广泛使用。
- **toxmeister（作者回应）**：详细解释了从 Clojure 迁移到 TypeScript 的原因：对性能的需求、对现代 Web API 的集成，以及用文学化编程（literate programming）风格很难吸引到贡献者；并强调了项目跨多种语言的多语种特性。
- **geokon**：补充迁移理由——Clojure 社区兴趣有限、文学化编程带来的摩擦，以及当时拥抱 TypeScript 正好能触达更广的受众。
- **sleepybrett**：提到自己很早就熟悉作者此前的 toxiclibs，也讨论过出于语言偏好（而非能力顾虑）将其移植到其他语言的尝试。
