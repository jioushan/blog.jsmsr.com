---
description: まず、明けましておめでとう　来年も宜しくね。
---

# 在终端 访问http2 website

まず、明けましておめでとう　来年も宜しくね。



這個話題是個很老的話題了,我知道大家都是Linux使用者，但是在shell中瀏覽網頁，很多人應該都想到過，

但是現實是 幾乎現在主流的網站前端都不再是單純的超文本網站，有太多javascript元素在裡面，沒錯談到這個話題，是我今天下午看到很多人都在評測 \
拉\<NPC<人上人<頂級<夯\
&#x20;![https://commons.wikimedia.org/w/index.php?curid=180700275](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Chinese_Language_Blank_Tier_List.svg/2560px-Chinese_Language_Blank_Tier_List.svg.png)

這樣的一張圖片，很多人都在評測瀏覽器，ブラウザ，每個人持有不同的觀點，但是我要提的是我也有自己的觀點，很多人說的一些錯誤言論 我並不完全認同。

但是我們今天要講 關於 在今天，shell瀏覽超文本網站,至少不至於表現很差。

Linux上面有不少，類如`w3m` `lynx` `links`

這樣的工具包，但是如果你瀏覽的哪怕有點javascript元素的網站，你會發現，沒錯它連當下Google都無法正常使用，這真的是我們想要的嗎？

```
  ブラウザを更新してください
  お使いのブラウザのサポートは終了しました。検索を続けるには、最新バージョンにアップグレードしてください。 詳細
```

沒錯，google 根本就不支持它。 這讓我想到一個項目叫 [Vieb](https://github.com/Jelmerro/Vieb)

我們去它的[官網下載](https://vieb.dev/download)，意外的發現它們對 很多端都有支持，可是事實上是什麼，如果你運行的是一台實體具有GPU的PC，或許你可以吃上chromemiu內核的shell網站，它依賴Electron進行通過GPU進行渲染，

```
xvfb-run -a vieb --no-sandbox --disable-gpu --headless google.com
```

由於我絕大多數都是server,這些サーバー有一個特點就是幾乎都不配備GPU 你或許會check system info的時候發現它們GPU這一欄會持帶,不幸的是

```
GPU: Amazon.com, Inc. Device 1111 (VGA compatible)
```

你指望這樣的GPU可以工作那麼你就太天真了,沒錯它幾乎沒辦法從我的servers上面工作，應該是我的內存吃緊也好各種原因也好，Vieb適合已經有GUI 圖形化介面的Linux用戶,錦上添花的一件事。

但是對我來說，這讓我想到還有一個 今晚的壓軸 那就是 `brow.sh` 它採用firefox，而且可以通過headless方式訪問對方的網站並且渲染到ttl終端呈現給我們。<br>

<figure><img src="../.gitbook/assets/スクリーンショット 2026-01-01 1.37.18.png" alt=""><figcaption></figcaption></figure>

我的這張圖片，或許你認為，瞧圖片都是馬賽克，好吧我承認如果你希望有高質量的請選擇vibe。但是如果你的system或許只有512M RAM，開啟swap的情況下，閱讀純文本的網站，輕鬆有餘而且js相關的內容是正確展示的， \
[安裝介面](https://www.brow.sh/docs/installation/)

[快接鍵使用介紹](https://www.brow.sh/docs/keybindings/)

你可以通過它們的`toml`文件深度定製話，據我所知，它們甚至可以藉由Firefox的插件進行使用。

\
\
\----------------\
這是我的2026年 新的一年的博文，結尾 讓我們放一首 MISA-愛の形

​​

{% embed url="https://embed.music.apple.com/jp/album/%E3%82%A2%E3%82%A4%E3%83%8E%E3%82%AB%E3%82%BF%E3%83%81-feat-hide/1538122052?i=1538122206" %}

祝願閱讀的客人，新的一年，您也是一切順利。
