---
layout: article
title: ".gitignore 并非 Git 中忽略文件的唯一方式"
issue: 798
number: 26
category: code
original_url: "https://nelson.cloud/.gitignore-isnt-the-only-way-to-ignore-files-in-git/"
hn_url: "https://news.ycombinator.com/item?id=48583356"
date: 2026-06-19
---

## 文章摘要

虽然 `.gitignore` 是最常用的忽略文件方式，但 Git 还提供了另外两种作用于不同范围的忽略机制。

- **`.gitignore`（仓库级）**：这是一个会被纳入版本控制、与所有协作者共享的文件，适合放置全项目都应忽略的内容，如构建产物、依赖目录等。
- **`.git/info/exclude`（仓库内个人级）**：位于 `.git` 目录中且不被追踪，适合按仓库忽略个人专属文件，比如临时笔记、本地脚本，既能忽略又不会污染共享的 `.gitignore`。
- **`~/.config/git/ignore`（机器全局级）**：在本机所有仓库中生效，适合忽略与操作系统相关的文件（如 macOS 的 `.DS_Store`）。也可通过 `git config --global core.excludesFile ~/.gitignore_global` 自定义路径。

文章特别提醒：当不确定某个文件被哪条规则忽略时，可运行 `git check-ignore -v <文件名>`，它会精确显示是哪个文件、哪一行规则在起作用。三种方式的使用场景大致为：仓库级用于团队共享的排除规则，仓库内个人级用于个人工作流偏好，全局级用于机器或操作系统层面的通用模式。

## HN 评论精华

**hungryhobbit**（439 赞）：推荐了被低估的 `.gitattributes`，它能让 `package-lock.json` 等文件照常提交，却不在 `git diff` 输出中产生巨大的 diff 噪音。

**kevincox**：主张把 IDE、操作系统相关的忽略项放进全局/用户级配置，而不是塞进每个项目的 `.gitignore`；仓库级忽略应只处理项目自身的产物。

**bryancoxwell**：分享自己大量使用 `.git/info/exclude` 来忽略仅本地有用、对协作者无意义的脚本和 Makefile。

**antisol**：做了技术澄清——区分用户级（"global"）与机器级（"system"）的 Git 配置，指出 `~/.config/git/ignore` 是按用户生效而非整台机器。

**leleat**：提到 `git update-index --[no]-skip-worktree`，可在本地实验时管理已被追踪的文件。

**Hendrikto**：认为这篇文章基本是在复述官方 Git 文档里早已有的内容。
