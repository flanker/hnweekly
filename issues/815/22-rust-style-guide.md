---
layout: article
title: "Google 发布 Rust 风格指南"
issue: 815
number: 22
category: code
original_url: "https://google.github.io/styleguide/rustguide.html"
hn_url: "https://news.ycombinator.com/item?id=50186010"
date: 2026-10-23
---

## 文章摘要

Google 在其公开的风格指南仓库中加入了 Rust 风格指南。Rust 已在 Google 内部的 google3 主仓库、Android、ChromeOS、Fuchsia 和 Chrome 中使用，这份指南的目的是让这些代码库内部和彼此之间的 Rust 用法保持一致，并树立质量标准。这里的「风格」不只指格式，还包括 API 设计和实现层面的指导。部分条目带有 [chrome] 或 [Fuchsia] 标签，只适用于特定代码库。

**目标与总体原则。** 指南列出几条目标：可读性和可维护性优先于好写；规则要便于自动化，因为 Google 经常做由工具辅助、横跨整个代码库的「大规模变更」（LSC）；尽量与更广泛的 Rust 生态保持一致；每条规则都要「物有所值」；还要减少评审中无谓的风格争执。基础是遵循官方 Rust 风格指南和 Rust API 指南，格式交给 rustfmt（例如每行最多 100 列、十六进制字面量用小写）。只有手动对齐（比如矩阵构造）和生成代码可以用 `#[rustfmt::skip]` 跳过格式化，个人偏好不算理由。指南还区分「库代码」和「应用代码」：API 设计规则对库代码有约束力，对应用代码则是强烈建议，后者应偏向简单直接，避免过早泛化。

**代码组织。** Chromium 偏好小 crate，最好一个文件一个抽象，因为 crate 是编译单元，细粒度目标有利于并行编译和构建缓存；一个 crate 的源码要放在同一个目录，不使用子目录，觉得需要子目录就说明该拆分了。叶子模块以其主要类型命名；非叶子模块只放模块声明和再导出。禁止 glob 导入（`use super::*` 等少数情况例外），也不鼓励自定义 prelude，因为这会给做大规模变更的人增加负担，也让人难以判断类型来自哪里。一条 use 语句只从一个模块导入；trait 尽量在函数内部局部导入；调用其他模块的顶层函数要带模块名（写 `mem::transmute` 而不是直接 `transmute`）；类型名则导入后直接使用（写 `Regex` 而不是 `regex::Regex`），有歧义时可以写 `io::Error`。

**所有权与函数签名。** 优先使用唯一所有权和借用，避免 `Rc` 和 `Arc`，除非替代方案复杂到难以编写和维护；不要用 `.clone()` 代替借用。参数接受最灵活的引用：用 `&[T]` 而非 `&Vec<T>`，用 `&str` 而非 `&String`，用 `Option<&T>` 而非 `&Option<T>`。可复用的库代码在可能时返回迭代器、接受 `impl IntoIterator`，应用代码直接用 `Vec` 和切片也可以；`impl Into<T>` 参数在库中可酌情使用，应用代码几乎不该用。参数含义不明显时用 enum 代替 bool。错误处理上：原因显而易见、调用方不需要细节的用 `Option`；需要区分失败原因、可恢复的外部错误用 `Result`；bug 和违反不变量用 `panic!`。

**类型与 trait。** 语义不同、需要独立 API、可能更换底层类型或容易混淆时用 newtype 而不是类型别名；优先使用具名字段而非元组。能用标准库 trait 就别自定义方法（实现 `FromStr` 而不是写 `from_str`），能 derive 就别手写。库函数参数优先用泛型而非 `dyn Trait`，代码体积敏感时例外。只有在需要泛型约束、`impl Trait`/`dyn Trait` 或扩展 trait 时才定义 trait，不要只为组织代码而用 trait；扩展 trait 要加 `Ext` 后缀，并由原类型的所属团队维护。指南还对常见 trait 给出建议：错误类型尽量实现 `Copy`；有 `new()` 的类型应实现 `Default`；作为选项集合的结构体应实现 `Default`，以便使用结构体更新语法。

**并发、宏与命名。** 绝不访问已中毒（poisoned）的互斥锁内部；Chromium 中不使用 async/await，也不通过标准库直接开线程，而用 Chromium 自己的 sequence；Fuchsia 使用 `fuchsia_sync::Mutex`。表达 API 时依次优先：普通函数和类型，`macro_rules!`，其他代码生成，最后才是过程宏，因为宏会造出语言「方言」，让 IDE、代码搜索和大规模变更都更难。命名方面：生命周期要有描述性名称；泛型参数名应提示其约束；构造函数用 `new()`、`new_with_foo()`，返回 Result 的用 `try_new()`；避免项目外的人看不懂的缩写。getter 直接叫 `foo()`，可变版本 `foo_mut()`，setter 叫 `set_foo()`，另有 `take_foo()`、`replace_foo()` 以及接受闭包的 `with_foo()`。

**其他规则。** 只有在安全代码无法实现时才写 unsafe，要封装在安全抽象里，加上 `SAFETY:` 注释并说明理由，如果是为了性能需要有基准测试支撑。不使用不稳定特性（Android 嵌入式等有例外）。实现了 `len()` 就要实现 `is_empty()`；不要访问别人标记为 `#[doc(hidden)]` 的条目；公开的 const 函数要确保未来仍是 const；不要为了语法糖而重载运算符。面向其他团队的公开 enum 和带公开字段的结构体要标 `#[non_exhaustive]`，除非它们描述的是固定不变的概念，或者你愿意通过大规模变更修复所有破坏。不要在 main() 之前运行代码。实现层面：不要过度依赖类型推断，`.into()`、`.parse()` 和复杂迭代器链的结果要标注类型；用模式匹配代替「先检查再 unwrap」；用 `expect` 代替 `unwrap` 说明原因；断言加上上下文字符串；迭代器组合链太长或带副作用时改用 for 循环，也不要把组合链写在 for 循环头里；禁止使用 `panic::catch_unwind`（Chrome、Android、Fuchsia 都要求 panic=abort）；拼接多个字符串用 `format!`。

## HN 评论精华

本帖 382 分、220 多条评论。讨论开头被「有 AI 了谁还看风格指南」的言论带偏，又引出一长串 C/C++ 与 Rust 的存亡之争；回到指南本身时，争论最集中的是 `#[non_exhaustive]`、小 crate、`Rc`/`Arc` 和 getter/setter 命名。

**AI 时代还需要风格指南吗？** 首条评论 Thanemate 说如今看风格指南的唯一理由是喂给模型，「我现在是我的 agent 大军的将军」。u1hcw9nx 原样戏仿了一遍回敬，anonymous908213 问他用 agent 大军做出了什么。monocasa 认为风格指南的意义在于让认知方式各异的人在同一仓库里写出可读代码，在 agent 时代反而更重要，「糟糕的将军无法有效评审部下的工作」。smoe 指出指南第一段就说了它包含 API 和实现设计，不管代码是谁写的，可预测的行为都有价值；nimih 补充学新语言时这类指南仍是很好的起点。yodsanklai 说他们公司确实把指南喂给模型，但模型并不总是遵守，代码太多无法人工把关，大部分还被 AI 自动批准，「这是一场注定失败的仗」。threethirtytwo 认为把整份指南塞给 LLM 只会浪费上下文。

**#[non_exhaustive] 之争。** 这是关于指南内容最热烈的讨论。japgolly 认为这条建议很疯狂：enum 的威力就在于新增变体时编译器会标出所有需要更新的 match，加了通配分支就把编译期错误变成了运行时问题。zelphirkalt 也认为被强制更新正是严格 enum 的好处，「我们不是 Google」。支持的一方 zeroCalories、smallnamespace 指出，在 Google 这种规模下，修改一个被几十上百个团队使用的 enum 的人根本不知道如何更新所有调用点，最好把 enum 变更和调用点更新解耦，让各团队按自己的节奏跟进。demurgos 分享了亲身经验：他所在公司有庞大的 Rust 代码库和数千个接口，以前手写的 API 大量使用穷尽式 enum，但并不能防止旧客户端收到新服务端发来的新变体，semver 主版本升级在 Rust 名义类型下又很难集成；他主导改为从 IDL 生成代码并默认 `non_exhaustive`，配合 `non_exhaustive_omitted_patterns` lint 在 CI 中检查，两年来情况大为改善。hmry 则指出它的缺点：新变体如果不能被安全忽略，就仍是破坏性变更，只是没有编译错误了，他举 PNG 格式用标志位区分「可忽略」和「不可忽略」块作对比。kibwen 补充，在定义 enum 的 crate 内部匹配时仍有穷尽检查，这个属性只约束外部使用者。ekidd 和 quotemstr 则对 protobuf 那套「什么都可选」的演进方式表示反感，quotemstr 更倾向于 Foo1、Foo2 这样的离散版本。

**为什么要那么小的 crate？** monocasa 觉得 Chromium 要求极小 crate、不许嵌套子目录，像 2010 年前后「函数越短越好」的潮流。多人解释了原因：crate 是 Rust 的编译和并行单位，Bazel/Blaze 不支持 crate 内增量编译，所以 crate 越小构建越快、越容易缓存（hugmynutus、skohan、vvillena）；evmar 说在 Google 的构建系统里 crate 不需要单独的目录和 Cargo.toml，开销远小于 Cargo；pornel 和 skavi 认为 crate 能强制声明依赖、禁止循环依赖，对公开 API 的约束更严格；i2talics 讲了一个反例：以前公司有个大杂烩「utilities」crate，只想用其中的 IP 工具却连带引入射频工具，任何子模块更新都会让版本变动波及所有下游。

**Rc/Arc、命名与导入。** slaymaker1907 强烈反对「避免 Rc 和 Arc」，认为调用栈之外的引用就该用它们，别为了说服编译器去玩复杂的生命周期，并猜测这条建议源自 C++ 中引用计数代价高的经验；i2talics 认为严格的所有权图能迫使你更审慎地设计类型，smj-edison 补充生命周期只能表达树，而有些问题本来就是图。incrediblers 不同意 getter/setter 命名，他做的 TUI 框架里 setter 远多于 getter（几个项目中 setter 数百上千、getter 只有几十），所以他让 setter 用短名、getter 加 `get_` 前缀；epage 认为两者取决于类型用途，像 `Command` 这样的构建器用无前缀 setter，记录类结构体用无前缀 getter。epage 还反对「类型名不加限定」，认为这会逼出 `IoError` 这种冗余名字，像 `Source` 这样的泛名需要模块前缀才能看懂；B-Con 则说 Google 的风格指南一贯致力于减少命名冗余。tcallred 赞赏指南区分库代码和应用代码，Rust 复杂泛型的坏名声大多来自库，应用代码很少需要那么复杂。oddCopper65 说「用 enum 代替 bool」是他独自写代码也会保留的规则，曾经帮他在证书校验器里避免了一个参数反转的 bug。dathinab 提醒，这是为在 Google 规模下统一套用而设计的规则，不一定是每种情况下的最佳风格，antonchekhov 引用 Rob Pike 的话：「gofmt 的风格谁都不最喜欢，但 gofmt 是所有人的最爱。」afdbcreid 分享了一个小技巧：在扩展 trait 上加 `#[rust_analyzer::prefer_underscore_import]`，rust-analyzer 会默认以 `as _` 方式导入。

**跑题的 C/C++ 存亡论。** petcat 问现在还有人写新的 C/C++ 吗，引发大量回复：neutronicus 说国防领域到处是 C++ 职位，tormeh 说新兴无人机公司则偏爱 Rust；fasterik 列举 Windows、Linux、Chrome、GCC、LLVM、3A 游戏引擎仍在大量写 C/C++，还说用了 LLM 之后他反而更倾向于 C 的简单。skohan 和 tialaramex 就 Rust 在高性能计算和数据导向编程中是否碍手碍脚争论了一番，tialaramex 举例说 C++ 的 `std::sort` 在比较函数不一致时会触发未定义行为，而 Rust 的不稳定排序始终安全，而且更快。m00dy 宣称 Rust 正在「吃掉」C/C++，pjmlp 反讽 rustc 本身还依赖 C++ 写的 LLVM。59nadir 分享自己正把约 15 万行 Odin 代码移植到 C23，理由是编译快、工具链成熟、调试器支持好。
