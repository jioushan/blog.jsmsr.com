---
description: shadowsocks、ssr、trojan、v2Ray
---

# 泛談科學協議選擇

{% hint style="danger" %}
本篇文章具有时效性！与您当下阅读此文字具有出入性！
{% endhint %}



如果你久违发现我很久没有更新内容,那么恭喜你还在关注我.\
许久几个月我的确偷懒到,想写一些什么内容.\


### 首先

这篇文章并不会在要喝茶的站点上发布的.所以要阅读的朋友.仅此可以看到.\
好的.每当我要写一些内容或者说发布一些观点.分享一些奇特的知识.我一定的是有起因的.这个我们接下来聊.\
先.恭喜我考过科目二.不过要拿到驾照得等到明年了.属实头疼.没法约考.

### 观察

\
前段时间.youtube上有着一位视频主 Ak.在讲这方面话题.不过业余对于这方面感兴趣的孩纸.我应该细讲一下

\
不过不需要扯很多理论.资料.我只需要言简意赅 能让大家理解就ok.\
很久之前,我蹭分析过v2ray+tls+CDN 嗯,嵌套CDN这种方式,也许不久后我蹭说过IPLC这类线路.它们不受到\
GFW 的审查.较于前者公网而言.相对更fast best.\
在19年左右..ss这种方式已经不安全了.其实并没有绝对百分之百的安全,任何协议.纵使,ss,ssr,trojan,\
weirgard,v2ray,book.\
各种混淆协议.多个代理IP.嵌套,中转.\
在实际体验上.会因为.复杂的协议与解析.带宽运营商的Qos.阻断.链路未peer.绕路.带宽.,丢包.时延.等问题使得\
得不到较好的体验,然而这些问题就是要解决,以击避免的问题.\
所以有人会提出.自建DNS服务器避免解析阻断\
嵌套或者中转.使得国内-国内的中转机 出墙.这种能够解决很大部分的阻断问题.,以击运营商骨干网绕路问题.\
比如某节点 属于CN2 ,但是移不动缺要绕路.,你可以使用一台联通/电信的中转机 来当跳板.中国移动国内连接那台中转机\
联通/电信.也可以得到有效且优化的CN2 访问

### 相对而言.

大家知道国内的公网 移不动.联不通.电不信 .其中电不信的CN2 昂贵以及占比较高.\
海外的运营商连接国内的运营商,如果没有很好的peer或者.对接.那么就很有可能美国绕路.这就是要极其避免的\
延迟 丢包问题.\
所以移动有自己的优化CMI,联通可以尝试iij或者ntt.虽然亚洲ntt 经常爆炸.但电不信除了极其难受的163骨干网就是\
拥有流畅优化的CN2\
上面是公有化线路.,还有众多私有化线路.也许他们的源头供应商也是上面的某一家.如果是国内的网络公司.海外线路的租聘\
就是另一种发展了.\
还有中国教育网以击私有化的中国科研网 其中科研网不受GFW审查.\
剩下就是一些稀有的IPLC 这种点对点内网跨国线路. 昂贵的带宽 租赁费用.\
因为是内网线路.不存在有GFW 这种受审限制.拥有低延时,.高稳定性.等优点.

### 讲回协议.

并非复杂的协议就一定安全.在某一种程度.最简单的反而效率更强.也易于优化.复杂的对于终端设备的解析能力\
处理器芯片的解析能力也是一种挑战.

我收回我曾说过的ss已死这种结论.只是你并未有选择较安全的一种混淆协议.以及安全的端口出墙方式.\
或者说如果某IP段.或者说某ASN 供应商的IP处于敏感被监视的状态.在紧急敏感时期这种ip就是被优先照顾的状态.

其次国家并没有严重管理,或者说是一种允许的手段可以允许你使用.但是不应该传播.侮辱言论散播.而对你进行监控.所谓喝茶行为.

### 以後單獨寫點這方面文字關於我的看法。

### &#x20;自行珍惜.

\
出去丢人不如不出去.\
\
