---
layout: article
title: "Ledge.sh：可以直接运行的 Markdown 笔记"
issue: 812
number: 18
category: show_hn
original_url: "https://ledge.sh"
hn_url: "https://news.ycombinator.com/item?id=49902382"
date: 2026-10-02
---

## 文章摘要

Ledge 是一款面向开发者和运维人员的 Markdown 笔记应用，特点是笔记里的代码块可以就地运行：shell 命令、Python、SQL，甚至 AI 提示词，按下 ⌘↩，输出就流式显示在代码块下方。作者 dancablam 在 Show HN 中说，他做这个是因为自己一天中大量时间都在把笔记里的命令复制粘贴到终端。他受 cmux（一个终端管理工具）启发，cmux 帮他整理好了终端，但笔记和常用命令之间仍是「割裂的两个大脑」。他从 7 月开始开发，最近几周每天用它做部署、调 API、跑冒烟测试。Ledge 基于 Bun 和 Electrobun 构建，以 Apache-2.0 协议免费开源，支持 Apple Silicon 的 Mac、Linux、Windows 11（需要 WSL）、iPhone/iPad 和 Android（测试版）。帖子获得 200 多分。

**每篇笔记有自己的 shell。** 笔记中的 shell 代码块在一个属于这篇笔记的持久 shell 里运行，`cd`、导出的环境变量、激活的 virtualenv 都会延续到下一次运行。⇧⌘↩ 可以把代码块发送到笔记底部的终端抽屉，那里是第二个可以继续手动输入的 shell。正在运行的代码块能接收输入，可以直接在输出面板里输入 sudo 密码或回答 `[y/N]`。笔记的 frontmatter 可以用 `cwd:` 和 `env:` 设置工作目录和环境变量；给代码块加 `norun` 标记则去掉运行按钮，用于只是引用、不打算执行的命令。

**笔记放在服务器上。** 用 ssh 连接一台机器，笔记文件就存放在那里，shell 也在那里运行，本地应用只是一个窗口。不需要账号、云服务，也不需要开放端口，ssh 是唯一的通道。合上笔记本电脑，命令仍在服务器上继续跑，再打开时输出已经在那等着。电脑、手机和多个窗口可以同时连同一台服务器。`ledge backup` 每小时把笔记加密备份到兼容 S3 的存储桶。手机端本身不存笔记，只读写、运行服务器上的内容，键盘上方有 ^C、^D、Esc 和方向键，方便中断命令或在 htop 里移动；配对时扫描服务器上 `ledge pair` 打印的二维码，手机用安全隔区（Secure Enclave）生成的密钥登录，这个密钥只能和 Ledge 通信。

**在其他机器上执行。** 在 frontmatter 里加一行 `host:`，笔记里的所有运行都会通过 ssh 在那台主机上执行，笔记本身留在原处。声明多个主机时每次运行都会询问选哪台。给代码块加 `confirm` 标记，运行前会显示代码、注明目标机器并请求确认，适合「清空生产环境缓存」这类危险操作。认证完全沿用你自己的 ssh 配置，包括密钥、agent、`~/.ssh/config` 和双因素认证。

**密钥不进笔记。** 「profile」是存放在笔记目录之外的具名环境变量文件，例如 `profile: deploy` 会读取 `~/.config/ledge/profiles/deploy.env`。笔记里只写名字，真正的值留在本机，因为笔记会被同步、备份，也会被 agent 读取。

**为 agent 设计。** Ledge 自带一个 MCP 服务器，用 `claude mcp add ledge -- ledge mcp` 就能让 Claude Code 等 agent 读取、搜索、创建和编辑笔记。笔记按标题寻址，改名后引用不会失效；`prompt` 代码块也是可运行的，按 ⌘↩ 就把内容交给 agent，回复显示在下方。出于安全考虑，MCP 没有删除工具，加锁的笔记也不会向 agent 暴露正文。

**加锁与同步。** 「锁定」会用口令加密笔记正文，标题、frontmatter 和标签仍然可见，链接和导航照常工作；空闲 15 分钟自动重新上锁，忘记口令无法恢复。笔记就是普通文件夹里的普通文件，没有额外数据库，所以可以用 iCloud、Dropbox、git、Syncthing 等任何方式同步。工作区可以推到 git 远程仓库与同事共享，profile 永远不进文件夹，加锁笔记以密文传输。

**其他笔记功能。** 内置支持 Python、Node、Ruby、PHP，`redis` 代码块交给 redis-cli，TypeScript 用应用自带的 Bun 运行时，添加新解释器只需在设置里加一行（比如把 `sql` 映射到 `psql "$DATABASE_URL" -f`）。还有分屏和标签页、每日笔记和模板（模板中可以用 {% raw %}`{{date}}`、`{{yesterday}}`{% endraw %} 这类占位符，配合 prompt 块可以一键生成每日简报）、搜索、标签、反向链接和 wikilink。官网列出的典型用法包括数据探索、项目运行手册、事故响应、学编程、家庭实验室管理和日记。

## HN 评论精华

评论区最大的话题是「这和已有工具有什么区别」，大量用户列出了同类产品；其次是一段关于「可执行文档为何始终没有成为主流」的深入讨论，有前 Atuin Desktop 开发者现身说法。

**和 Jupyter 比较。** **wakaru44** 说多年前他就用 Jupyter 配 bash 内核做同样的事。作者回应，Jupyter 的 bash 内核不是真正的终端会话，sudo 提示、top 之类的交互程序容易出问题，而且他更喜欢 Markdown 而不是 .ipynb。**bpshaver** 建议改掉 README 里「The notebook that runs code」这句口号，他第一反应就是「这人没听说过 Jupyter 吗？」，问题主要出在定冠词「The」上，听起来像是自称唯一。**rpdillon** 说这很有 Hammacher Schlemmer 商品目录的味道，每件商品都要冠以「The」显得登峰造极。作者接受建议，打算改成「Runnable markdown notes」（如今官网标题正是如此）。**theis_once** 提到 agent 处理 Jupyter 时常避免直接编辑文件，因为里面塞满 JSON 和图片字节，希望 Ledge 加一个类似 Jupyter「运行上方所有单元格」的模式；**jdranczewski** 推荐了把 notebook 存成纯文本的 Jupytext。

**和 Emacs org-babel 比较。** **rvdca** 问它和 Org-mode 有何不同，作者说：不需要 Emacs，用 Markdown 而不是 Org 语法，shell 默认交互，移动端也能运行。用了十多年 Emacs 的 **theCodeStig** 说这「很出色」，正好解决了他无法在团队里用 org-babel 的痛点，称之为「给麻瓜用的 org-babel」：Org 比 Markdown 早，但它的致命弱点是绑定 Emacs，他不能也不应该强迫同事用某个编辑器，一个 Emacs 用户往仓库里提交 org 文件就会造成断层；而 Markdown 无处不在，没装 Ledge 的人也能看，在纯文本编辑器里也比 org 好读。**mfld** 做研究时大量使用 org-babel，把数据处理代码、图表、公式和笔记放在一个文档里，再导出 PDF 发给合作者，建议 Ledge 也考虑这条路；**IanCal** 补充了 Marimo、RMarkdown、Quarto 等同类工具。

**可执行文档为什么没流行起来？** **edude03** 提到 Atuin Desktop 也做过类似的事，但似乎已经不再维护，问这类工具为什么一直没火。**junehwi** 也问，org-babel、Jupyter、Atuin Desktop 都探索过这个方向，却都没成为主流工作方式，是体验问题，还是人们本就喜欢把文档和实际工作分开？曾大量参与 Atuin Desktop 开发的 **ellieh** 给出了三点原因：一是编写运行手册很难，大多数人连基本文档都不写，更别说可执行的；二是让运行手册跨系统保持更新很难，容易出错；三是 agent 现在已经能很好地完成这些工具想解决的许多任务，还能绕过文档过时带来的问题。她总结说，这些工具用来记个人笔记很好，但很难规模化。巧合的是，**drayfield** 发现 Atuin Desktop 的仓库恰好在这条 Show HN 发布当天被归档，作者表示与 Atuin 毫无关系，纯属巧合。

作者对此的看法是：Ledge 的赌注在于它不该让人感觉在「编写」什么，他的 Markdown 笔记本来就塞满了命令，让它们可运行只是顺理成章；他也承认保持文档更新很难，希望内置的 agent 支持能帮忙维护，团队共享笔记仍是未解决的问题。**rancar2** 给出更悲观的经验：想把这类工具做得可持续，最终会陷入构建系统和依赖映射，想要通用就会造出类似 Homebrew 的东西，跨系统的依赖、更新和配置会引发「疯狂的连锁 bug」。**frumiousirc** 用 org-babel 十多年，总结了规模上来后的问题：需要 make 这样的依赖图，或让每条命令幂等；文档膨胀到库的规模后无法组合；同事不用 Emacs，org 文件成了「个人文件」。他现在的做法是让 LLM 把 org-babel 外置：LLM 维护文档、一个生成结果的外部库和 Makefile，他自己再另写笔记，确保信息经过眼睛和手指，保留一定的理解。他甚至认为，给 LLM 访问源码并通过问答学习，比读一份预先写好的文档有效得多。

**「这是反模式」。** **Piraty** 提出明确反对：可以用它探索自己的流水线，但不建议用于严肃项目，把重要的代码块放在 Markdown 里是反模式。他的做法是，发现自己重复使用笔记里的代码，就把它变成 dotfiles 或项目 scripts 目录里的脚本，再用 Makefile 的通用目标调用，最终只需 `make release`，可移植，也不会给每个人增加一个宿主依赖，「更不需要一个 JavaScript 引擎」；复制粘贴的工作流很难用，那就改进它，vim 有 `:Terminal`，也能复制到剪贴板。作者回应，Ledge 并不取代脚本和自动化，复杂的部署流程中间仍有很多人工步骤，他用 Ledge 运行自动化脚本、做人工检查，再根据结果运行下一步。

**同类工具大合集。** 评论里提到的相似项目非常多：Runme（**akavel** 研究运维手册类工具时最喜欢它，支持环境变量参数化、CLI，并且标志写在代码块上而非 frontmatter；作者说他需要移动端支持，也觉得依赖 VS Code 太重，并为参数化和从 CLI 运行代码块各开了 issue）、Atuin Desktop（**KetoManx64** 说它用 Tauri 写的界面性能极差，一打开风扇就狂转）、xc（**20after4** 用它管理家庭基础设施，看重它是单个 Go 二进制、有内置依赖系统，认为可运行 Markdown 是 Knuth「文学编程」理念的延续）、mask、已经停止开发的 xiki、Fiberplane 转型前的产品、Observable Framework，以及几位评论者自己做的 piary.dev、bsh.nvim、Brashtag 等。还有好几人问能不能做成 Obsidian 插件。

**功能建议与使用反馈。** **tetraodonpuffer** 希望编辑器里也能用上 shell 补全、别名和 Ctrl-R fzf 历史搜索，作者说这些在终端抽屉里可用，编辑器里还没有。**IanCal** 很喜欢「什么都不做的脚本」（do-nothing scripts）这种文档化流程的方式：先只提示你手动做事，再逐步自动化，他预计作者很快会遇到非线性流程的需求，比如依赖、条件、并行、失败后继续还是重跑，建议尽早想清楚项目要做什么和明确不做什么。作者承认自己也遇到过根据输出走向完全不同分支的情况，目前由用户自己判断下一步。**nullbio** 打算用它存放常用提示词，觉得比 skills 更透明，并建议加入钩子让 agent 自动更新任务状态；他起初在 Ubuntu 上遇到严重卡顿，作者专门开了 issue，后来他发现是自己显卡驱动的问题。**Gabrys1** 调侃说，本以为终于看到一个不提 AI 的项目，结果马上看到「Built to be worked by agents」。作者说明 Ledge 本身没有内置 AI，是「自带 AI」模式，需要的人自己接入。
