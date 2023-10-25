---
description: iwdctl
---

# 在archLinux上配置wlan0（wifi）

或許是我菜的原因，我在archlinux上用的一些網卡工具配置Wi-Fi 連結都失敗了。

但是這裡我要推薦一個很好用的工具

### iwd

如果你在arch上使用pacman來安裝軟體

`paceman -S iwd`

Debian系 `apt-get install iwd`

Fedora `def install iwd`

#### 鍵入 `lwctl` 進入它的配置命令行

1.  ```
    device list
    ```

    我們可以看到我們的無線網卡
2.  ```
    station wlan0 get-networks
    ```

    通過 我們網卡 wlan0 設備檢肅附近Wi-Fi SSID Name
3.  ```
    station wlan0 connect  <your wifi ssid name>
    ```

    連結到你要加入到Wi-Fi
4.  輸入Wi-Fi密碼

    正常情況下 應該就完成連結了！真的超easy
5.  ```
    ctrl+C
    ```

    終止iwdctl 就好了，可以通過 `ip a` 或則 `ping -c`

    來檢測 是否正確的獲取到網卡！





### 後記：

我一開始有嘗試，`wpa_supplicant` 又或者 dailog的`wifi-menu`

Wi-Fi-menu 有界面化了，但是 鍵入密碼 一路ok之後它還是連不進，更不要說還要install `netctl` start \<network\_profile> 有多失敗了，反到是 iwdctl 唷夠省心w

附：

### 在archlinx上切換 中科大鏡像源 喔～

我們使用vim 編輯這個路徑下文件吼～

```
vim /etc/pacman.d/mirrorlist
```

我們把這個添加至尾部

```
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```

`:wq` 保存並退出嗷～

然後繼續，

```
vim /etc/pacman.conf
```

我們繼續編輯這個文件。

在尾部添加以下

```
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

然后请安装 `archlinuxcn-keyring` 包以导入 GPG key。



結束了喔～
