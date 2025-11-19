---
description: On the Linux system use Tayga enable NAT64
---

# NAT64 form Tayga

NAT64/DNS64 早就不是什么稀罕的技术，准确说它是为了帮助only ipv6用户过渡实现可以访问ipv4网络一项协议。被[RFC6146](https://www.rfc-editor.org/rfc/pdfrfc/rfc6146.txt.pdf)所定义。\
\--------\
我们使用Linux Debian做实验，希望这小小的tayga配置可以帮助您理清思路。\
我希望并呼吁 各位ISP 尽早为您的only ipv6客户至少support NAT64/DNS64这项服务。\


您截止到这篇文章2025年为止，目前\[network type]Carrier(CGN)/Eyeball尽管绝大多数声成支持ipv6网络。\
但是几乎包括GitHub,Discord,speedtest.net,这样的网站尽管通过CDN分发等技术，only ipv6网络用户可以下载来自贵网的文件，但是内在api等多个服务仍然还是指向了纯ipv4访问，或者说有太多的服务的ipv6都不是足以脱离ipv4正常访问的。\
\
\-------------\
我知道很多Only ipv6的用户借助cloudflare提供的warp来解锁ipv4网络支持。但是不是所有的ISP to Cloudflare的网络容量都是足够的。并且通过Cloudflare这样CDN确实有效缓解了您的IP痕迹信息,但这不是给我找一个理由不来介绍这样的工具。\
我相信作为Carrier网络的您来说，一台only ipv6的机器，在访问各种ipv4内容为难的情况下，NAT64/DNS64这项服务是您应当部署的。\
\
\---------

言归正传，在Linux上面实现NAT64这项服务其实就是地址转换这项功能并不是很复杂的一件事情。市面上有除tayga外Jool,Ecdysis等工具軟體。\
但是我們這篇文章僅介紹tagay的NAT64搭建\
\
在Debian中 即可完成安裝.

```
apt install tayga -y

```











参考资料：\
1.[JPIRR ](https://www.nic.ad.jp/ja/newsletter/No64/NL64_0800.pdf)\
2.[RFC6146](https://www.rfc-editor.org/rfc/pdfrfc/rfc6146.txt.pdf)\
3.Tayga  [repository](https://github.com/new)

