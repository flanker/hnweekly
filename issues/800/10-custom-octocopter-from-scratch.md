---
layout: article
title: "零硬件基础，从头造一架八旋翼无人机"
issue: 800
number: 10
category: favorites
original_url: "https://karolina.mgdubiel.com/drone/"
hn_url: "https://news.ycombinator.com/item?id=48704289"
date: 2026-07-03
---

## 文章摘要

这是 Karolina Dubiel 记录自己从零打造一架八旋翼无人机（octocopter）的项目日志。她此前完全没有硬件或 CAD 经验，甚至从未飞过无人机，却在两周半内走完了从"想法"到"能飞的无人机"的全流程：在 Fusion 360 中设计，用 G10 玻璃纤维和碳纤维 CNC 铣削出机架，再手工组装电机与螺旋桨。项目的终极目标是训练一个强化学习（RL）控制器，能在仿真中承受单电机、双电机乃至四电机失效并维持飞行，最终以 zero-shot 的方式部署到真实硬件上。

项目分为四个阶段：Phase I 完成定制机架的 CAD 设计、CNC 切割与组装；Phase II 接好电子设备，先作为一架普通的飞控（FC）驱动八旋翼起飞；Phase III 开发并训练能在常规飞行及双电机失效下维持飞行的 RL 策略；Phase IV 完成 sim-to-real 迁移，实现 RL 驱动飞行。她在第 17 天（立项恰好两周半）实现了首飞——此时还是一架没有任何 RL 能力的普通八旋翼。她也解释了八旋翼天然容忍单电机失效的原因：一是推力冗余巨大（8 个电机在悬停时各承约 125 gf 负载，损失一个后总推力从约 11000 gf 降到约 9750 gf，仍足以维持健康的推重比）；二是 Betaflight 的 PID 环以数千赫兹运行，不需要知道"为什么"倾斜，只要陀螺仪报告横滚，就命令低侧的剩余电机加大出力。

日志中最精彩的是训练 RL 策略的调试历程。她诚实地列出了七次尝试的失败与教训：熵值失控、残差动作导致饱和翻覆、课程学习（curriculum）反而暴露隐藏 bug、甚至还有前一晚的"僵尸训练进程"和新进程抢写同一个 checkpoint 文件的乌龙。最终定位到两个系统性根因并修复：其一，高斯策略输出无界均值，而环境将指令硬裁剪到 [0,1]，PPO 却在未裁剪值上计算梯度，导致电机一旦漂过裁剪边界就再无纠正梯度——解决办法是通过 tanh 做残差压缩；其二，"活着"本身没有回报——+0.1 的存活奖励恰好被 -0.1 的高度惩罚抵消，于是把存活奖励从 0.1 提到 1.0，PPO 终于有了留在空中的理由。最终策略是一个仅 4.34 万参数的 MLP，不仅能扛住训练过的单/双电机失效，还能泛化到从未训练过的三电机失效（只要物理上可恢复）。她还调整了架构决策：放弃独立协处理器，直接把策略编译进 Betaflight 固件（基于 STM32H743，480MHz M7），既符合安全模型又大幅降低了回环延迟。

## HN 评论精华

评论区一半是对作者的鼓励与好奇，另一半则演变成一场关于 "octocopter" 这个词是否合乎词源的欢乐语言学辩论。

- **cyclopeanutopia** 作为"波兰同胞发明家"率先送上鼓励，作者 **kar0lina** 也亲自现身留下了自己的 X 和 LinkedIn 联系方式，欢迎交流。

- 语言学辩论由 **adrian_b** 挑起。他"吹毛求疵"地指出 "octocopter" 构词不合理："helicopter" 由 "helico-"（螺旋）和 "-pter"（翼）组成，而 "octo-" 是八、"-co-" 什么都不是，严格说应叫 "8-propeller drone"。

- 这条评论招来一众反驳。**maciuz** 指出 "-copter" 后缀在无人机社区极为常见（如广为接受的 quadcopter）；**KPGv2** 更是火力全开：既然要抠字眼，那 "nit pick"（挑虱子卵）本义也和"批评"毫无关系。他随后正面论证 "-copter" 是完全合理的后缀（gyrocopter、hexacopter、octocopter 乃至 roflcopter 都有词典条目），"语言的唯一目的是传达思想，只要清晰、简洁即可，忠于古希腊语不该是英语的目标"。

- **cryptopian** 从语言学角度给出了最专业的解释：这是一种常见现象，一个词被"重新切分"（rebracketing）形成新后缀，即便与原始词源不符——类似的还有 -holic（alcoholic → workaholic）、-thon（marathon → danceathon）、-gate（Watergate → partygate），术语叫 "libfix"（解放出来的词缀）。

- **afandian** 则贡献了一个可爱的误读："我本以为这是一篇关于业余半导体制造的文章"——因为他把 octocopter 看成了 optocoupler（光耦合器）。
