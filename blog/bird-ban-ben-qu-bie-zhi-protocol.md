---
description: Only IPV6 【About config】
---

# 🌏 bird版本區別之 protocol

当我准备写这篇文章时 我准备讲些什么了

你知道的，我们采用bird，所以你用的是**硬路由**！请CTRL+W或者command+W。此**教程对您没有任何帮助！不值得您在这儿浪费时间！**

而且，我写的一些内容，是希望初入BIrd的小白能够在闭门造车的情况下，方便抄大佬的代码！

所以，前提是要明白，如何读懂！

### 准备

我前面提到过，bird有1.6.8 有bird2 甚至还有没推出的bird3

但目前。中文区大量的代码采用bird1.5或者bird2

（系统环境我假定您和我一样用的Debian

### 所以请看下面两者区别

`protocol bgp j1s2m3s4r5 {`

&#x20;   `local as 134478;`&#x20;

&#x20;   `source address xxxx:xxxx::1;`&#x20;

&#x20;   `import none;`&#x20;

&#x20;   `export filter {`&#x20;

&#x20;          `include "./peers/filter.conf";`&#x20;

&#x20;          `reject;`&#x20;

&#x20;`};`&#x20;

&#x20;   `graceful restart on;`

&#x20;          multihop 2;&#x20;

&#x20;         neighbor xxxx:xxxx::2 as 53667;&#x20;

&#x20;         password "xxxxxx";&#x20;

}

这是**bird1**的conf（config

首先`j1s2m3s4r5` 您可以自由填写，这个protocol的会话名称但是首字母请务必是字母，否则bird会**报错**

`local as 您的ASN`

`source address` 您本地local address （即我举个例子

您通过GRE方式和某A建立了隧道。（这是二层协议。所以这个地方的地址即对方分配给您的客户端地址

`import none;` #导入 `none` #是不导入 `all`#是全收 `filter` #即代表你的过滤

`我们这节不讲过滤 所以 filter怎么写我们暂时先不聊！`

`graceful restart on` 是自动平滑重启

neighbor 我不知道翻译成怎样好 邻居这个翻译 感觉怪怪的。但是你要知道 后面的地址是你对端BGP它的local address 所以这里您应当写它给你的服务端地址 as 对方的ASN

&#x20;multihop 2; 是多跳 为2 ； password XXXXX 事实上多跳和密码 这似乎是您的上游制定的。

对于一般的BGP会话只需要写

local address as Asn

neighbor address as Asn

导入

导出

#### 当您理解这些，我们来看bird2的写法

`protocol bgp he{`&#x20;

`local as 134478;`&#x20;

`source address xxxx:xxxx::2;`

&#x20;`neighbor xxxx:xxxx:xxxx::1 as 6939;`&#x20;

&#x20;`ipv6{`&#x20;

&#x20;          import none;

&#x20;          export all;

&#x20; };

&#x20;}

我相信这次我不需多言。你就可自己看懂。

### 我们来说说一致性的内容，

`route id xxx.xxx.xxx.xxx;`

\#这里的ipv4随便填 最好是机器的ipv4 地址

`protocol bgp xxxx{}；`

\#然后你可以写你的 BGP会话（就是上面我所言区别的内容

`protocol static{`

&#x20;`route XXXXXX/44 reject；`#宣告为这个段，路由将导向这台机器

&#x20; `route xxxxxx/48 via “local address”` #将这个段的路由导向某个网卡

`}；`

\#这里是静态地址宣告

### 我这边教程写的不好，只是举一个例子，更多我建议参考，以下网站

[bird.wiki](https://wiki.skywolf.cloud/)

但是您要是说 bird的各种功能组件写法，咱还是建议您翻sohajin的[文档](https://soha.moe/post/bird-bgp-kickstart.html)



~~未完待补充！~~

{% hint style="info" %}
博主很懒，所以没心思更！
{% endhint %}
