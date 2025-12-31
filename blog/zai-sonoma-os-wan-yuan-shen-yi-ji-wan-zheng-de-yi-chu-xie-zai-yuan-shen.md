---
description: Sonoma OS play GenshinS
---

# 在 Sonoma OS 玩 原神 以及 完整的移除卸載原神

### 前言 這確實是一篇playcover教程，但是這是一個能解決小bug的教程，如果您有興趣。

請耐心閱讀不到1min的敘述。

首先我的mac OS版本是14.1.1 這個問題在14.0也存在過。

唯一的期望就是 後續 playcover的開發者可以解決這個問題。

一般來說，我們都是去play cover的github倉庫下載它的最新版本。

[https://github.com/PlayCover/PlayCover/releases](https://github.com/PlayCover/PlayCover/releases)&#x20;

因為 是sonoma版本，所以目前只能 使用3.0.0版本的beta版本。

當你按照以往安裝 Playcover 然後 使用 [https://decrypt.day/](https://decrypt.day/)

獲取了 Genshin 的安裝ipa拓曳到playcover 安裝之後。這時候。請不要急於點開，哪怕是點開，也會迸出閃退到錯誤。

這便是我們遇到的一個問題。

### 解決&#x20;

首先請不要使用 github下載的版本，請去gitee下載 開發者所公布的這個每夜版

{% embed url="https://gitee.com/playcover_community/PlayCover/releases/tag/3.0.0-nightly528" %}

請確認你使用的 這個 版本

`3.0.0（528）`

請右鍵 所安裝的ipa文件，點擊 偏好設定\
我們需要打開 - 啟用繞過越獄監測<br>

<figure><img src="../.gitbook/assets/截圖 2023-11-19 11.33.31.png" alt=""><figcaption><p>啟用繞過越獄監測</p></figcaption></figure>

或者 我們可以選擇在 雜項 -移除 playcover

<br>

<figure><img src="../.gitbook/assets/截圖 2023-11-19 11.34.26.png" alt=""><figcaption><p>移除playcover</p></figcaption></figure>

我們右鍵這個 app 在finda中查看。\
\
然後我們打開終端機 輸入下面的指令<br>

```
 sudo codesign --force --deep --sign - /Users/xxx/Library/Containers/io.playcover.PlayCover/genshinn.app
```

`/Users/xxx/Library/Containers/io.playcover.PlayCover/genshinn.app`\
這裡這串路徑 請自行 拖動 您的電腦 終端機的 app的路徑

鍵入並回車，輸入 root密碼，耐心等待。在 Finda 中 再次運行 原神



### 題外話

當然也有有的朋友說，移除 playtools這一步就可以 genshin start了，但是據我 所使用體驗，一般這種操作 您所運行的genshin無法通過 鍵盤 映射 遊玩 也無法連結 手柄 通過 手柄 遊玩。使用這種 方式 打開的 原神既保留了 playcover工具 可以映射。鍵盤 也可以 使用手柄。



### 如何 移除 原神

如果你只是 command+backspace將genshin的安裝app清空垃圾桶 這並不代表 您完整的移除了原神。

mihoyo會在您遊玩原神的過程中 將 遊戲本體數據 下載到以下路徑 還請手動 刪除它們。\
尤其對於 mac 硬碟 為數不多的人。<br>

<figure><img src="../.gitbook/assets/截圖 2023-11-19 10.42.54.png" alt=""><figcaption></figcaption></figure>

注意區分 使用者-後面 您mac 的 用戶名 替換。
