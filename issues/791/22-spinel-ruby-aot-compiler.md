---
layout: article
title: "Spinel：Matz 亲手打造的 Ruby AOT 原生编译器"
issue: 791
number: 22
category: code
original_url: "https://github.com/matz/spinel"
hn_url: "https://news.ycombinator.com/item?id=47887334"
date: 2026-04-27
---

## 文章摘要

Spinel 是 Ruby 之父 Matz（松本行弘）发布的一款实验性 AOT（Ahead-of-Time）编译器，能把 Ruby 源代码编译为独立的原生可执行文件。它的工程定位非常务实——不是要"再造一个 Ruby"，而是要让 Ruby 在不需要解释器、不需要运行时依赖的前提下跑得更快。项目本身具有一个鲜明特征：**自举（self-hosting）**——编译器后端用 Ruby 写成，自身被 Spinel 编译为原生二进制。

技术管线分三步：

1. **解析**：使用 libprism 解析 Ruby 源代码并将 AST 序列化。
2. **代码生成**：执行整体程序的类型推断（whole-program type inference），输出优化后的 C 代码（这一步是用 Ruby 自举实现的）。
3. **编译**：用标准 C 编译器把 C 代码编译为可执行文件，运行时只依赖 libc 和 libm。

性能方面 Spinel 的成绩相当扎眼。在 28 个基准测试中，**几何平均比 miniruby 快约 11.6 倍**；在计算密集型基准上拉得更开：康威生命游戏 86.7×，Ackermann 函数 74.8×，Mandelbrot 58.1×。即便是更接近实际负载的 JSON 解析（10.1×）、模板引擎（6.2×）、光线追踪（8.0×）也都有显著加速。

语言特性支持范围已经相当广：类与继承、mixins、模式匹配、Block / Lambda、基于 Fiber 的并发、异常处理、正则表达式、任意精度整数、文件 I/O，以及对字符串拼接链做了专门优化。项目附带 74 个通过测试和 55 个基准。值得指出的是，Matz 在这一版本里**主动省略了某些边角特性**（比如完整的字符编码体系），这与 mruby 的取舍思路一脉相承。背景信息：这是 Matz 在 RubyKaigi 2026 现场展示的实验项目，名字 Spinel 来自他的新猫（这只猫又是以《魔卡少女樱》中 Ruby 的搭档命名）。

## HN 评论精华

- **whizzter** 提出资深质疑：如果不是 Matz 做的他会"严重怀疑"——他自己当年做过 EcmaScript 的 AOT 编译器，最终被 JSON.parse 这种"输入类型完全未知"的特性打败而放弃。他追问："eval、send、method_missing、define_method 在真实 Ruby 代码里到底有多常见？无类型 JSON 是怎么处理的？"
- **vidarh** 给出极为详尽的回答：Spinel 的设计很务实——用 Prism（Ruby 解析最难的部分已经搞定）、生成 C；基础 Ruby 语义并不难实现，他自己也在做一个纯 Ruby 自举的 AOT 编译器。"前 80% 你可以糊弄过去，能跑很多 Ruby 代码。第二个 80% 才是真正的硬骨头，Matz 在 Spinel 里和 mruby 一样省掉了编码、各种边缘特性。" `send / method_missing / define_method` 实现起来不难，但 `eval` 是巨大的痛苦——好在 Ruby 中大量 eval 用法可以静态归约成 `instance_eval` 的 block 形式。
- **xp84** 从工程实践角度看：eval 和 send 在普通业务代码里其实很少见（send 常被认为是绕过私有方法访问的"耍流氓"），元编程（define_method 等）也是"95% 情况下应当避免"。这些主要出现在 Rails 这类框架的"魔法"里，应用代码本身用得不多，所以 Spinel 的限制对绝大多数 Ruby 程序员是可接受的。
- **Calavar** 持不同意见：他经常在应用代码里用元编程，特别是 instance_eval，因为 Ruby 的真正魅力就是"像 Lisp 那样能快速搭出领域专用语言"，如果不需要这种能力他宁可用静态语言。
- **dluan** 提供了关键背景：Spinel 是 Matz 在 RubyKaigi 2026 展示的，**借助 Claude 大约一个月就构建出来**，并完成了成功的现场演示。命名来自他的新猫，猫名来自《魔卡少女樱》里 Ruby 的伙伴。**rayiner** 顺势点评："大家都在谈 AI 从零写程序，但更可能的现实是 AI 把 10× 工程师变成 100×，把 Matz 这种本来就是 100× 的人变成 500×。"
- **satvikpendem** 用一句俏皮话总结："AI 是 f(x) = x · |x|——10× 变 100×，1× 还是 1×，-10× 变 -100×。"——既呼应了"AI 放大原有能力"的观点，又点出了一个重要的反向警告：能力本来弱的人，AI 可能让他们的产出变得更糟。
