---
layout: article
title: "SSH 隧道实战指南：本地与远程端口转发"
issue: 800
number: 21
category: code
original_url: "https://labs.iximiuz.com/tutorials/ssh-tunnels"
hn_url: "https://news.ycombinator.com/item?id=48606222"
date: 2026-07-03
---

## 文章摘要

这是一篇讲解 SSH 隧道（SSH tunnels）的实用教程，系统梳理了本地端口转发与远程端口转发这两大核心机制。**本地端口转发（`ssh -L`）** 让你把一个远程服务映射到本地端口上访问，语法为 `ssh -L [local_addr:]local_port:remote_addr:remote_port [user@]sshd_addr`；**远程端口转发（`ssh -R`）** 方向相反，把本地服务暴露到远程网关的端口上，语法为 `ssh -R [remote_addr:]remote_port:local_addr:local_port [user@]gateway_addr`。文章给出了一个便于记忆的口诀：`-L` 中的 L 代表 **L**ocal 一侧在监听，`-R` 中的 R 代表 **R**emote 一侧在监听。

文章还介绍了两种动态变体：**动态本地转发（`ssh -D`）** 把 SSH 客户端变成一个 SOCKS 代理，无需事先指定每一个目标地址；**动态远程转发（不带目标地址的 `ssh -R`）** 则把 SSH 服务端变成 SOCKS 代理，实现更灵活的路由。

在实际应用层面，SSH 隧道可用于访问私有数据库和内网 Web 应用、把家庭网络的服务对外暴露、经由堡垒机（bastion host）访问 VPC 内部端点，以及通过跳板机（jump host）转发调试端口等场景。整体而言，这篇教程属于"读一遍手册就能学会"的扎实基础运维知识。

## HN 评论精华

讨论中最热闹的话题是"文章没提到跳板/ProxyJump"这一遗漏。

- **chasil** 指出文章讲了堡垒机却没提跳板功能 `ssh -J user1@bastion1,user2@bastion2 targetuser@targethost`，并补充这个特性早在 2016 年 OpenSSH 7.3 就引入了。**dspillett** 附和说这类教程内容多年来反复出现，配图几乎一模一样，却始终没更新加入跳板主机——如今连 Debian/LTS 这类保守发行版都早已支持。
- **saltcured** 分享了更优雅的做法：不用命令行参数，而是在 `~/.ssh/config` 里写 ProxyJump 规则，且它能递归生效——目标主机的规则指向 bastion1，bastion1 的规则再指向 bastion2，比手动拼接多级参数更易维护。**05** 进一步展示用 `Match ... exec` 规则实现"按需 VPN"：根据当前是否在家自动决定是否走跳板。
- **idatum** 点出 `-J` 的一个妙处：跳板主机无需拥有对最终主机的任何权限。他的方案是只有笔记本持有 SSH 密钥，家庭服务器从 FreeBSD jail 里向廉价 VPS 建立反向隧道，笔记本再通过该端口跳转，从而实现家庭网络零开放端口。
- 关于学习方式，**teddyh** 调侃"读读手册能学到的东西真惊人"，而 **trollbridge**、**GL26** 等人则代表了新潮流：直接让 Claude 之类的 AI 边写代码边讲解概念。
- **buredoranna** 安利了一个冷门技巧：连接中按回车后输入 `~C` 可进入 SSH 命令行动态添加端口转发（新版默认禁用，需 `EnableEscapeCommandline yes`），**telotortium** 补充说救命的 `~.`（终止卡死会话）即使在禁用状态下也仍可用。此外 **wbadart** 推荐了《The Cyber Plumber's Handbook》作为进阶读物，作者 **opsdisk** 本人也现身致谢。
