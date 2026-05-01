---
layout: article
title: "ChatGPT 是如何投放广告的：完整归因链路拆解"
issue: 792
number: 4
category: favorites
original_url: "https://www.buchodi.com/how-chatgpt-serves-ads-heres-the-full-attribution-loop/"
hn_url: "https://news.ycombinator.com/item?id=47942437"
date: 2026-05-01
---

## 文章摘要

威胁情报博客 Buchodi 在 2026 年 4 月 28 日发布了对 ChatGPT 广告系统的逆向工程分析。作者通过一个"用户授权同意的移动流量研究群体"捕获了真实流量，全部结论都基于对实际数据包的观察。文章首次公开拆解了 OpenAI 广告体系的**完整 attribution loop（归因闭环）**，把从"对话内插入广告"到"商户网站转化追踪"的每一步都还原了出来。

整套系统由两端组成：

**ChatGPT 客户端侧**——当用户向 ChatGPT 发送消息后，后端会在 SSE（Server-Sent Events）响应流中注入一种名为 `single_advertiser_ad_unit` 的结构化对象。它包含广告主品牌、卡片轮播、商品信息、跳转链接等。每条广告会附带 4 个 **Fernet 加密令牌**（Fernet 是 AES-128-CBC + HMAC-SHA256 的组合），用于追踪和防伪。

**商户网站侧**——商户嵌入了一份名为 **OAIQ SDK**（OpenAI Quantification）当前版本 0.1.3 的脚本。它会从着陆页 URL 中读取参数，把令牌写入第一方 Cookie，然后定时把转化、加购、下单等事件回传到 OpenAI 自己的两个域名：`bzrcdn.openai.com`（CDN）和 `bzr.openai.com`（事件接收）。

**4 类令牌的分工**：

1. `ads_spam_integrity_payload`：仅出现在 SSE 流里，用于服务器端反作弊和真实性验证。
2. `oppref`：跟在点击 URL 上，被 OAIQ SDK 写入 `__oppref` 第一方 Cookie，有效期 30 天，是核心的"前向归因"令牌。
3. `olref`：与 oppref 成对出现，但目前未被观察到的 SDK 写 Cookie，推测用于离站日志记录。
4. `ad_data_token`：base64 编码的 JSON，里头嵌着另一个 Fernet 令牌，随 SSE 载荷一起下发。

**真实案例**：作者抓到一条用户讨论"北京旅游"时被插入的 Grubhub 广告，跳转 URL 形如 `https://www.grubhub.com/?utm_source=chatgptpilot&...&oppref=gAAAA<...>&olref=gAAAA<...>`。点击后浏览器加载 grubhub 主站，OAIQ SDK 解析 oppref 写入 cookie，此后用户在站内的所有事件（浏览商品、下单等）都会被 POST 到 `https://bzr.openai.com/v1/sdk/events`，自动捎上这个令牌，构成完整闭环。已观察到的广告主包括 Grubhub、GetYourGuide、Gametime、Aritzia、Canva 等，广告还会按对话主题动态投放（聊 NBA 推体育票务、聊旅行推目的地服务）。

更精彩的细节：Fernet 令牌的前 9 字节是公开可读的时间戳。作者通过对比某条 Home Depot 广告令牌的"铸造时间 11:30:08"和浏览器实际加载页面的"11:31:43"，**测算出从广告生成到用户点击的延迟是 95 秒**。这种通过协议本身可观测时序的特点，给独立研究者提供了少有的透明度。

## HN 评论精华

1. **programjames**：直击商业层面的暗讽——Sam Altman 一年前还说广告是"最后的手段"，现在却悄悄铺开。问"是不是 OpenAI 现金压力大了？"。

2. **danparsonson**：高管的口头声明本质上是用来"塑造他人认知"的工具，不能当承诺看。当人们用 "Altman 说……" 当论据时，价值是相对有限的。

3. **kqp**：进一步把高管发言比作"吹哨子叫狗"——目的是产生想要的行为，而非陈述真理；衡量它的标准是有效性，不是真伪。

4. **staticshock**：现实主义视角——这就是从理想主义滑向现实的必经之路。广告是迄今为止能撑起免费消费产品最有效的商业模式，OpenAI 不可能例外。

5. **Aurornis**：替 Altman 做了部分辩护：广告恰恰让 OpenAI 能维持低成本免费层，让全世界都能用，这跟当年的承诺并不完全冲突。

6. **chromacity**：从微观经济学反推——如果广告对 OpenAI 收入贡献小到可以忽略，他们何必冒这么大舆论风险来上线？说明商业模型里它已经举足轻重了。

7. **utopiah**：拿 Google 的历史做类比——拉里和谢尔盖在 1998 年的论文附录里也写过"广告资助的搜索引擎天然偏向广告主、对用户有害"，结果 Google 后来变成了广告巨兽。OpenAI 重走 Google 老路，不是巧合是宿命。

8. 评论区还普遍关注隐私维度：oppref 这种 30 天第一方 Cookie 让 ChatGPT 实际上获得了跨站行为追踪能力，这是浏览器厂商正在努力消除的"老式追踪 cookie"——但这次是 LLM 端反向催生的新形态。
