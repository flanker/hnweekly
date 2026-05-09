---
layout: article
title: "GitHub 之前，我们是怎么写代码、托管代码的"
issue: 793
number: 22
category: code
original_url: "https://lucumr.pocoo.org/2026/4/28/before-github/"
hn_url: "https://news.ycombinator.com/item?id=47940921"
date: 2026-05-08
---

## 文章摘要

Armin Ronacher（Flask、Jinja2、Sentry 早期开发者）在这篇带回忆色彩的博文里，向更年轻的开发者描述了一个被很多人遗忘的世界：**GitHub 出现之前的开源生态长什么样**。他自己的早期开源项目跑在他亲手维护的服务器上——一个 Trac 实例、一组 Subversion 仓库、tarball 发布、自己写的文档站。多个项目通常会抱团组成"集体"以分摊运维成本，他和一群朋友就一起搞了 **Pocoo** 这个共享基础设施，承载 Flask、Jinja2、Werkzeug、Pygments 等一系列项目。

那个时代的版本控制以 **Subversion 为主**，集中式架构让"自托管"成了天然选项——一个项目就是一个有具体 hostname 和目录的"家"。后来 Git 和 Mercurial 这类分布式系统出现，但多数项目仍保留独立的 home page、独立的 bug tracker（Trac、Bugzilla、Mantis、Bitbucket、SourceForge…），还没有统一的"个人主页"概念。开发者之间的信任更多来自**直接参与项目本身**——读源码、看历史、跟维护者打交道——而不是看 GitHub follower 数、绿色草地图或者 star 数。Debian 这样的发行版会真的去逐项审 license 和代码质量，"声誉"是一种实打实的资本。

文章的另一条主线是**依赖摩擦**。当时引入一个新依赖是要慎重权衡的事：外部源未必可靠访问，所以"vendor 进自己仓库"是常见做法。这种摩擦反过来逼着开发者认真评估"我真的需要这个依赖吗"。与今天 `npm install` / `pip install` 一行命令拉来上千依赖、大家对依赖图基本无感的世界形成强烈对比。Ronacher 没有做出"那时更好"的廉价怀旧判断，而是提醒读者：GitHub 把许多东西做得**更省事**，但同时也**抹掉了一些有价值的摩擦和分布式韧性**——当 GitHub 抽风时，整个生态才意识到自己有多依赖一个中心。

## HN 评论精华

- **mlex / simonw**：被这篇文章勾起回忆。Simon Willison 也是 SVN+Trac 时代过来的，他认为 Trac 当年提供的 wiki + ticket + 浏览代码的一体化体验，至今仍是优秀的工程范本——只是审美和性能没跟上。

- **dijit**：一段对 GitLab 的尖锐评价——"GitLab 的 CI 是它真正的 killer feature"，他非常喜欢用；但其余几乎所有功能（kubernetes 管理、AST 扫描、AI Duo、Work Items、Milestones、Snippets、Workspaces、Operations、Security Dashboards、Value Stream、Service Desk……）都很糟糕。他用这个反例呼应文章主题：当一个平台想"包圆"开发者所有东西，结果就是大部分功能都做得平庸。

- **saghm**：自嘲式认同——"我用任何 CI 平台都没顺过"。GitLab CI 不比别人差也不比别人好，但 GitLab 那一长串左侧菜单令人头大，自己只用源码托管 + MR 就够了。

- **mbreese / bombcar**：一段 Bugzilla 时代回忆。当时 Bugzilla 装上不难、配起来要命；它的高度可配置性正是后来"opinionated software"潮流的反面教材——Perl 风格"做同一件事有 N 种方法"渗透到了 Bugzilla 设计里。bombcar 还贴出 Dwarf Fortress 至今还在用 MantisBT 当 bug tracker 的现实例子，说明这些老系统从未真正消失。

- **the_mitsuhiko（即作者本人 Armin）**：在评论区补充——Trac 的插件系统是促使他选择 Python 而不是 PHP 来构建可分发 Web 应用的重要原因之一。

- **noir_lord**：自己一直把 GitHub 当成"哑巴 Git endpoint"用——build/deploy 全靠 docker + shell 脚本，因此对他来说迁移成本极低。这与文章的精神契合：把核心控制权握在自己手里，平台就只是托管 git 的工具。

- **cmrdporcupine**：替 GitLab 辩护——它最大的问题不是功能太多，而是 UX 没跟上：层级嵌套太深、东西不好找、UI 偶尔慢得离谱。把客户从 GitHub 迁到 GitLab 后，整体满意但"也带来了新的不满"。

- 综合大量回忆贴，HN 用户普遍认同文章的核心观点：GitHub 之前的世界**摩擦更多但更分布式**，今天对单一平台的过度依赖正变成系统性风险，而像 Codeberg、Forgejo、sourcehut 等替代物某种意义上是在"把 Pocoo 时代的多样性请回来"。
