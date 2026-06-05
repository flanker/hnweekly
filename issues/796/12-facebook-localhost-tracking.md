---
layout: article
title: "Ask HN：Facebook 的\"localhost\"追踪后来怎么样了？"
issue: 796
number: 12
category: ask_hn
original_url: "https://news.ycombinator.com/item?id=48397731"
hn_url: "https://news.ycombinator.com/item?id=48397731"
date: 2026-06-05
---

## 文章摘要

这是一个 Ask HN 帖子，回顾并追问此前曝光的 Facebook（Meta）"localhost 追踪"事件的后续进展。该事件最初由 localmess.github.io 等研究者揭露：Meta 的网页追踪脚本（Facebook Pixel）会通过用户设备上的 localhost（本地回环地址，本是给开发者测试应用用的）与已安装的 Facebook/Instagram 安卓 App 通信，从而将用户在嵌入了 Meta 像素的移动网页上的匿名浏览行为，与他们的 Facebook/Instagram 真实身份关联起来——绕过了浏览器的隐私隔离。Yandex 也被发现采用了类似手法。

帖子梳理了三条主线进展：其一，**Meta 已停止该行为**——据 localmess 的更新，截至 6 月 3 日，Facebook Pixel 脚本已不再向 localhost 发送任何数据包或请求，相关发送 _fbp cookie 的代码"几乎完全"被移除，Yandex 也停止了这一做法。其二，**浏览器层面的防御正在跟进**——Chrome 和 Firefox 正在部署"本地网络访问（Local Network Access, LNA）"机制，当网页试图访问本地网络时会弹窗征求用户许可。其三，**法律诉讼仍在推进**——一名加州居民 Devin Rose 发起的集体诉讼已被法官裁定 Meta 必须应诉，案件已与其他隐私诉讼合并、Google 也被列为被告，被告的驳回动议被驳回，并于近日提交了第三次修订诉状。

## HN 评论精华

- **现状确认（KomoD）**：引用 localmess 的更新——"截至 6 月 3 日 7:45 CEST，Meta/Facebook Pixel 脚本已不再向 localhost 发送任何数据包或请求……Yandex 也已停止。"hulitu 则意味深长地点出措辞里的那个"几乎（almost）"。

- **浏览器对策（mozvalentin）**：Chrome 和 Firefox 已部署/正在部署 local-network-access，当 App 尝试这类访问时会提示用户。Tade0 指出 Chrome 似乎把所有基于 IP 地址的 URL 都当作"本地"处理，不分地址类别。

- **该不该信任访问裸 IP 的网站（kibwen）**：他对任何试图连接裸 IP 地址的网站都本能地警惕——除了 localhost，默认假设这种网站要么在绕过 hosts 文件 /pi-hole/DoH 等 DNS 配置，要么是恶意软件在联系命令控制服务器，要么是在绕过广告拦截。

- **弹窗措辞之争**：多位网友认为浏览器的提示文案太技术化、用户根本看不懂。SoftTalker、dpoloncsak 主张改成更通俗的措辞，比如"这个网站想监视你网络上的其他设备"。但 lukan 提醒不要用"spying（监视）"这种词，因为存在合法用例（复杂网页应用间共享数据）。Aachen 和 SchemaLoad 进一步指出大量场景是正当的——Spotify 找局域网内可播放设备、VLC 找 Chromecast、DJ 软件发现打碟机并串流曲库——问题在于这个提示太新，软件还没来得及向用户解释它为何触发，用户手里没有任何判断信息。lelandfe 抱怨 Chrome 没有"别再问我"按钮。

- **跨平台层面的拦截（crtasm）**：他发现 macOS 在系统设置里有逐应用的开关，曾默默阻止 Firefox 连接局域网设备，让他困惑了好一阵；不过访问路由器 Web 界面没被拦截。apitman 追问 Safari 是否也会跟进 LNA。

- **法律进展（applfanboysbgon）**：引用报道——2026 年 5 月 12 日，旧金山联邦法官 Rita Lin 裁定 Meta 必须面对集体诉讼，指控其在 2024 年 9 月至 2025 年 6 月间，利用安卓的 localhost 特性把用户的移动网页浏览与其 Facebook/Instagram 身份关联起来。1vuio0pswjnm7 进一步补充了案件合并、Google 被追加为被告、第三次修订诉状提交等细节，并附上了相关诉状 PDF 链接。

- **对工程师伦理的反思（woodrowbarlow）**：他很希望有一个软件工程师工会——倒不是为了争取更好的待遇，而是为了能理直气壮地说"我不能实现这个不道德的功能，这违反工会规定，否则我会丢掉会员资格"。

- **延伸创作**：apitman 分享他一直在探索让网页应用访问局域网服务（如 WebDAV 看本地视频）的方案，提到可用 mDNS 注册 `service.local` 实现一定程度的发现，并称 Chrome 的 LNA 实际上很有帮助。0john 则表示受此事件启发，最近做了一个安卓项目 Pal Pipe。
