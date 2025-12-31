---
description: 实名上网
---

# 🌏 在公網宣告地址須知

基于种种因素，我决定将自己踏坑的经验整理出来。（最近翻技术的时候，看到别的大牛写的一些文章。自愧不如）当然，从某些因素而言，我的BGP知识不过还是小白。如果这点帮助有用的话，希望能够帮助那些有興趣的學習者，微不足道的解釋。毕竟有些内容 业界大牛反比起我 更优秀。咱是一隻菜雞。

2021年，注意这篇文章是2021年，以后的变动与作者无关

### 首先&#x20;

根据不同的地域 ，您应当向**對應區域** 所在的**组织** 註冊账号。

<figure><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Regional_Internet_Registries_world_map.svg/620px-Regional_Internet_Registries_world_map.svg.png" alt=""><figcaption></figcaption></figure>

比如歐洲，RIPE NCC 。**注意，不通區域註冊管理局 資源管理自治權限不同。**

### 然後&#x20;

您應當 尋找LIR申请一个對應區域的ASN号。「個人與企業」

如何找到LIR？请访问[此链接](https://www.lowendtalk.com/discussion/160162/the-aio-ip-related-thread-ipv4-ipv6-asn-only-providers-are-allowed-to-post-offers-2)，这个论坛帖子有全球大量的LIR及ASN求购♂家。

或者[聯絡我](https://t.me/jioushan)，我盡我所能為您解答您的疑惑，解決您的問題。

#### 當然，若您有自己的需求，您可以註冊一家海外公司，通過加入所在地區互联网自治区域 加入 会员，支付一筆不菲的费用，那么你自然可以申请到ASN号资源，以及可以分配的ipv6资源。（等，好处多多

比方說：APNIC [https://www.apnic.net/get-ip/apnic-membership/](https://www.apnic.net/get-ip/apnic-membership/)



### 其次

與lir接洽，字段的創建。大部分管理註冊局都有對應的文檔翻閱。

引用之前補檔內容，RIPE NCC的字段[創建](https://www.jioushan.top/2022/05/31/ripe-%E6%B3%A8%E5%86%8C%E5%8F%8A%E5%AD%97%E6%AE%B5%E5%88%9B%E5%BB%BA/)

直到當您已经拥所屬與您的ASN。

您需要學習，有一定網路基礎。通過路由器 或者 bird等路由工具， 將您的地址宣告。

完善您的ASN資料在 [peeringdb.com ](https://peeringdb.com) 便于更新抓取。



### 後續您的ASN觀察

[bgp.he.net](https://bgp.he.net) . [bgp.tools](https://bgp.tools)



### 補充

如您是以個人身份註冊您的ASN，謹防实名上网。



關於如何广播宣告您的ipv6 prefix。首先您要諮詢您的ISP/上游是否具有給您广播BGP的能力？<br>

[列举网站](http://bgp.services/)，可以广播 BGP及售卖ip地址段，lir等商家汇总。数据不一定全。



強烈建議 新手 去[vultr](https://vultr.com) 實驗學習。關於如何在vultr上廣播宣告prefix。它們文檔寫的很清楚。以及關於LOA的製作，您需要的無非是模板。建議您參考cloudflare[模板 ](https://developers.cloudflare.com/byoip/about/loa/)

\
後續內容请续读这里！[URL](https://blog.jsmsr.com/blog/bgp4-zuo-wei-ge-ren-yong-hu-de-kan-dai)
