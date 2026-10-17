---
layout: article
title: "你 PATH 里的波浪号可能并不指向你的 HOME"
issue: 814
number: 23
category: code
original_url: "https://disconnect3d.pl//2026/10/02/dont-put-tilde-in-your-path/"
hn_url: "https://news.ycombinator.com/item?id=50042425"
date: 2026-10-16
---

## 文章摘要

这是安全研究者 Disconnect3d 在 10 月 2 日写的一篇短博文，讲的是一个很多人可能一直没察觉的 shell 陷阱。

**起因。** 作者在试用智能体沙箱工具 nono 时，它给出了一条警告："沙箱可写入的 PATH 条目：`~/.local/bin/`"。这看起来很可疑：为什么 PATH 里会出现一个字面意义上的 `~`？

**问题所在。** 很多人会在 `~/.bashrc` 或 `~/.zshrc` 里这样写：

```sh
export PATH="$PATH:~/.local/bin/"
```

这里的 `~` 并不会被展开成家目录（`$HOME`），因为波浪号展开只发生在**未加引号**的输入中。作者引用了 Bash 文档：如果一个单词以未加引号的波浪号开头，那么直到第一个未加引号斜杠之前的字符都被视为"波浪号前缀"；Bash 还会检查每个变量赋值中紧跟在 `:` 或第一个 `=` 之后的未加引号波浪号前缀，并对其进行展开。由于整串内容被双引号包住，`~` 原封不动地进入了 PATH。结果是 PATH 里多了一个**相对路径** `~/.local/bin/`，也就是"当前目录下名为 `~` 的目录里的 `.local/bin`"，而不是 `/home/<用户名>/.local/bin/`。

**正确写法** 是用 `$HOME`：

```sh
export PATH="$PATH:$HOME/.local/bin/"
```

作者也提到，不加引号的 `export PATH=$PATH:~/.local/bin` 在 Bash 和 Zsh 里其实能正常工作，因为变量赋值中 `=` 之后和每个 `:` 之后的波浪号也会被展开；但依赖这一点很脆弱，比如路径里出现空格就会破坏赋值。

**演示。** 他在一个空目录里创建了 `./~/.local/bin/`，编译一个打印 hello 的小程序放进去命名为 `kek`，然后执行 `PATH="~/.local/bin/" kek`，程序真的被找到并运行了，全程跟家目录毫无关系。这也说明了安全隐患所在：只要当前目录下恰好有（或被人放了）一个名为 `~` 的目录，里面的程序就可能被当作命令执行。

**自查方法。** 用 `echo "$PATH" | grep -- '~'` 或 `echo "$PATH" | tr ':' '\n' | grep '~'` 检查，有输出就去改 `.bashrc`、`.zshrc` 或 `.profile`，把 `~` 换成 `$HOME`。文末他还点赞了 nono 发出这条警告，只是觉得警告可以写得更详细些，并表示会提交 PR。

## HN 评论精华

本帖 253 分、180 多条评论。讨论从波浪号展开的精确规则出发，延伸到 PATH 顺序的安全性、Bash 引号规则的"随心所欲"、要不要换个更好的 shell，以及几则惨痛的亲身经历。

**"Bash 第 238503 号坑。"** IshKebab 的一句话概括了不少人的感受，_ZeD_ 补充 zsh 也一样。paulddraper 说这就是标准 POSIX：波浪号只在未加引号的单词开头展开，`~user` 展开为该用户的家目录，Bash 另有 `~+`（当前目录）、`~-`（上一个目录）等扩展。FeepingCreature 质疑：你的 `~/.local/bin` 不起作用，难道不会察觉？ndsipa_pomu 回答那里面通常没几个命令，可能要等装了 yt-dlp 之类才会发现。

**到底怎么写才对？大家自己也吵了起来。** amarshall 建议干脆别给波浪号加引号，大家条件反射地给"字符串"加引号，却没意识到 Bash 里几乎一切都是字符串。paulddraper 给出一条通用规则：只给含 `$` 的表达式加双引号，只给含特殊字符的字面量加单引号；但他又说作者的例子换成 `export PATH="$PATH":~/.local/bin` 也修不好，因为波浪号只在单词开头展开。Izkata 立刻实测反驳：这样写其实能用，Bash 对 `:` 后面的波浪号有特殊处理，但 `"$PATH:"~/.local/bin`（冒号放进引号里）就不行。kragen 先说不加引号也没用，`echo $PATH:~` 输出的仍是字面 `~`；mjmas 指出这条规则只适用于变量赋值，kragen 认错，称这些规则是"Calvinball"（《卡尔文与霍布斯》里规则随时变的游戏）。chasil 一开始给出的写法在 mksh 上测试后被 yencabulator 指出 PATH 里仍是字面 `~`，他查阅 POSIX 标准 2.6.1 节后确认，标准允许在"未加引号的冒号之后"展开波浪号，最简洁的合规写法是 `export PATH="$PATH":~/.local/bin`。这一串来回本身就很好地说明了问题有多微妙。amarshall 推荐用 ShellCheck，它会提示引号中的波浪号不会展开。vbernat、russellbeattie 提到 zsh 可以用 `path+=(~/.local/bin)` 操作数组，kccqzy 说 fish 有专门的 `fish_add_path`；JdeBP 补充 BSD 的 `login.conf` 也会展开路径项开头的波浪号，适合多种登录 shell 并存的情况。SoftTalker、ndegruchy 的做法最省心：脚本里一律用 `$HOME`、`$XDG_DATA_HOME`，波浪号只在交互时用。

**PATH 该放前面还是后面？** gjvc 认为文章示例不好，应该把 `$HOME/.local/bin` 放在 PATH 最前面。Backslasher 说习惯上让系统路径优先，是为了防止恶意脚本伪装成 `sudo` 窃取密码、把一次本地文件写入升级成 root 权限。toast0、paulddraper、bityard 反驳：放在后面就无法覆盖老旧的系统命令，而且能往你 PATH 里写东西的攻击者多半也能改你的 shell 配置；3eb7988a1663 指出 uv、mise、asdf、Python 虚拟环境、nix shell 都会把自己插到 PATH 最前面。account42 建议真要覆盖系统命令就在 `.bashrc` 里用别名，以免影响其他脚本。GrinningFool 则说他越来越担心有同名程序被塞进 `~/.local/bin`，于是改放到了最后。

**该不该换掉 Bash？** vips7L 说干脆别用 Bash。CodesInChaos 指出一半的问题不在 shell，而在 Unix 向进程传参的约定；akoboldfrying 说 Windows 那种把整行命令作为一个字符串传给进程的方式更糟，引号解析取决于编译器厂商的启动代码；ChrisSD、CodesInChaos 还讨论了以 `-` 开头的文件名与选项无法区分、`--` 支持不一致的问题。murex shell 的维护者 hnlmorg 说他研究过很久，发现无论 shell 做得多聪明，一调用 `fork()` 就全没了。malux85 推荐 fish、zsh、nushell，其他人还提到 Xonsh、PowerShell；但 bornfreddy、lelandbatey、account42 的反驳很现实："最好的 shell 是几乎每台需要用 shell 的机器上都有的那个。"m463 的规则是：简单脚本用 shell，超过一定复杂度就换 Python，他最怕评审同事用 AI 生成的一页又一页晦涩 Bash 脚本。

**对"RTFM"的分歧。** Grimeton、arkt8 认为这就是不读手册的典型，ultraboom 说 Bash 手册全面、精确、写得很好。3eb7988a1663 回应：常用软件里有大量写在文档里却照样坑人的陷阱，要求每个人精通工具的每个角落并不现实。Cockbrand 说他本以为这是常识，但这篇文章恰恰证明它不是；MathMonkeyMan 说他共事过的人几乎没人懂 shell 是怎么工作的。

**惨痛经历和延伸话题。** 最让人心惊的是 em-bee 的故事：他当新手时用一个 Perl 镜像工具，配置里写了 `~/somewhere`，结果生成了一个字面名为 `~` 的目录。他想删掉它，随手敲了 `rm -r ~`，然后等啊等，纳闷删一个几乎空的目录怎么这么慢……他的教训是：模糊路径一律加 `./` 前缀（`rm -r ./~` 就没事），能用 `rmdir` 就别用 `rm -r`。very_good_man 说早期 Cursor 曾在他的仓库根目录建了个名为 `~` 的目录，收拾起来很吓人。quotemstr 原以为文章会讲另一个坑：`HOME` 环境变量和 `/etc/passwd` 里记录的家目录可以不一致，`~` 用前者，`~foobar` 用后者，各语言运行时又各取所需。ButlerianJihad 考证波浪号源自 csh，csh 写脚本很糟糕，但波浪号却是它的成功之作，后来被几乎所有 shell 甚至网址采用。duncangh 自嘲："早知道该先读文档，再把 `~/` 纹在手腕上。"lysium 回复：只要没加引号，你就没事。ChrisArchitect 则指出了一个小讽刺：这篇文章的网址里本身就有个奇怪的双斜杠。
