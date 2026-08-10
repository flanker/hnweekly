---
layout: article
title: "《Rust on ESP》：乐鑫官方的嵌入式 Rust 开发手册"
issue: 803
number: 43
category: books
original_url: "https://docs.espressif.com/projects/rust/book/"
hn_url: "https://news.ycombinator.com/item?id=49047351"
date: 2026-08-01
---

## 文章摘要

《The Rust on ESP Book》是乐鑫（Espressif）官方托管在 docs.espressif.com 上的一本在线书，用 mdBook 构建，目标是把「用 Rust 给 ESP32 系列芯片写固件」这件事从零讲到能上手。它明确写给「对嵌入式感兴趣的 Rust 开发者」——不要求你有嵌入式背景，低层编程的概念会在用到时随讲随补；反过来，它默认你已经会 Rust，想补语言本身的话它把你推去《The Rust Programming Language》和 Rust 嵌入式工作组的《The Embedded Rust Book》。

内容结构上，这本书分五大块。**Introduction** 讲硬件与生态：乐鑫的产品线跨两种指令集架构——ESP32 与 ESP32-S 系列是 Xtensa，ESP32-C 与 ESP32-H 系列是 RISC-V。这个区分是全书最重要的一条分水岭，因为 Rust 官方并不支持 Xtensa（LLVM 至今没有 Xtensa 后端），所以乐鑫自己维护了 LLVM 和 rustc 的 fork，并用 `espup` 这个工具来安装和维护这套分叉工具链；RISC-V 那边则可以直接用官方 rustup 装 `riscv32imac-unknown-none-elf` 之类的 Tier 2 target，无需任何 fork。书里还坦白了上游化进度：LLVM fork 已有实质进展，rustc 这边的补丁则卡在等 LLVM 合并。另外明确说明 ESP8266 不受支持，但 ESP32-C2/C3 支持，且 C3 与 ESP8266 引脚兼容，可直接替换。同章还以 ESP32-C6-DevKitC-1 为例，逐个讲清一块开发板上的 Boot/Reset 按键、两个 USB-C 口（UART 桥 vs 原生 USB/JTAG）、WS2812B 型 RGB LED 这些新手最容易踩坑的细节。

**生态一览**是很实用的一章：它用一张表把 `esp-hal`、`esp-radio`（Wi-Fi/BLE/802.15.4/ESP-NOW）、`esp-alloc`、`esp-storage`、`esp-backtrace`、`esp-bootloader-esp-idf`、`esp-rtos` 等十几个 crate 的用途和**稳定性等级**一次列全。值得注意的是，目前只有 `esp-hal` 标注为 Stable（且各外设驱动的稳定性还需逐个查 Peripheral support 表），其余几乎全是 Unstable——书的前言也直说：unstable 部分不受 SemVer 保护，一次 `cargo update` 就可能把项目搞挂，整个嵌入式 Rust 生态都还在快速演进期，请紧盯依赖版本，官方为主要 crate 提供版本迁移指南。

**Getting Started** 是工具链与工程生成：装 `espup`（Xtensa 用）、`esp-generate`（交互式 TUI 项目生成器，会根据你勾选的无线/异步/日志等选项自动配好 Cargo.toml 与 feature 组合）、`espflash`（串口烧录）、可选的 `probe-rs`（带 RTT 与片上调试，需要芯片有 USB-Serial/JTAG 外设，C3 rev0.3+/C6/H2/S3 具备）以及 `esp-config` 的 TUI。一条 `cargo run` 就完成编译、烧录、串口监视。

**Application Development** 是全书的技术核心：讲第二级 bootloader（目前只支持 ESP-IDF bootloader，配合分区表工作，未来计划支持 MCUBOOT）、配置系统、日志（推荐 `defmt` + `probe-rs`，也支持传统的 `log` facade 加 `esp-println`）、内存分配（先讲**为什么你可能不该用堆**——碎片化与运行时开销；再讲乐鑫芯片内存非连续映射的现实，可以用宏把 bootloader 用过、之后闲置的那块 RAM 回收成堆；以及 PSRAM 的使用，并警告 Xtensa 芯片上 PSRAM 中的原子操作是坏的、会造成数据竞争，RISC-V 芯片则不受影响）、异步方案（`esp-hal` 驱动默认是阻塞模式，调 `into_async()` 转异步；异步驱动不是 `Send`，因为中断注册在当前核上——想跨核必须先传阻塞版再在目标核上转换；框架层面介绍 Embassy、纯 Rust 的 IoT 操作系统 ArielOS，以及目前只支持 C3/C6 的 RTIC）、测试与 OTA。

关于 **std 与 no_std 两条路线**：需要说明的是，当前这一版书**几乎完全站在 no_std（裸机）这一侧**——它的主线是 `esp-hal` + `esp-generate` + Embassy 的裸机异步开发，全书没有独立的 std 章节，只把「Embedded Rust (no_std) on Espressif」列为延伸资源。这与 esp-rs 生态里另一条路线形成对照：走 std 的方案是用 `esp-idf-hal`/`esp-idf-svc` 去绑定 C 写的 ESP-IDF 框架，从而能用上标准库、线程、文件系统和 IDF 成熟的 Wi-Fi/TCP-IP 协议栈，代价是要拉进整套 C 工具链、二进制更大、也更难做到「纯 Rust」。而 no_std 路线的好处是编译产物小、启动快、全 Rust 可审计、与 Embassy 这类 async 执行器结合紧密，代价则是协议栈、分配器、OTA 这些东西都得自己在 crate 生态里拼装，而这些 crate 大多还标着 Unstable。书里唯一与 ESP-IDF 强绑定的地方是 bootloader 与 OTA——`esp-bootloader-esp-idf` 仍复用 IDF 的镜像格式与分区表。

测试一章给出的建议是「能在宿主机上测就别烧到板子上」，硬件相关的用 HIL（硬件在环）：用 `embedded-test` 框架写单元/集成测试，用 `probe-rs` 烧录并运行，`esp-generate` 勾选 embedded-test 即可自动配好，之后本地 `cargo test` 就行。

## HN 评论精华

这条帖子拿到 176 分，讨论集中在两件事：宿主机测试怎么做，以及 esp-hal 大重写留下的伤口。

**关于「在宿主机上测试」的落地难题。** `iconara` 直接质疑书里的建议：Rust 的测试框架需要 std，为了绕开只能上条件编译，写起来很别扭；就算绕过去了，`esp-hal` 依赖的一些 crate 在宿主机上根本编译不过——那推荐做法到底是什么？`devmor` 说除了能塞进 QEMU 的设备，这是他做嵌入式时每个环节的老大难。

回答里最被认可的是 `bigfishrunning` 提出的 **sans-io 模式**：把所有硬件/网络 I/O 代码隔离出去，程序主体写成一个大的同步状态机，"发一个网络包""等 10ms""改这个 GPIO 引脚"都只是状态机发出的指令，底层用 Embassy、tokio 还是 BSD socket 都无所谓。`tylerc230` 描述了具体做法：esp/idf 代码放一个 crate，业务逻辑放另一个不含任何无法在宿主机编译的依赖的 crate，测试全写在后者里。`tredre3` 补充了一个现实约束：乐鑫不少纯软件组件本身就依赖 `esp-hal`，所以你还是得让它能编译。`waterTanuki` 给出更轻量的写法 `#![cfg_attr(not(test), no_std)]`，即测试时才引入 std，从 std 代码调用 no_std 代码——I/O 测不了，但逻辑能测。`officialchicken` 则完全不用内置测试框架：每个测试单独做一个 `[bin]`（因为 flash 装不下所有测试合成一个二进制），再用几行 bash/python 做 runner 和报告，他说自己有时一天烧 50 次，AVR/ESP/STM 的 MCU 至今没烧坏过。

**关于 esp-hal 重写的怨气，这是本帖情绪最重的一支。** `the__alchemist` 说 ESP32 上的 Rust 曾经很好用，但大约一年前 HAL 经历了一次大改写与整合，对他而言「从能用变成了很坏」，而且改版的主要提交者既不是乐鑫员工、也不是原代码作者。他最后干脆改用 ESP-Hosted（官方固件，把 ESP 当射频协处理器），自己写 Rust 库通过 SPI/UART 与之通信，主控换成 STM32。`_paulc` 完全同意，说 esp-hal 的过渡与不稳定「是一场彻底的噩梦」，他已转向 Nordic 的 nRF52840，配合 embassy-nrf，BLE 支持又好又稳，AliExpress 上还能买到便宜的 nrf-micro 板子。

`DannyBee` 的回复火力最猛：他引述维护者对他的答复——「新 API **将会**要求你重新思考你的代码」「新 API 对 Rust 新手**将会**更难用，他们真的需要去想生命周期、可变别名之类的东西」——然后反问，「技术上更好」难道就是让用户和新人痛苦的正当理由吗？他的结论是「ESP 的非 Rust 方案挺好用，但 ESP-Rust 感觉就是一群不珍惜客户时间的人在搞」，现在他只用 STM32 或 nRF。`ost-ing` 说自己做嵌入式 Rust 五年，用重写后的 OTA API 是场灾难，最后只能用 unsafe 全局变量硬绕过去赶交付；他的核心不满是：公共 API 应当足够灵活以适配各种场景，而这次改动付出的痛苦、时间和金钱，几乎换不来什么好处。`selfmodruntime` 补了一刀：probe-rs 在多数芯片上也从「勉强能用」变好、又退回「勉强能用」。`jpgvm` 说自己也全面转向 STM32，「probe-rs、defmt、embassy 异步 HAL，日子好过多了」。有人问能不能 fork 回旧的好版本，`the__alchemist` 的回答很现实：可以，但工作量不小，而且最可能的结局是只有你一个人在用。

**其他视角。** `jauntywundrkind` 感慨这本书挂在 espressif.com 官方域名下很棒，并借题发挥地想：如果 Rust 当年更成熟，Zephyr 会长成什么样？他认为 Zephyr 在无线支持上依然遥遥领先。`bobmcn` 呼应说，他一直想用 Embassy，被信任的人反复推荐、也反复尝试，「但 Zephyr 就是能用」。`_joel` 提供了一个乐观的反例：他把抽屉里吃灰的 ESP32-S3 翻出来丢给 Claude，自己 Rust 经验极少，一个上午就跑起了显示公司网络监控概览的自定义 Rust 固件——虽然他说没做审计前不敢真接进网络，但作为周日消遣已经很满足。
