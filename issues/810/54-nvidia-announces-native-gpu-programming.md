---
layout: article
title: "英伟达宣布支持用 Rust 原生编写 GPU 内核"
issue: 810
number: 54
category: startup_news
original_url: "https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/"
hn_url: "https://news.ycombinator.com/item?id=49724881"
date: 2026-09-18
---

## 文章摘要

2026 年 9 月，英伟达在开发者博客上宣布正式投入 CUDA Rust，并计划在 2027 年及以后持续培育它。官方的论证起点是：AI 的系统层——推理引擎、服务基础设施、驱动、agent 运行时——变动极快，其中越来越多的部分正在用 Rust 编写，因为它能在编译期消灭一整类 bug 而不牺牲性能。英伟达自己也在这条路上：Nova Linux 驱动用 Rust 写，NVIDIA Dynamo 的核心是 Rust，NVTX 有 Rust 绑定。唯一的例外是 GPU kernel 本身——你可以从 Rust 发起 kernel，但 kernel 通常还得用别的语言写。CUDA Rust 要补的就是这个缺口：kernel 用 Rust 写，原生编译成 PTX，而不是包一层别处的代码。

它提供两条轨道，对应 CUDA 本身的两种编程模型：

**SIMT 轨道 —— cuda-oxide**，这是一个自定义的 rustc codegen 后端。它拦截编译过程，把标注了 `#[kernel]` 的函数经由 Rust MIR、社区的 Pliron IR 框架和 LLVM IR 一路送到 PTX，其余代码交回标准后端；其上的 GPU dialect 与所有变换都是英伟达自研且完全用 Rust 实现。要求较重：Linux、计算能力 8.0 以上的 GPU、CUDA 12.x 以上、clang 及其 libclang 头文件，以及一个固定版本的 nightly 工具链。`cargo oxide doctor` 会检查这些依赖，`cargo oxide new` 生成的模板就是一个完整可跑的向量加法程序，主机代码和设备代码写在同一个文件里，一条命令构建，不需要单独的 kernel crate。

它的安全论证全写在 kernel 签名上：输入是普通的共享切片，输出是 `DisjointSlice<f32>`，这个类型只把当前线程自己的那一个元素的独占访问权交给它；线程索引由 `thread::index_1d()` 返回一个索引类型而不是裸整数，配套的取值方法只接受这个类型并返回 `Option`，越界因此成为一个你必须处理的分支，而不是事后才发现的内存错误。启动同样是被校验而非被信任的：`#[launch_contract]` 声明这个 kernel 按一维索引、每块 256 线程，准备函数会拿你的启动配置去比对这份声明和设备的实时上限，然后交回一张"凭证"，安全的启动方法必须收到这张凭证才肯执行。

**Tile 轨道 —— cutile-rs**，工作在更高一层：你对 tile（子张量）而不是标量做运算，每个 tile block 把 kernel 体当作单个逻辑线程执行一次，由编译器决定背后用多少真实 GPU 线程。要求轻得多：CUDA 13.3、稳定版 Rust 1.89 以上、Linux，不需要 nightly，也不需要自备 LLVM，直接 `cargo add cutile` 即可。主机侧的 `.partition([128])` 一行同时做三件事：让每个 tile 独占自己的 128 元素块（独占性因此是真实的）、把网格固定为 1024/128 = 8 个 tile、并提供静态的 tile 宽度参数。宏生成的 launcher 会取得三个张量的所有权并在 GPU 完成后原样交还；在调用同步之前，一切——包括创建张量、kernel 调用、乃至拷回主机——都只是被记录下来的惰性描述，整个程序是一条只有单个同步点的链。

文章用同一个 1024 个 float 的逐元素加法在两条轨道上并排演示，两者都是完整可运行的程序、输出同一行结果。在"编译器能抓到什么"一节里，两边都能在编译期拒绝经典的别名错误：SIMT 那边把输出缓冲区同时作为自己的输入传入会触发借用检查错误，Tile 那边同样的错误则触发"使用了已移动的值"。作者明确指出两者划线的位置不同——cuda-oxide 检查的是每一次启动调用，而 cutile-rs 的所有权跨越了启动边界跟随张量，这是更强的主张。Tile 之所以"构造上安全"，是因为它根本不给你共享内存和线程索引可搞错，而这也正是你交换掉的控制力；SIMT 保留了这份控制，目前那边的共享内存仍需要 unsafe。

官方的建议是：优先选 Tile，需要控制内存与线程时再下沉到 SIMT；语言的选择和模型的选择是两个独立问题，英伟达计划支持 CUDA Rust、CUDA C++ 和 CUDA Python 之间的跨语言互操作。现状方面文章没有粉饰：两个项目都是早期，都还不能用于生产。cuda-oxide 处于 early alpha；cutile-rs 走得更远，已发布到 crates.io，并已被 HuggingFace 的 Grout 推理引擎和 mistral.rs 在英伟达之外实际使用。

## HN 评论精华

956 分、401 条评论。这条帖子的走向相当出人意料——技术讨论虽然扎实，但票数最高、分支最长的那条线，讨论的其实是这篇公告本身是 AI 写的。

**"连英伟达都这样了"。** **claiir** 只引用了原文一句被动语态的短句，配上一句"连英伟达都在发纯 Claude 写的文章了"，就点燃了全场。**manyatoms** 补刀说页面上还配了个 AI 生成摘要框，**greenavocado** 称之为"递归摘要金字塔"。
- **jorl17** 写出了其中最具体的批评：他最受不了的是那种"语言的拟人化"——每一个东西都在做、产生、感受、想要、询问、回答；启动被检查，问题被提出，实现在回答，模型想要，结果在命名，连接在浮现……"每一个该死的东西都是活的，都有欲望，都在做事情。"
- **keybrd-intrrpt** 觉得"连英伟达"这个说法很奇怪：他们全面押注 AI 做一切，这就像说"连麦当劳都在卖不健康食品"。**jchw** 的回应引出了一场长辩论：他说即便是 Anthropic 似乎也不用 Claude 写博客文章。对方断言 Anthropic"绝对在用"，只是藏得更好；jchw 要求拿出证据，双方就"举证责任该有多重"来回拉扯了很久，**rkharsan64** 引用了此前的讨论称 Anthropic 雇用不使用 LLM 的撰稿人。
- **daemonologist** 提供了一个具体观察：他一年多前就在英伟达一些小项目里看到过完全由 AI 生成的"文档"——那还是这么做完全不是好主意的时候。**DonsDiscountGas** 的回应"反正以前也没人真读文档，现在是 agent 在读，它们读 LLM 写的东西毫无障碍"引发了强烈反弹，**WD-42**、**californical**、**latentsea**、**Vegenoid** 纷纷表示自己天天读文档，其中 Vegenoid 说："我职业生涯中最大的、始终让我区别于同侪的技能，就是认真读文档。这有多大的区分度令人震惊——你会发现你已经在用的软件，远比你以为的强大。"
- **nicebyte** 给出了最刻薄的一句总结：这篇文章告诉我的是，英伟达没有任何人真正在乎这个项目，否则他们会让一个人来写这份公告。

**技术上的实质讨论。**
- **rvz** 提出了务实的担忧：这是需要 nightly 编译器的 pre-1.0 软件（若选 SIMT 轨道），本身就不稳定；而且出问题时你现在要在 cuda-oxide、Rust、CUDA、Tile 四层之间定位问题，又多了一层依赖，而除 CUDA 本身外全是开源的——一旦问题出在 CUDA 那层，你只能等。
- **the__alchemist** 从实际用户角度对比：cuda-oxide 相当于 cudarc 的主机端，但用的是 Rust 风格的 kernel 方言，好处是主机与设备可共享 struct，坏处是拿标准 CUDA kernel 换了一个正在开发中的新方言。**embedding-shape** 补充说 cudarc 的好处正是它与 CUDA 一一对应，能直接查现成的示例和参考。
- **jacobgorm** 唱了全场最大的反调（并被大量踩到灰色）：他认为没人在乎 kernel 是不是用 Rust 写的，kernel 天然简单，用 Triton 这类能优雅抽象 tile 尺寸的 DSL 更好；他还表达了对 CUDA 的强烈反感——一旦让这种专有东西进了你的 C++ 代码库就很难摆脱，最后要么绑死单一厂商，要么陷入 `#ifdef` 地狱，多半两者兼有。他主张正视 GPU 与 CPU 不是同一台机器这个事实，把 kernel 写在独立文件里手动启动，像 Metal、OpenCL、D3D12 那样。**bigyabai** 反问 D3D12 和 Metal 难道就不专有了。
- **keithnz** 的回应被顶得很高：kernel 并不"注定"该用某种语言写，C 只是从汇编接手过来的一个传统上不错的默认选项，没有理由必须一直用它。
- **pjmlp** 贡献了多段历史视角：OpenCL 之所以在采用率上失败，恰恰是因为它执着于 C 而不像 CUDA 那样多语言；在 SPIR 出现之前，Khronos 根本不理解为什么有人会关心 C99 以外的东西，或者为什么 GPU 上支持 Fortran 有意义。他还坚持 CUDA 既不是 API 也不是语言，而是一整个生态；**fc417fc802** 把这句翻译成"这是在委婉地说它是个依赖大泥潭"，并追问为什么不能像 CPU 那样直接暴露 GPU ISA，引出了关于 PTX 究竟是 ISA 还是字节码、以及 GPU 厂商为何不愿固定 ISA 的讨论。
- 替代方案被反复提及：**melodyogonna** 推 Mojo（有人确认编译器已在一个多月前以 Apache 2.0 开源）；**adgjlsfhk1** 和 **patagurbon** 推 Julia，称其 CUDA 生态成熟得多，只是核心开发者太少；**zackmorris** 演示了 Julia 里同一个函数不加任何标注、只把 `Array` 换成 `CuArray` 就能跑在 GPU 上，认为这才是应该去的方向，相比之下英伟达的 Rust 移植是演进而非革命；**laggui** 介绍了 Burn 使用的 CubeCL，可以用 Rust DSL 写 kernel 并同时面向 CUDA、AMD、Metal、Vulkan 和 WebGPU。
- **dllu** 指出英伟达已经收购了 HuggingFace，而后者的 Candle 是优秀的 Rust 推理库，这一步顺理成章；**melihelibol**（看起来是项目相关方）回复说与 Candle 的集成已经有 PR 了，并澄清 cuda-oxide 目前仍只支持 Linux 但不再强制 async。

**一条意料之外的支线。** **LarsDu88** 说在这个什么都由 LLM 写的时代，他学 Rust 的动力被磨得差不多了，而这个消息重新点燃了他的兴趣——理由仅仅是"LLM 还没在这上面训练过"。**cmrdporcupine** 遗憾地打破了幻想：他已经让 LLM 写了一堆 cuda-rust 代码，把若干 C++ CUDA kernel 移植过去并调到了同等性能，用的还不是前沿模型。这条线随后滑向了关于"学新语言的回报是否已经贬值"、"务实派 vs 园丁派程序员"、以及 LLM 到底适不适合当老师的长篇争论——**tete** 列举了 CORS 缓存、Content-Disposition 等模型反复给出错误答案的具体例子，主张回到官方文档和自己动手；**suresk** 则坚持它是一位知识渊博、无限耐心、随叫随到的导师，学习远不只是获取信息，还包括建立心智模型、消解歧义、探索原始资料讲不清楚的地方。
