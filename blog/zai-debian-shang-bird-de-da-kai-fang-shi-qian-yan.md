---
description: 萌新的网络 正确打开方式
---

# 🌏 在Debian上bird的打開方式前言

## 引言

我不知道你是如何阅读到这篇文章，在这里我不会和你科普任何知识！但是我希望这篇经验使然的文章能够帮助您 认识 及减少 踩些坑。



首先，或许我曾写过 [BGP教程（1](bgp-jiao-cheng.md)）这样的文字，但是 他并不适用于 萌新玩家 ，索性 我更应该 使其简化，至少给出一个清晰的逻辑。

同时上期博文所提到的花销也是有低成本选择



#### 1，在了解 BGP 网关协议后，而且对此 抱有一定兴趣的同时，你将经历漫长的配置网络的学习，同时 这会为你 **认知网络** 打下良好基础。

所以，这篇文章 不适合有一定基础同行阅读，欢迎给我[指错](mailto:admin@jsmsr.com)。

#### 2.既然是一定要弄懂BGP 的同时，你作为一名Linux 初学者（很怪 哪有Linux 初学者 一天天捣鼓这东西的 噗）我认为 你应当 了解一些事情 你应该熟练 掌握 在Linux 上文本编辑 ，或者您学习的是硬路由，[新华三](https://www.h3c.com/cn/)，[思科](https://www.cisco.com/c/zh\_cn/index.html)，[Juniper](https://www.juniper.net/)，[Mikrotik](https://mikrotik.com/) 等，**您可以直接关闭此窗口 对您没帮助！**

### 文本编辑

#### &#x20;我推荐学习 [vim](https://www.vim.org/)啦。如果您不是一定要在vim上编辑文本。简单的 命令

#### 编辑 `I` 退出`：q` 保存退出`：wq` 清屏 `ggdG` 删除单行 `dd` 退出编辑 `esc`

即可帮助到您，假设您使用的nano 等其他文本编辑器 这样也是OK的

### 软件包

#### 熟练掌握 `apt-get install`  `apt-get update`&#x20;

以及更换软件包 源 [这篇博文](linux-xi-tong-huan-zhong-ke-da-jing-xiang-yuan.md)会帮助到您

以及第三方软件包源

### 文件检视

#### 您应当学会 简单 的查看文件路径 命令 `ls` 切换目录 `cd` 切换至上级目录`cd../`&#x20;

修改文件权限啦，创建文件 copy文件，删除文件啦等命令 熟练掌握。

## 正文

在您掌握上述基础的同时，我想我们可以开始关于BGP 学习的探讨。

### 网络接入

#### 首先，我建议请先了解[**VPN**](https://zh.wikipedia.org/zh-tw/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)  [**Tunnel**](https://en.wikipedia.org/wiki/Tunneling\_protocol)  的存在，与你国逾越防火长城**代理**差异还是很大的。

#### 我们需要理解 谁是我们的网络 上游和下游&#x20;

而VPN 工具 network tunnel各种协议 正是帮助我们 接入我们所互联的方式/工具

例如 [WireGuard](https://www.wireguard.com/) [SoftEther](https://www.softether.org/)&#x20;

又或像 GRE tunnel&#x20;

当然在这一步 甚至物理光纤 接入

在您未接入对方网络，或者对方网络未接入您的同时，我们又怎么可能会存在BGP 会话 和Peer呢？

### 网络知识

你需要有一定的网络知识 理解

从域名（domian） DNS （DNS服务器）端口（port）到IP（ipv4 和ipv6 ）各种网络协议

常见的TCP UDP SFTP FTP&#x20;

到网卡 网关 Mac地址 &#x20;

甚至 多跳 多播 路由 穿透 这些都请了解

### 报错

您需要  **报错 查阅** 这样才可以帮助您学习，当然 还有请教大佬，不厌其烦的打扰他们，有一个好的领路人 会让你在未知领域的学习事倍功半。

有一定的学习能力。

当你有一日 能够读懂 大佬的代码 并且 写出适合自己的成功跑起来。恭喜你 你就向前进一步迈进了

### Bird or Bird2

路由存在任何网络，哪怕是内网，您自家的路由器和智能设备的连接中。



既然我们不采用 硬路由 ，并且在软路由/Linux 系统 上 配置路由网络。

[Bird](https://bird.network.cz/)将会是我们最友善的工具。

#### 首先，我要提一嘴。目前bird 有两个分支 一个是[bird](https://bird.network.cz/?get\_doc\&f=prog.html\&v=16) 另一个是[bird2](https://bird.network.cz/?get\_doc\&f=prog.html\&v=20)

#### 即意味着 您 查阅到的教程或者 大佬的代码 有可能使用的 版本差异。您可以通过 `birdc` 这样的命令查询您使用的bird版本。

而两者的最大的差异 莫过于 bird是将ipv4和ipv6分开配置的 `bird.conf` `bird6.conf`&#x20;

而bird2 仅在`bird.conf`   区分bird2 在您检阅 抄大佬配置的时候 区分在配置中「Ipv6」

#### 注意⚠️：bird的配置 不适配bird2，同理bird2的配置也会在bird1中报错。俩者的版本写法具有差异性，但是阅读起来 大同小异。



您需要了解 广播（Announce） 过滤（filter）以及Peer 等写法 IGP BGP  OSFP

详情请查阅 [官方文档](https://bird.network.cz/?get\_doc\&f=prog.html\&v=16)

适合萌新入门[文档](https://github.com/moesoha/bird-bgp-kickstart) sohajin&#x20;



欢迎加入[MoeqingZoo](https://t.me/MoeQing) （telegram ）在这里 探讨与BGP相关的事情，



### DN42

前面 我们写到还有低成本的BGP学习。

我们可以找到DN42 是一家提供内网 BGP学习的地方。

（其实不厌其烦的话你甚至可以在自己的内网配置IGP协议



我没有配置过DN42，不过DN42和公网直接配置BGP 有一定区别。但是区别又不大。

[官方文档](https://dn42.eu/Home?)

[文档](https://blog.baoshuo.ren/post/dn42-network/) baoshuoren

[文档 ](https://miaotony.xyz/2021/03/25/Server\_DN42/) miaotony

[telegram](https://t.me/+T6k9KgL\_yjIkau4k)



## 文末

最后，写此篇文章 只是想给 初探BGP 网络世界的人一点思路和见解/

您仍需要不断的学习，这些工具`ping` `mtr traceroute` 或许对您的网络有一定帮助

ip link 网卡的设置&#x20;

防火墙 放通

这些我都没说，但是您一定会遇到，很多很多要实现的各种麻烦。请不厌其烦的做下去。您终将不会后悔的。

您可以在[这里](https://bgp.he.net) [这里](https://bgp.tool) 网络拓扑图观测到您的配置

#### 最后的最后，感谢我的两位老师兼朋友 [yekong](https://www.moeyk.com/) [haima](https://blog.peers.cloud/)

没有他二人的帮助，我的BGP学习还将在慢慢长路中无从下手！感谢！！





