---
layout: article
title: "我用 F# 写了一个 Game Boy 模拟器"
issue: 792
number: 8
category: favorites
original_url: "https://nickkossolapov.github.io/fame-boy/building-a-game-boy-emulator-in-fsharp/"
hn_url: "https://news.ycombinator.com/item?id=47965503"
date: 2026-05-01
---

## 文章摘要

作者 Nick Kossolapov 是一位有 8 年经验的软件工程师，他想补上自己对"计算机底层到底怎么工作"这一块的认知空白。在做完 NAND-to-Tetris 课程和一个 CHIP-8 模拟器作为热身后，他选择 Game Boy 作为下一站，并把整个工程用 **F#** 写下来。这篇文章是这个项目（取名 "fame-boy"）的全过程复盘，干货密度极高。

**为什么选 F#**：F# 的判别联合（DU）和模式匹配天然适合给 CPU 指令做"领域建模"——Game Boy 的 LR35902（Sharp SM83）有 ~512 个操作码，但作者通过类型抽象把它压缩成 58 条指令。例如：

```fsharp
type From  = Immediate of uint8 | Direct of Register | Indirect
type To    = Direct of Register | Indirect
type LoadInstr = Load of From * To
```

这种建模方式让"非法状态在编译期就不可表达"——比如 `Load(Direct D, Immediate)`（把寄存器写到立即数里）会直接被类型系统拒绝，不需要运行时检查。

**架构**：模拟器分前后端，对外只暴露三个接口——160×144 像素的 framebuffer（4 级灰度）、32768 Hz 的环形音频缓冲、`stepEmulator()` 单步指令、`getJoypadState()` 手柄回调。内部由 CPU、Memory（含 RAM 和总线）、PPU（图形）、APU（音频）、IoController（硬件寄存器）、定时器、串口和中断系统组成。

**可变性的现实妥协**：CHIP-8 模拟器作者写得完全纯函数式，但 Game Boy 每秒要做上千万次内存访问，纯不可变方案直接被性能淘汰。这是"学习目标"对"现实约束"的让步——文章诚实地讨论了这种取舍。

**标志位优化**是个有教育意义的小例子。最初用列表传 `[Half, false; Zero, a=0uy]` 速度不行，改成内联管道后性能涨了 ~10%：

```fsharp
cpu.Flags <- cpu.Flags |> setH false |> setZ (a = 0uy)
```

`inline` 让闭包不上堆分配。

**调试的"定时器之冬"**：Tetris 版权画面卡住超过 2 分钟。作者在这个 bug 上花了 20+ 小时，最后是 Claude Opus 几分钟内定位出来——定时器应该按 CPU 周期数（每条指令 1–6 个周期不等）递进，而不是按指令数。

**PPU**：Game Boy 真机的 PPU 是按"像素 FIFO"逐像素扫出，模拟硬件 CRT 时序；作者为简化采用了"扫描线批量渲染"，牺牲一些精确像素时序的硬件极限游戏特效，换取实现量。建议从"只渲染背景层"开始调试，逐步上 sprite。

**手柄寄存器**有个反直觉点：CPU 和游戏软件共享一块寄存器，游戏靠两次快速读出（在两个状态位之间切换）来读全部 8 个按钮。作者一开始按"每周期更新"实现，导致 d-pad 失灵；改成"只在 CPU 读寄存器时更新"才正确。

**APU 是最难的**：选 32768 Hz 采样率正好是 128 个 CPU 周期/采样，可以全部走整数运算；驱动方式上作者最终选"音频驱动"（让音频缓冲为主时钟）而不是"帧驱动"，因为人耳对音频爆音比对帧率波动敏感得多；Mac 和 PC 的音频输出栈差异大，需要单独适配。

**Web 移植与性能**：用 Fable 把 F# 转 JavaScript，第一坑是 Fable 没自动截断 uint8——JS 的位运算是 32 位语义，结果出来是 -15565461 这种数。手工 `value &&& 0xFFuy` 修复。优化后 100 KB 的 web 包在 M4 Mac 上能跑到 779–976 FPS（不同 demo）。

**性能优化的关键洞察**：FPS 起初只有 45，最大瓶颈是内存映射用 DU 表示——每次访问都堆分配一个 `MemoryRegion` 对象，每秒上百万次。改成直接数组访问后 FPS 翻倍。其他收益：把 DU 标成 `[<Struct>]` 改栈分配（+15%），分支布局优化让 JIT 更好预测（+85%）；最重要的发现是 Debug 配置下性能只有 Release 的 1/10——基准测试一定要在 Release 跑。一个反直觉的细节：禁用 APU 比禁用 PPU 多省 ~500 FPS，音频处理的 CPU 开销比想象大。

**AOT vs JIT**：作者实测发现 .NET AOT 反而比 JIT 慢约 35%。原因是模拟器中少数指令被反复执行，JIT 能针对热点做激进优化，AOT 只能保守地一刀切。这条结论与 .NET 社区"AOT 永远更快"的常识相反。

**AI 使用边界**：作者刻意区分——代码审查/思路碰撞充分用 AI；让 AI 直接生成代码尽量少用，希望保留"创作所有权"；技术规范阅读适度用；性能建议批量收下后人工验证（事实证明有些"提升"会破坏特定游戏兼容性，比如 STAT 寄存器优化）。AI 还有个独到用法：让它根据规范文档反向生成测试用例，而不让它看实现代码，避免测试和实现在同一脑回路里。

**收获**：作者承认这次项目可能没让他成为"更好的工程师"，但他对自己每天接触的工具——CPU 周期、内存映射、中断、JIT、缓存——有了具体可触摸的理解。Game Boy 这个目标的"复杂度/学习收益比"明显高于 Game Boy Advance（复杂度 3 倍但额外收获只有 20%）。

## HN 评论精华

- **debugnik**：建议给 `Instructions.fs` 里的判别联合加 `[<Struct>]` 属性，把 DU 实例改为栈分配——对模拟器这种每秒分配上百万次小对象的场景是"低成本高收益"的典型优化。
- **关于 Fable 的 uint8 截断**：评论区普遍认为 Fable 在数值类型上的非标准行为是"令人失望"的设计选择——把 F# 的强类型语义透明地映射到 JS Number，结果丢掉了 wrap-around 行为，强迫开发者手动加 `&&& 0xFFuy` 才能保证逐位语义。
- **tombert（函数式语言性能取舍经验）**：F# 的好做法是"接口纯函数式、内部命令式循环+可变缓冲"，外部对调用者保持代数推理友好，内部该可变就可变。这条评论被很多人回应，认为是 F# 在工程项目中能站住脚的关键原因。
- **作者本人对 AOT 的回复**：在线讨论里他重申 AOT 让他的模拟器慢了 35%，原因正是 JIT 能 specialize 热路径——这个例子值得性能社区记一笔，因为它是少见的 "JIT 在长期运行 workloads 上反而更快" 的实测。
- **cermicelli（学习态度的赞许）**：现在很多项目是"让 LLM 几小时撸出来"，作者花数月真刀真枪地学硬件、写优化、写测试，这种"靠人脑+热爱推进的项目"才让 HN 还像 HN。
- **hurril（F# vs C#）**：C# 用户应该尽早接受 F#——既能享受函数式建模的清晰，也能直接复用整个 .NET 生态，比"在 C# 里继续硬塞函数式特性"舒服得多。
- **WoodenChair（《Computer Science from Scratch》作者）**：推荐的模拟器学习曲线是 CHIP-8 → NES/GB → GBA。NAND-to-Tetris 不是必修，但里面的"自下而上拼系统"思维方式是关键。
- **学习资源推荐**：评论区点名几个值得对比的高精度参考实现——MetalNES（晶体管级 NES）、MetroBoy（逐周期 Game Boy）、Higan（逐周期 SNES）；想从模拟器学硬件的人，这三个仓库是公认的"教材"。
