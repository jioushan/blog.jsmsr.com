---
description: AWS、azure、gcp、vultr
---

# 国内三大运营商对于大厂的路由跟踪如何。

{% hint style="danger" %}
本篇文章具有时效性！与您当下阅读此文字具有出入性！
{% endhint %}

<figure><img src="https://cdn2.jioushan.top/LightPicture/2022/03/c2d3e705fbdbf55b.jpg" alt=""><figcaption></figcaption></figure>

以前写过SS教程。不过一键脚本啥的。傻瓜操作。\
&#x20;为了延迟绞尽脑汁！虽说极大程度属于薅羊毛。顺手就写点经验之谈。&#x20;

国内三大运营商对于大厂的小鸡线路怎样。&#x20;

## GCP tw

\
&#x20;起因：&#x20;

**移动的路由表就是有坑**\
&#x20;同样的节点，隔壁电信，联通，延迟不超80\
&#x20;移动一跑，几乎200&#x20;

开了几台**GCP tw/jp** 有人说，hk联通回城也会绕。为了直接直连。GCP联通/电信首选tw\
&#x20;移动都是跑美国圣何塞绕圈。巨难受。为了避免这种延迟。单纯做网站80/443端口加速，可以选择。\
&#x20;cloudflare CDN（hk）节点对移动很友好。延迟不超60ms&#x20;

再说说其他家的主机。主要讲**jp，轻度涉及hk，tw，sg**也会说一说&#x20;

目前只有移动家宽节点，单纯凭山东移动这边测试数据说点经验。&#x20;

## vu（vultr）&#x20;

jp 直接圣何塞绕圈。ok？不要想。而且vu的104节点千万不要开。pixiv被屏蔽。就不多说了&#x20;

现在的VU 应该是北方移动 北京出口\
&#x20;我有一次开到vu 66节点的很香。但是那可能真的是运气好。&#x20;

## digitalocean&#x20;

\
&#x20;目前国外网站服务器主机用的就是这个（我能说上面跑着很多站点吗？不能停机），估计今年年底左右就会\
&#x20;换了。在用的是sg 应该是对电信很友好。联通也可以用。说起移动。走的香港ntt。总体还可以。但是\
&#x20;digitalocean没有jp。digitalocean适合us/sg&#x20;

## azure jp/hk&#x20;

\
&#x20;移动首选。其他电信联通，选hk就对了。jp走的也是hk。会hk绕路。\
&#x20;三网差不多，联通，电信就不如选择aws了延迟好很多了。&#x20;

## AWS lightsail jp&#x20;

\
&#x20;电信比联通好些。移动最差。&#x20;

移动走的北京出口直连，日本equinix的机房应该是。&#x20;

AWS的 US 移动是 **上海** 出口&#x20;

1/12 日期\
&#x20;最近有点赌运气。GCP hk&#x20;

选**asia-east2-b** 这个区。**34.92./**&#x8FD9;个段移动绕日本东京。延迟110左右。赶上我隔壁azure jp100ms了&#x20;

更香的在后面 **35.220./**&#x8FD9;个段移动直连香港。北方北京出口->广东->hk 50ms太香了。广州延迟估计也就1x ms&#x20;

![](https://cdn2.jioushan.top/LightPicture/2022/03/93c1eeaee6cc0671.png)属实 香&#x20;

<figure><img src="https://cdn2.jioushan.top/LightPicture/2022/03/a40b347c35e6fabd.png" alt=""><figcaption></figcaption></figure>

### GCP osaka（大阪）不要碰。看了看GCP说明。感情大阪分出来IP不论哪个区，一个机房。ip 怎么整都会环球旅行。绕美就算了。伦敦也  旅游。纯粹就等于浪费时间。&#x20;

### GCP（电信/联通）>AWS lightsail(电信/联通)>azure（三网一个样。太平均了。）&#x20;

linode和ECS（阿里国际）没用过。曾经有过kagoya的节点。和azure类似。直连jp（osaka）可惜，没了。\
&#x20;等我用过后，在补档。&#x20;

## linode&#x20;

[点击此连接](https://paste.ubuntu.com/p/RNMgKVGCtv/) 获取linode 到国内各个运营商的路由信息！&#x20;
