---
layout: article
title: "TIL：不用 curl，靠 Bash 的 /dev/tcp 也能发 HTTP 请求"
issue: 798
number: 24
category: code
original_url: "https://mareksuppa.com/til/bash-dev-tcp-http-without-curl/"
hn_url: "https://news.ycombinator.com/item?id=48558018"
date: 2026-06-19
---

## 文章摘要

当容器里没有 curl、wget 这类 HTTP 工具时，Bash 自身其实也能建立 TCP 连接并手动发送 HTTP 请求。其奥秘在于 Bash 内置的 `/dev/tcp` 重定向特性。

尽管 `/dev/tcp` 看起来像一个文件路径，但它并非磁盘上真实存在的设备文件，而是 Bash 的重定向语法：当你给出形如 `/dev/tcp/host/port` 的路径时，Bash 会在内部完成 DNS 解析并尝试打开对应的 TCP 套接字。一个最简单的健康检查只需三步：

```bash
exec 3<>/dev/tcp/service/8642
printf 'GET /health HTTP/1.1\r\nHost: service\r\nConnection: close\r\n\r\n' >&3
cat <&3
```

即打开文件描述符、写入 HTTP 请求、再读取响应。添加认证头也是同样的模式，只需在终止请求的空行之前多加几行 header。

不过这种方式限制明显：没有 HTTP 解析、重定向处理、压缩或 TLS 支持；必须带上 `Connection: close` 否则会一直等待；它是 Bash 专属的，在 dash 等 POSIX shell 中不可用；还需要编译时开启 `--enable-net-redirections` 标志（某些发行版历来禁用了它）。总之，它不适合生产环境，但在无法安装软件包的极简容器中，是一个轻量的调试手段。

## HN 评论精华

**xenadu02**（535 赞）：怀旧地回忆起 90 年代末发现 telnet 可以手动与 HTTP、SMTP、POP3 服务器交互的经历，由此领悟到计算机"没有魔法"——一切都是人造的，只要肯花力气都能理解。

**charles_f**：回忆在 DKIM/SPF 出现之前，曾用 telnet 伪造从 jacques.chirac@elysee.fr 发出的邮件，以"黑客"姿态唬住朋友。

**mrshu**（作者）：承认文中措辞不够严谨，澄清应是"Bash 能打开 TCP 套接字"而非声称 Bash 本身会说 HTTP；并解释当时用的极简 Docker 镜像里确实没有相关工具。

**simonw**：给出了用 `/dev/tcp` 访问 example.com 的可用代码，并指出如今几乎没有现代域名还允许非 HTTPS 访问了。

**basilikum**：批评"bash 会说 HTTP"的说法，强调二者的区别——bash 只是打开套接字，HTTP 请求是人手动构造的；并提醒不要在生产环境里无人值守地运行这类脚本。
