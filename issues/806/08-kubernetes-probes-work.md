---
layout: article
title: "Kubernetes 探针到底是怎么工作的"
issue: 806
number: 8
category: favorites
original_url: "https://ngrok.com/blog/probes"
hn_url: "https://news.ycombinator.com/item?id=49363665"
date: 2026-08-21
---

## 文章摘要

Sam Rose 在 ngrok 博客上发的这篇 4,197 词长文，用一连串可交互的浏览器内模拟器把 Kubernetes 三种探针讲透了。所有 demo 跑在他自己写的 webernetes 上——一个把 10 万多行 Kubernetes Go 代码移植成 TypeScript 的部分实现，能在浏览器里跑一个模拟集群；他还拿 k3s 逐一校验过 demo 的行为，过程中甚至挖出一个 Kubernetes 真实 bug。

开篇是没有探针的世界：一个 my-app:latest 容器启动后要花几秒初始化才开始监听 8080 端口，但 Kubernetes 从容器启动那一刻就认为它 Ready，于是这几秒内的请求全部失败。容器反复崩溃还会进入 CrashLoopBackOff，默认首次延迟 10 秒、每次翻倍、上限 5 分钟。

**Startup 探针**：httpGet 打 /startup，periodSeconds: 1、failureThreshold: 5，即给容器约 5 秒完成初始化，连续失败 5 次就杀容器。探针由每个节点上的 kubelet 发出。Kubernetes 还支持 tcpSocket、exec、grpc 三种探针类型。作者顺手澄清一个细节：Kubernetes 其实没有 NotReady 这个 condition，只有 Ready 的 True/False/Unknown，文中用 NotReady 只是为了 demo 里字短。配置陷阱是 failureThreshold 给太小（1 或 2），容器永远来不及启动，直接无限崩溃循环。

要真正避免掉请求，还得配上 ReplicaSet（replicas: 2）+ Service 做负载均衡，让 pod-b 打 service-a.default.svc.cluster.local 而不是直连 Pod IP——Kubernetes 用 Ready condition 决定是否把 Pod 纳入 Service 的负载均衡。另一半靠优雅终止：删除 Pod 时 kubelet 先发 SIGTERM，terminationGracePeriodSeconds（默认 30 秒）后才发 SIGKILL；处于 terminating 的 Pod 会被移出 Service 且不再计入 ReplicaSet 的可用副本，所以替换 Pod 会立刻被创建。启动探针 + 优雅终止两者合一，删 Pod 时可以做到零失败请求。

**Readiness 探针**：在 startup 探针成功之后接管，失败到阈值只是把容器标记 NotReady、从 Service 摘掉，不重启。默认 successThreshold 为 1、failureThreshold 为 3，作者建议别乱改。他还挖出一个文档写得极含糊的行为——「带外探测」（out-of-band probing）：官方文档只说容器 not Ready 时探针「可能在配置的 periodSeconds 之外执行，以便让 Pod 更快就绪」。作者在 webernetes 上摸清了：容器处于 NotReady 时，几乎任何对 Pod 的更新（改 annotation、更新 status 等）都可能触发一次带外探测，而很多你意识不到的东西都在更新 Pod。有趣但不该依赖。

为什么有了 readiness 还需要 startup？作者列了几条：startup 探针会推迟 readiness 和 liveness 的启动；可以给启动阶段单独设 periodSeconds 和 failureThreshold（慢启动探得勤，稳态探得稀，减少 kubelet 和容器负载）；startup 反复失败会杀容器并触发重启策略，而 readiness 失败不会——重启有时恰好能救活卡住的容器。

**Liveness 探针**：机制和 readiness 一样，但达到 failureThreshold 时直接杀容器，然后按 Pod 的 restartPolicy（默认 Always）重启。适用于主线程死锁、关键后台线程挂掉这类容器自己救不回来的情况。就是在做这个 demo 时他发现了 bug：容器被 liveness 杀掉重启后，在 startup 探针还没来得及发出时，一个 liveness 探针就抢先触发并失败，导致容器又被重启一次——liveness 本不该在 startup 成功前触发。他在 k3s、minikube、kind 上都复现了，撰文时最新版本 v1.36.2，问题似乎是 v1.35.0 引入的，issue 已提交且被 SIG Node 接受，标为 priority/important-soon。

liveness 的经典误用是拿它检查数据库健康：数据库抖一下就能让所有容器一起崩溃循环。作者做了个 demo，把数据库拉下线，Pod 逐个进 CrashLoopBackOff（延迟按真实 Kubernetes 的 10 秒起、逐次翻倍）；再加上客户端重试，就演示出了教科书级的「惊群」（thundering herd）导致的级联故障——demo 里的容器每秒只能处理 3 个请求，超了就崩，于是数据库恢复后任何敢起来的容器都会被一束流量激光打死。探针救不了这种局面，只能靠限流慢慢放量或者让客户端加退避。结论：只有当故障是单容器局部的、重启很可能修好时才让 liveness 失败，绝不要让所有容器同时满足的条件去触发它。

最后是探针与 Deployment 的关系。示例用 replicas: 3、RollingUpdate、maxUnavailable: "25%"（floor(3×0.25)=0，即滚动期间 3 个副本必须全部可用）、maxSurge: "25%"（ceil(3×0.25)=1，最多允许 4 个副本）。滚动只能多起 1 个 Pod，且必须等它 Ready 才能杀旧 Pod，所以探针周期直接决定滚动速度：periodSeconds 为 1 时整个滚动约 11 秒，改成 5 就变成 19～20 秒；完全不配探针的话滚动飞快，但因为容器一启动就算 Ready，会掉少量请求。

文末给了设计探针端点的建议清单：startup 探针在启动慢或耗时不确定、初始化可能卡死时使用，探测要勤（降 periodSeconds 就要同步升 failureThreshold），目标是覆盖最坏启动时间加一点余量；readiness 要便宜、保守，只在「把这个 Pod 摘掉真能改善整体健康」时才失败，不要因为共享依赖（数据库、第三方 API）或 CPU/内存高而失败——服务接近满载时摘副本反而可能引发级联故障；liveness 只在很确定容器卡住且重启能修好时失败，不确定就返回成功。

## HN 评论精华

这篇帖子 141 分、24 条评论，讨论几乎完全被一条反对意见带着走：探针到底该不该检查上游依赖。

- **stackskipton**（自称 SRE）贴出全场最热的反对帖，强烈不同意「不要因上游依赖失败而让 readiness/liveness 失败」。他给了一串理由：DNS 变了但本地缓存因为不尊重 TTL 而卡住旧记录（他点名了 Java），重启进程能清掉；TCP 连接卡在奇怪状态，重启一般能好；有人配错了环境变量连不上数据库，此时拒绝推进 rollout 反而避免了故障。他的立场是「如果你还没准备好干活（包括关键上游依赖），就不要对系统撒谎说你准备好了」。
- **arccy** 一句话点出反面：惊群 / 级联故障。你干掉了足够大比例的机队，剩下的负载会把剩余节点一个个压垮，永远凑不出足够的健康节点。**javier2** 补充：这样你会从一个服务挂变成 20 个服务挂。**cmckn** 补充另一条：指数退避会把恢复时间拖到 kubelet 的 maxContainerRestartPeriod（默认 5 分钟）。
- **erulabs** 打趣「SRE 团队今年第 540 次为正确性与可用性辩论——当然你们都对」。**atmosx** 给出务实结论：无论选哪种，关键是全公司服务保持一致（不要 A 服务一套 B 服务另一套），并且让工程团队知道这些东西是怎么配的、可能怎么出错。**solatic** 感慨很多工程管理者根本不懂 CAP 定理，还会在面试里因为候选人选了他们不认同的那一边就挂人。
- **jaggederest** 认为惊群应该用断路器解决，而不是靠健康检查撒谎：健康检查失败→重启→失败→重启→断路器跳闸、告警、等人工介入或 X 分钟后再试。**deathanatos** 指出这个断路器默认就存在，就叫 CrashLoopBackOff，文章里也讲了。但 **dilyevsky** 反驳：退避只作用于单个 Pod/容器，不跨 Pod 生效；规模一大就很容易陷入不做（通常是手工的）全量流量排空就恢复不了的局面。
- **solatic** 后来又长篇支持 stackskipton：「不同于那些只在网上读过惊群问题的兄弟评论，作为花了不少时间做 SRE 的人，我同意他这该是默认做法。」小集群、只有几个服务时哪来的惊群？而好处是实打实的。但他也承认，几十个服务、调用链好几层深、每个服务归不同团队所有、整体可用性由 SRE 团队而非各开发团队负责的大集群，是完全另一回事。
- **Flamkuchlo** 给了折中方案：健康端点不要在被调用时才去探数据库，而是让一个后台线程持续检查连通性并把结果写进内存，端点只读最近一次的值。这样还能对不同上游问题差异化响应——数据库返回「permission denied」说明认证数据坏了，那就标记 not ready；后端理解数据库维护模式时应该返回「Maintenance」，Pod 保持 ready。
- **hahn-kev** 认为这取决于你对部署对象有多少控制权：有一派观点认为，对付你修不了的内存泄漏，定期重启就是正确解；但那也只是给真问题贴创可贴，能修就该修。「说存在一刀切的答案有点傻。」**connicpu** 的方案最简洁：「更好的办法是别有那么多关键上游服务。」
- **sidcool** 说文章「没讲什么新东西，但比 Kubernetes 官方文档讲得好太多了」。**srichard16** 说 k8s 文档有时让他想起 Google 的文档，**leetrout** 补刀：「我说过很多次了，光靠把 Google 文档写好就能开一家公司。」**dev_cprice** 替作者说了句公道话：Sam 写教育内容确实有一手，而且他写这篇时真找到了一个 Kubernetes bug，所以技术上至少有一件新东西。
- **stroebs** 想知道怎么给内部文档做这种动画，**claytonjy** 答：作者为了这些动画用 TypeScript 重新实现了一大堆 k8s 逻辑，项目叫 webernetes。**smartbit** 顺带链出他之前那篇《我把 Kubernetes 移植到了浏览器里》。
