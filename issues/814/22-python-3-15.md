---
layout: article
title: "Python 3.15 正式发布"
issue: 814
number: 22
category: code
original_url: "https://www.python.org/downloads/release/python-3150/"
hn_url: "https://news.ycombinator.com/item?id=50021127"
date: 2026-10-16
---

## 文章摘要

Python 3.15.0 于 2026 年 10 月 9 日发布，是 Python 语言最新的大版本。按官方发布页的说法，相比 3.14，这个版本包含来自 1012 位贡献者的 5643 次提交，带来大量新特性和优化。发布经理依然是 Hugo van Kemenade，他担任 3.14 和 3.15 发布经理的工作由德国主权技术署（Sovereign Tech Agency）通过"主权技术奖学金"资助。下面结合发布页和官方的 What's New 文档，梳理主要变化。

**解释器层面的新特性：**

- **PEP 810：显式惰性导入。** 这是社区期待最久的特性之一。新增软关键字 `lazy`，写 `lazy import json` 或 `lazy from pathlib import Path` 时，Python 只创建一个轻量代理对象，等到第一次真正用到这个名字时才加载模块。这样既能把所有 import 集中写在文件顶部，又只为实际用到的模块付出加载成本，大型应用的启动时间可以明显缩短。如果惰性加载失败，异常会在首次使用处抛出，回溯信息同时包含使用位置和原始 import 语句。也可以通过 `-X lazy_imports=all` 或环境变量全局开启，还能用 `sys.set_lazy_imports_filter()` 只让特定模块惰性加载。限制是 `lazy` 只能用在模块顶层，不能用在函数、类体或 try 块中，星号导入和 `__future__` 导入也不能惰性化。需要兼容旧版本的代码可以定义 `__lazy_modules__` 列表。
- **PEP 814：内置 `frozendict` 类型。** 不可变字典，不是 `dict` 的子类，键和值都可哈希时它本身可哈希，保留插入顺序但比较时不看顺序。
- **PEP 661：内置 `sentinel` 类型。** 用于创建具有简洁表示的唯一哨兵值，拷贝时保持同一性，可以用 `|` 出现在类型表达式里，也能被 pickle。
- **PEP 798：推导式中的解包。** 列表、集合、字典推导式和生成器表达式现在支持 `*` 和 `**` 解包，比如 `[*L for L in lists]` 就能把嵌套列表拍平，替代嵌套推导式或 `itertools.chain`。
- **PEP 686：UTF-8 成为默认编码。** 不再依赖系统区域设置。
- **PEP 829：包启动配置文件。** 过去 `.pth` 文件里以 `import` 开头的行会在启动时执行任意代码，难以审计。3.15 引入 `.start` 文件，以 `pkg.mod:callable` 形式声明启动时要调用的入口点，`.pth` 中的 import 行开始被静默弃用。
- **JIT 显著升级。** 实验性 JIT 编译器在 x86-64 Linux 上比标准解释器的几何平均性能提升 7% 到 8%，在 AArch64 macOS 上比尾调用解释器提升 11% 到 12%。
- 错误信息继续改进。

**标准库：** PEP 799 新增专门的 `profiling` 包，把确定性追踪分析器（原 cProfile）放到 `profiling.tracing` 下，并加入代号 Tachyon 的高频统计采样分析器 `profiling.sampling`。Tachyon 可以按 PID 附加到正在运行的进程，无需修改代码或重启，采样频率最高可达 1 MHz，开销几乎为零，适合在生产环境排查性能问题，还能对挂起的进程做一次性的线程栈快照。旧的 `profile` 模块已弃用，计划在 3.17 移除。此外，更多命令行工具和模块输出带上了颜色。

**类型系统：** PEP 728 支持为 TypedDict 的额外键指定类型，PEP 747 新增用于注解类型形式的 `TypeForm`，PEP 800 在类型系统中引入"不相交基类"。

**C API：** PEP 782 新增用于构建 bytes 对象的 `PyBytesWriter` API；PEP 788 防止 C 扩展在解释器终结阶段死锁或崩溃；PEP 803、820、793 为自由线程（无 GIL）构建提供稳定 ABI，即所谓 `abi3t`，扩展模块可以编出一个同时兼容普通构建和自由线程构建、并能跨多个未来版本使用的 wheel。

**构建和发布变化：** PEP 831 默认启用帧指针，方便系统级性能观测；Windows 64 位官方二进制改用尾调用解释器；macOS 官方安装包默认安装自由线程支持。

**macOS 27.0 用户注意。** 发布页特别警告：由于 macOS 27.0 的系统行为变化，使用 tkinter 的 GUI 程序（包括 IDLE）在打开对话框类菜单命令时可能卡死（转彩球，只能强制退出），这被认为影响所有当前版本的 Tk 和所有 Python 版本。依赖 Tk 应用的用户可以考虑暂缓升级 macOS 27.0。

**彩蛋。** 为庆祝新版本，Barry Warsaw 做了一个 TUI 文字冒险游戏 whatsnewt：你在解释器内部的"启动门厅"醒来，一路走到"发布之门"，途中有 18 个谜题，每个都是一项 3.15 真实特性，必须实际写代码在 3.15 解释器里运行才能过关，涉及类型检查的谜题由 pyrefly 验证。运行方式是 `uvx --python 3.15 whatsnewt`，"当然，里面也有彩蛋"。

## HN 评论精华

本帖 317 分、110 多条评论，讨论相对平和，主要集中在惰性导入、自由线程稳定 ABI、旧版本支持策略，以及一段跑题的语言存亡之争。

**惰性导入终于来了。** kbumsik、fishgoesblub 都说等了很久。frodowtf2 问有什么理由不全局开启；ris 说那会让导入顺序变得不确定、受代码执行路径影响，zem 指出主要问题是导入时的副作用，尤其是有意为之的注册表模式。zem 还推荐了自己在工作中做的 Facebook 开源工具 lifeguard，可以扫描代码找出哪些地方能安全地开启惰性导入，赶在 3.15 发布前出了个 alpha 版。6gvONxR4sf7o 说推导式解包和 `frozendict` 是他"几乎一直想要"的小改进，tipsytoad 说从 3.6 起就纳闷为什么推导式不能解包。PaulHoule 为 PEP 686（默认 UTF-8）欢呼，hleszek 惊讶"我以为早就是默认了"，masklinn 和 zahlman 解释：Python 3.0 起源代码文件默认按 UTF-8 读取，但内置函数和标准库此前跟随系统区域设置，Windows 上尤其明显。

**自由线程的稳定 ABI。** 参与这项工作的 ngoldbaum 说他心目中的头号特性是 `abi3t`：Petr Viktorin 完成了大部分 CPython 实现，他负责确保生态系统做好准备，beta1 之前还为一个 Windows 专属问题熬过几个通宵。他很自豪 cryptography 项目已经为每个平台只发布一个 `abi3.abi3t` wheel，有 GIL 和无 GIL 的构建可以共用，因为 `PyObject` 现在是不透明的。protobuf 的维护者 haberman 表示感谢，并问 `PyUnstable_TryIncRef` 这类 API 是否会进入受限 API，因为实现弱引用值缓存需要它们；ngoldbaum 认为不太可能直接晋升，建议先在 Discourse 上发帖说明用例，还说很难从 Google 内部拿到 protobuf 和 grpcio 的官方进展。hncbw02z5a 说改发 abi3 wheel 后 CI 矩阵从 20 个任务降到 4 个。

**旧版本支持之争。** simonw（Simon Willison）说，作为一大堆开源 Python 库的维护者，他最喜欢新版本发布的一点是它意味着一个旧版本退役，这次是 3.10，于是他的库终于可以用 3.11 的特性了。ryandrake 表示不同意这种"Y 出来了就可以搞坏 X 上的用户"的策略，并自嘲是"iPhone 7 的伤心用户"。zahlman 和 simonw 解释：旧版本的包不会消失，打包元数据会让 `pip install` 自动选出与你的 Python 兼容的最新版本，永远免费支持所有旧版本会带来平方级的维护负担；simonw 说想通这一点让他终于不再为那些陈旧的安装环境发愁。nvme0n1p1 反问：3.10 已经五年多了，网页应用还该支持 IE6 吗？CJefferson 吐槽即便升级到最新 macOS 和 Xcode，自带的还是 Python 3.9.6，simonw 补充这个版本一年前就已终止支持；mrpippy 澄清 3.9 是随 Xcode 和命令行工具附带，而非系统本身。zzzoom 列出各企业发行版附带的 Python 版本（RHEL 8 仍是 3.6，支持到 2029 年），指出缺少 LTS 让很多版本实际寿命比预期长得多；throw0101a、dralley 补充 RHEL 8、9 都另外提供了较新的 Python 版本。

**其他。** zahlman 发现 3.15 的 XZ 源码包比 3.14 大了一半，原因之一是文档目录里加了两个总计 10 MB 多的 GIF 动图，演示 Tachyon 分析器的终端界面，他更希望用基于静态截图的 SVG；HackerThemAll 让他自己去贡献，bsoqk 认为这种回应不合适。simonw 推荐了 whatsnewt 游戏；ameliaquining 运行时却用上了 3.15.0a2，导致部分谜题无法完成，最后发现是一个过时的 curl|bash 方式安装的 uv 遮蔽了包管理器安装的新版本，"这就是 curl|bash 安装为什么邪恶"。alienbaby、Neywiny 反映发布页上的不少 PEP 链接是坏的。febed 觉得 Tachyon 能分析运行中的进程，对生产环境很有用。

**跑题的语言存亡论。** wg0 预言未来只会剩下 TypeScript/JavaScript、Rust 和（靠数据科学撑着的）Python，"能用 Rust 重写的东西都会被重写"，还说 Python 解释器正在转向 Rust。fastily 和 zahlman 纠正：核心团队只是在讨论允许 CPython 引入部分 Rust 代码，远谈不上"以 Rust 为主"。jack_sunsetless 说阻碍重写的从来不是代码转换，而是老业务代码没有真正描述当前行为的测试，没人能证明重写后行为一致，他至今还在为 .NET Framework 3.5 和 4.0 构建库。ciupicri 半开玩笑地问既然有 AI，何不直接生成机器码；kreneskyp、s-zeng 回答抽象的价值不会消失，好的抽象对人和语言模型同样有帮助。OutOfHere 建议用 AI 收紧类型注解，nhumrich 却说他的基准测试显示 LLM 处理无类型代码反而表现更好，引来一番争论。
