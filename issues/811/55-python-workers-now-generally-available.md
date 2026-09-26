---
layout: article
title: "Python Workers 正式全面可用（GA）"
issue: 811
number: 55
category: startup_news
original_url: "https://blog.cloudflare.com/python-workers-ga/"
hn_url: "https://news.ycombinator.com/item?id=49787142"
date: 2026-09-25
---

## 文章摘要

Cloudflare 官方博客 2026 年 9 月 21 日的公告，作者是 Dominik Picheta、Gyeongjae Choi 和 Hood Chatham（后两位也是 Pyodide 核心开发者）。两年前 Cloudflare 推出 Python Workers，让 Python 应用能跑在 Workers 运行时里，目标是让用 Python 写 Worker 和用 TypeScript 一样简单，并让 Python 的包和框架生态"开箱即用"。现在它正式进入 GA。

**GA 意味着什么。** Python 成为 Cloudflare 开发者平台上一等公民、得到完整支持的语言。你可以带上熟悉的 Python 代码、库和设计模式，直接对接 Workers AI、R2、D1、Hyperdrive、Durable Objects、Queues、Workflows 等全套平台能力；可以在 Python Workers 里运行 FastAPI、Django、Flask；甚至可以通过 Dynamic Workers 在一个 Worker 里再创建一个 Python Worker。开篇示例是一个 FastAPI 应用，从请求里取出 env，直接调用 Workers AI 上的 gpt-oss-120b 模型，最后用 `Default = asgi.entrypoint(app)` 导出入口。

**技术路线。** Workers 从 2018 年起就支持 WebAssembly，天然适合运行编译成 Wasm 的 Python 解释器。借助 Pyodide，他们很快就支持了大量 Python 应用。他们的目标是打造"第一个可无限扩展的 Python 应用平台"，同时让开发体验和性能不输于在其他地方开发 Python。

**绑定原生化。** 过去在 Python Workers 里使用 Cloudflare 绑定，必须在 RPC 边界手动把 Python 对象转成 JS 对象。比如往队列发一个字典，要写 `self.env.QUEUE.send(to_js({"key": "value"}, dict_converter=js.Object.fromEntries))`。这迫使 Python 开发者时刻惦记着 JavaScript 环境，也是人类和 AI 代理都经常出错的地方。现在整个类型转换被封装进了运行时和 Python SDK，一行 JS 都不用写：

```python
self.env.QUEUE.send({"key": "value"})
```

**Web 框架：FastAPI、Django、Flask。** 在普通环境里，FastAPI 应用要靠 uvicorn 之类的服务器来跑；在 Python Workers 里，只需用官方提供的 `workers.asgi` 包包一层：

```python
from workers import asgi

class Default(WorkerEntrypoint):
    async def fetch(self, request):
        return await asgi.fetch(app, request, self.env)

# 或者等价地
Default = asgi.entrypoint(app)
```

Django 这类同步框架则用 `workers.wsgi`。原理是：WSGI/ASGI 是 Python Web 应用与服务器之间的标准契约，传统部署里由 Uvicorn、Gunicorn 负责并发连接和扩展。在 Workers 上，平台本身就是 Web 服务器，全球网络已经负责负载均衡和无限扩展，没必要再在 Worker 里跑一个服务器。这两个连接器只是一座轻量的桥：把进来的原生 JS 请求翻译成 Python 应用期待的 WSGI/ASGI 结构，再把响应送回去，开销极小。任何基于 WSGI 或 ASGI 的框架都能这样用。

**通过 Hyperdrive 连接 PostgreSQL 和 MySQL。** 以前 Python Workers 不支持 TCP socket，数据库驱动根本用不了。原因在于 WebAssembly 的运作方式：aiomysql、asyncpg 这类驱动依赖标准库 socket 模块，后者在普通环境里会发起 POSIX 系统调用，而在 Wasm 沙箱里这些网络系统调用通常只是永远失败的桩函数。他们的解决办法是用 Workers 的 connect API 实现 socket 系统调用：驱动打开 TCP 连接、读取字节时，调用会被翻译成 Workers 运行时对应的 JS 调用。因为翻译发生在系统调用层，驱动完全不需要知道底层实现。在 Wrangler 配置里绑定 Hyperdrive 之后，就能直接用 aiomysql 按熟悉的方式连库、执行 SQL。

**扩展 WebAssembly 包生态。** 带 C/C++/Rust 原生扩展的包，必须交叉编译成 Wasm 才能在 Python Workers 里运行。过去没有任何标准方式做这件事，团队只能自己手动编译、托管定制包，可用的包因此非常有限。他们不想只做一些"只能在 Python Workers 里用"的包，而是希望整个 Python-on-WebAssembly 社区都受益，于是提出了 PEP 783，为在浏览器类运行时中运行 Python 标准化了一个名为 PyEmscripten 的平台。经过一年多的讨论和打磨，这个提案已被接受，包维护者现在可以为 PyEmscripten 平台构建和发布 wheel，所有实现了该平台的环境都能使用。他们还把 Pyodide 构建工具链稳定下来、改造成所有维护者都能用的形态，并给 cibuildwheel 加上了 PyEmscripten 支持。遇到还不支持的包，可以在 Discord 或 GitHub 上反馈，团队会去构建。相关细节见他们在 EuroPython 2026 的演讲《Python Everywhere: The State of Python on WebAssembly》。

**AI 代理与流水线。** openai、langchain 等库依赖 requests、httpx 这类 HTTP 客户端，而由于缺少底层 socket 支持，这些客户端以前在 Python Workers 里工作不正常。他们向上游做了贡献，让这些 HTTP 客户端在 WebAssembly 环境中可以直接通过 JS 的 fetch API 发请求；再加上新的底层 socket 支持，整个网络栈就打通了。现在 openai、langchain、mcp 等库都能原生运行，还可以结合 Workers AI 在 Cloudflare 网络的 GPU 上做无服务器推理，或通过 AI Gateway 代理请求。文中给了一个用 langchain-cloudflare 调用 Llama 3.3 70B 的示例。

**示例与文档。** python-workers-examples 仓库收集了一批可用于生产的模式：用 Queue + Workflows + Workers AI + R2 搭一个纯 Python 的异步图生图应用；用 Durable Object 保持长连接，消费 Bluesky Jetstream 的 WebSocket 实时事件流；用官方 Python MCP 包部署 MCP 服务器；用 Vectorize 做 RAG。Cloudflare 各产品的开发者文档里，几乎所有 TypeScript 示例旁边都补上了 Python 版本，可以在 JavaScript、TypeScript、Python 之间切换。下一步计划是提升 Python Workers 的性能和内存效率，并支持更多包。

## HN 评论精华

269 分，43 条评论，规模不大，但几位作者（dom96 即 Dominik Picheta，hoodchatham 即 Hood Chatham）和竞品 Wasmer 的创始人都亲自下场，技术讨论质量很高。

**开场先是标题歧义。** dsign 说他第一眼把标题理解成"我们用 AI 替换掉了所有 Python 程序员，他们现在'全面可用'了"。Joeboy、FartyMcFarter 也表示第一反应以为是讲软件开发就业市场惨状的新闻，或者某种"云端 Python 工程师出租服务"。

**冷启动是最核心的质疑。** stefan_lec 问冷启动表现如何，因为 WebAssembly 一直被认为启动更慢。dom96 回复说上一篇博客里有冷启动数据，这也是团队接下来的重点。aeyes 指出一个矛盾：那篇博客显示 Python Workers 比 AWS Lambda 快，但 dom96 链接的基准页面上 Lambda 不仅快得多，也稳定得多；dom96 解释说链接指错了，应该看带依赖包的那一组。amenghra 认为很多场景下用户感知不到冷启动，比如静态内容走 CDN、先展示本地缓存再更新，只要在 100 毫秒以内就会显得很即时。csomar 分享的经验是，他的 Rust Wasm Workers 最终冷启动和运行时都比 JS 版更短，因为编译好的 Wasm 比把一堆依赖加载进内存的 JS 优化得多。

**Wasmer 创始人 syrusakbary 的"竞品点评"。** 他先是称赞了 Cloudflare 团队，然后翻出自己 2024 年在首发帖里提的意见：包支持确实进步显著，PyEmscripten 已经通过 PEP 783 标准化；但当时提出的架构问题依然存在。第一，只能用 workerd 内嵌的那一个 Python/Pyodide 版本；第二，架构上绑定在 JS/V8 世界里，他认为以现有架构很难把启动时间压到 100 毫秒以下。他引用了 Wasmer 今年早些时候的基准：最小的 Python 应用在 Wasmer Edge 上约 60 毫秒启动，在 Cloudflare Workers 上约 900 毫秒，并请 Cloudflare 公布最新的 p50/p95 冷启动数据，最好分"带 FastAPI"和"无依赖"两种情况。dom96 回应说版本问题并不完全准确：可以用兼容性标志选择版本，比如 `python_workers_314` 对应 Python 3.14，也有 3.13 和 3.12 的标志，只是旧版本对应旧的 Pyodide，缺少 JSPI 等特性。关于 V8 绑定，他承认这确实是挑战，但内存快照已经显著改善了冷启动，分片（sharding）机制也大幅降低了冷启动发生的频率。syrusakbary 追问说，兼容性标志是全局版本，同时影响 Python 和 workerd 本身，他预计这种设计会带来架构问题；上一篇博客报告的启动时间约 1.027 秒，而 Wasmer 是 60 毫秒，差了 16 倍，所以才想知道有没有重新测过。

**urllib3 维护者谈"谁来为上游买单"。** 博文说"我们向上游贡献了代码，让 HTTP 客户端能通过 fetch 发请求"，urllib3 维护者 illia-v 补充了背景。几年前 urllib3 确实接收并合并了大量添加 Pyodide/Emscripten 支持的贡献，后来又加了 JSPI 支持，Requests 才能这样工作。但据他所知，这笔资助给的是实现代码的外部贡献者，而不是 urllib3 维护者；维护者负责审查和合并，项目此后还要负责维护这个后端。这很关键，因为 Emscripten 后端在 urllib3 里仍被视为实验性功能，明确不在安全策略范围之内。CVE-2025-50182 就是一个例子：请求经由 fetch 发出时，urllib3 的重定向控制没有按预期生效，由于浏览器 fetch 的网络语义与 urllib3 常规后端差别很大，类似的差异可能还有很多。"我很高兴这些工作贡献到了上游，对 Pyodide 和 Cloudflare 有用。但资助一次对上游的贡献，和资助之后必须长期支持它的上游维护者，两者有实质区别。"meagher 建议维护者可以考虑在对方签支持合同之前不合并；找到公司内部的支持者、按流程申请，对关键依赖来说，公司通常愿意拿出预算。

**事件循环语义之争，作者亲自纠正。** syrusakbary 认为 Pyodide 和 Cloudflare 把网络请求打补丁改成走 JS fetch、把 Python 的 asyncio 事件循环换成 JS 事件循环，会带来兼容问题：JS 事件循环是"抢占式"的（async 函数不管你 await 不 await 都会执行），而 Python 是惰性的（只有 await 才执行），他主张保留原本的网络语义。hoodchatham 回复说他想说的词应该是"急切"（eager），"抢占"通常指信号处理器那种不等显式让出点就打断正常执行的机制；而且用了 WebLoop 之后，Python 协程依然是惰性的，Python 事件循环需要实现的根本原语是 call_later()，能很干净地映射到 setTimeout()。之所以要用 JS 事件循环，是因为 JS 运行时里所有真实的 I/O 事件都发生在那里，另起一个事件循环会阻塞 JS 事件循环上的真实 I/O，所以让 uvloop 跑起来毫无意义。他还解释，浏览器里的 Pyodide 出于基本的浏览器安全原则"不能"使用真实网络栈，否则就能绕过 CORS；而在 Node 和 Cloudflare 上，得益于 Gyeongjae Choi 的工作，Pyodide 已经可以使用直接 socket。syrusakbary 大方承认用词错误并致谢。

**给 Pyodide 捐钱。** simonw 说 Pyodide 是 Python 生态里非常酷的一块，希望 Cloudflare 考虑通过 OpenCollective 资助它。codingglass 说几年前雇用 Hood 本身就是对这个项目很好的投入；simonw 这才发现 Hood Chatham 正是本文作者之一，认为这已经算相当扎实的赞助了。dom96 补充说另一位作者 Gyeongjae 也是 Pyodide 核心开发者，没有他们俩，Python Workers 走不到今天。

**其他声音。** spicypixel 希望有一天 Go 也能这么方便；OutOfHere 说 Go 没法跑在 Wasm 上，ncruces 纠正说 Go 编译器原生就支持 wasm/js（浏览器）和 wasm/wasip1（WASI）两个目标。xnx 感叹 PHP 和 Perl 当年被嘲笑，最后又被重新发明了一遍，"我们放弃了无状态、每个请求跑一次的脚本那种简单"。manquer 说这是绕了一圈回到 2008 年发布、支持 Python 2.5 的 Google App Engine；stevefan1999 回忆起十几年前用 GAE 部署 Django 应用、借免费额度搭 HTTP 代理绕过学校网络。dangoodmanUT 估计 Pyodide 比 V8 运行时重得多，会多占掉几十 MB 的 Worker 内存配额。pastrami_panda 反馈文中的示例链接全都 404。
