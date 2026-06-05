---
layout: article
title: "《Linux Basics for Hackers》学习笔记"
issue: 796
number: 30
category: books
original_url: "https://github.com/ahegazy0/linux-basics-for-hackers-notes"
hn_url: "https://news.ycombinator.com/item?id=48356468"
date: 2026-06-05
---

## 文章摘要

这是一个 GitHub 仓库，包含基于 OccupyTheWeb 所著《Linux Basics for Hackers》一书整理的结构化学习笔记。仓库被组织成一门教程式课程，共 18 个模块（编号 00–17），从基础到进阶覆盖了面向安全从业者的 Linux 知识。

仓库内容包括：原书的 PDF 副本，以及 18 个详尽模块。每个模块都配有通俗英文讲解、命令示例、参考表格、图示和练习题。

涵盖的主题从基础到高级，包括：终端基础与文本处理、网络与软件管理、文件权限与进程管理、Bash 脚本与自动化、系统管理（文件系统、日志、服务），以及安全相关主题（匿名性、无线网络、内核模块），最后是用于自动化的 Python 脚本。学习者需要 VirtualBox 等虚拟化软件和 Kali Linux 来在自己的实验环境中完成动手练习。仓库强调安全、合乎道德的学习方式，并声明所有练习都应仅限于个人实验环境。该项目已获得约 1100 个 star、102 个 fork。

## HN 评论精华

讨论中最尖锐的争议是**版权与盗版**。**InitialBP** 一开始就提醒作者应当从公开仓库中删除整本书的 PDF，"No Starch Press 是个值得保护的好出版社"。这引发了一场关于盗版伦理的长辩论：**Arainach** 坚决反对，认为"别人已经盗版了所以我也可以"就像"地上已经有垃圾所以我可以再扔"一样站不住脚，"为盗版辩护在 1997 年就已经过时，如今更显尴尬"。**iamalizard** 用更贴切的类比反驳——在同一处粘贴广告，新广告盖住旧广告，可见广告总量不变，盗版同一内容到不同站点也类似。**specproc** 则从全球视角辩护："知识是权利，而非有钱少数人的特权"，并自称是藏书逾 600 本的爱书人、买过约一千本书，引用研究指出盗版者在音乐和影视上的花费其实高于非盗版者——他只在拮据或当地难以买到时才盗版。**survirtual** 更以诗意笔调写道："树为你遮荫可曾索取报酬？知识理应自由共享，学习不是盗版，而限制知识才是暴政。"

此外，**wutwutwat** 调侃"用 PDF 分发黑客指南"很讽刺，因为 PDF 是出了名的恶意软件载体（并附上 Adobe PDF 零日漏洞的新闻）；**simoncion** 反问"如今谁还用 Adobe 软件读 PDF"。

多位用户质疑这份笔记的成色：**mzajc** 和 **drayfield** 指出仓库虽标注 (2019)，但除 PDF 外内容都是三周前的提交，行文有明显的 LLM 风格——"看起来就是有人把 PDF 丢给 LLM 让它生成 Markdown 版本，很糟糕"。**hggh**、**zokier** 等补充说这本书是 2019 年的第一版（基于 2018 年内容），第二版已于 2025 年出版（**Projectiboga** 给出了 No Starch 官方第二版链接）。

讨论也延伸出**优质 Linux 学习资源**推荐：**liendolucas** 力荐 Daniel J. Barrett 的《Linux Pocket Guide》（适合入门）和《Efficient Linux at the Command Line》（进阶）；**pss314** 推荐 William Shotts 免费提供的《The Linux Command Line》；**sas224dbm** 怀念经典的《Unix Power Tools》。**markus_zhang** 提问"用 Linux 一年却没学到多少"，**datenyan**、**opan**、**liotier** 等人给出了"动手做"的建议——买台便宜 VPS 部署服务、或托管一台 Minecraft 服务器，借此自然地学会防火墙配置、tmux/screen、vim、find 等命令，这成了不少年轻人走上系统管理之路的门径。**ldh** 留下一句犀利点评："懂 Linux 基础大概应该在自称'黑客'之前。"
