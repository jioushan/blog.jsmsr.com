---
description: 一些疑難雜症，希望可以幫到
---

# 不完整的Gentoo安裝

`不過我建議你應當滿足/準備以下`

`1.強大的內心`

`2.一塊U盤和你要安裝的`磁碟

`3.獨立思考的大腦`

`4.不厭其煩的嘗試`

`5.`[`chatgpt`](https://chat.openai.com) `& Google & 官網文檔`

## 開始

！！！`systemctl` ⚠️

前半部分是在`live CD` 狀態下操作的，我這裡就不執行分割了，分割工具有像`cfdisk`，`fdisk`

分割 建議了解下 `MBR`分割表 和`GUID` 分割表區別，我覺得你都是`Linux`用戶了，您應該很上手。

`liveCD` 環境下

```
mkdir /mnt/gentoo
```

創建 /mnt/gentoo

```
mkdir /mnt/gentoo/boot
```

創建 /mnt/gentoo/boot

通過`lsblk` 看到我這邊 對sda已經操作完成 分割完成。

sda1 用作 boot ，sda2 用作操作系統路徑

```
root@archiso ~ # lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0 693.5M  1 loop /run/archiso/airootfs
sda      8:0    0 119.2G  0 disk 
├─sda1   8:1    0   256M  0 part 
└─sda2   8:2    0   119G  0 part 
sdb      8:16   1  28.8G  0 disk 
├─sdb1   8:17   1   798M  0 part 
└─sdb2   8:18   1    15M  0 part 
sr0     11:0    1  1024M  0 rom 
```

```
mkfs.vfat -F 32 /dev/sda1
```

```
mkfs.ext4 /dev/sda2
```

我怕你看不懂，我這裡簡單說下，sda是我要用來存放gentoo的硬盤

sda1用作boot 給了256M，sda2 是系統路徑

### 格式化 磁碟

然後我們通過mount 讓sda掛載到這個系統上。

```
mount /dev/sda2 /mnt/gentoo
```

```
mount /dev/sda1 /mnt/gentoo/boot
```

### stage3 配置

```
wget https://mirrors.ustc.edu.cn/gentoo/releases/amd64/autobuilds/current-stage3-amd64-desktop-systemd/stage3-amd64-desktop-systemd-20230507T164658Z.tar.xz
```

```
tar xpvf stage3 <tab> --xattrs-include='*.*' --numeric-owner
```

解壓完之後 配置make的conf

```
vim /mnt/gentoo/etc/portage/make.conf
```

```
COMMON_FLAGS="-march=native -O3 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
​
# NOTE: This stage was built with the bindist Use flag enabled
​
# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C
​
USE="-bindist"
GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"
ACCEPT_LECENSE="*"
MAKEOPTS="-j4"
```

### 然後創建源

```
mkdir --parents /mnt/gentoo/etc/portage/repos.conf
```

```
vim /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
```

### 复制DNS信息 <a href="#fu-zhi-dns-xin-xi" id="fu-zhi-dns-xin-xi"></a>

```
 cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
```

複製live CD 下的dns到 掛在gentoo下的dns 文件`/etc/resolv.conf` 這個文件一般都是DNS配置

### 挂载必要的文件系统

```
 mount --types proc /proc /mnt/gentoo/proc
 mount --rbind /sys /mnt/gentoo/sys
 mount --make-rslave /mnt/gentoo/sys
 mount --rbind /dev /mnt/gentoo/dev
 mount --make-rslave /mnt/gentoo/dev
 mount --bind /run /mnt/gentoo/run
 mount --make-slave /mnt/gentoo/run
```

其中带有 –make-rslave的项目是使用systemd才需要的,如果你使用openrc可以不用运行.

如果你不使用Gentoo的livecd的话,需要运行下面的命令让/dev/shm/目录称为一个正常挂载的tmpfs

```
test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm
chmod 1777 /dev/shm /run/shm #如果报错，则去掉 /run/shm部分
```

感謝這裡一小部分教程來自[ta](https://gtrush.com/2022/06/12/%E6%96%B0%E6%89%8BGentoo%E6%8A%98%E8%85%BE%E8%AE%B0%E5%BD%951-%E5%AE%89%E8%A3%85%E7%AF%87-%E4%BA%8C%E8%BF%9B%E5%88%B6kernel%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95/)。

### 進入chroot

```
chroot /mnt/gentoo /bin/bash
```

```
source /etc/profile
```

```
export PS1="(chroot) ${PS1}"
```

如果遇到bash無法操控命令時

```
export PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/root/bin
```

讓我痛苦萬分的portage（包管理器）

```
vim /etc/portage/make.conf //make的路徑
```

### 編輯 make 配置

```
 rm -rf /var/db/repos/gentoo //如果出現問題再刪除重試
```

刪除已經配置的，再一次嘗試

以下是正常的配置順序

```
emerge-webrsync  //如果這裡執行出現報錯，請參考上面make的路徑進行參數修改
```

具體參數可以參考它的make配置參數 [URL](https://blog.yangmame.org/Gentoo%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.html)

繼續按步驟執行命令

```
eselect profile list
```

這裡你要選擇一個合適你的 list

```
eselect profile set X
```

```
emerge -auvDN --with-bdeps=y @world
```

//執行這條命令時 ，你應該準備 出門/開始看電影，或者躺平，或者有一顆強大的心臟，具體時間看您的機器配置，它讓我煎熬的坐了3個小時。

如果遇到什麼奇奇怪怪的報錯，善用搜索引擎，不過我想搜索引擎也不一定幫的上，善用GPT

### 配置時間和地区 <a href="#pei-zhi-shi-qu-he-di-qu-xin-xi" id="pei-zhi-shi-qu-he-di-qu-xin-xi"></a>

```
echo "Asia/Hong_Kong" > /etc/timezone
emerge --config sys-libs/timezone-data
echo "en_US.UTF-8 UTF-8 zh_CN.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
```

选择语言,選eng出錯少

```
eselect locale list
```

```
eselect locale set X 
```

同步环境

```
. /etc/profile
```

重新加载环境

```
env-update && source /etc/profile && export PS1="(chroot) ${PS1}"
```

### 內核安裝

這裡官方寫的好複雜，不過感覺可以嘗試通過2進制的方式安裝。建議參考其他大佬教程這一步，這裡我沒有任何話語權，這裡時間應該不會浪費很久。



跟著官方教程走即可，接下是創建root的passwd

```
passwd
```

務必創建一個複雜的密碼保障安全。

### 接下來是fstab

我的建議是`exit` 先退出chroot模式，直接複製`liveCD`下的fstab到gentoo這邊的路徑

```
echo "/etc/fstab" >> /mnt/gentoo/etc/fstab
```

然後再使用上面的 進入chroot模式回到我們的最後步驟

#### 安裝配置grub引導

如果你在前面編輯make.conf的時候沒有添加這個參數，請將這個`GRUB_PLATFORMS="efi-64"`參數放進去

```
echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf
```

然後執行編譯安裝grub

```
emerge --ask --update --newuse --verbose sys-boot/grub
```

然後你可以執行這句話寫入boot

```
grub-install --target=x86_64-efi --efi-directory=/boot
```

⚠️這裡僅使用uefi的引導方式，如果妳是其他的分區表或者傳統引導，建議參考[官方文檔](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader/zh-cn)

## 最終

退出chroot环境并unmount全部已持载分区。然后敲入一条有魔力的命令来初始化最终的、真实的测试：reboot。

`root# exit`

```
umount -l /mnt/gentoo/dev{/shm,/pts,}
```

```
umount -R /mnt/gentoo
```

```
reboot
```

退出，并取消掛載，重啟，至此，如果順利的話 我們不再用到liveCD。

## 進入系統後

### 包管理器install dhcpcd

```
emerge -av dhcpcd
```

#### 安裝vim

```
emerge --ask app-editors/vim
```

#### 庫搜索mtr

```
emerge --search mtr
```

### &#x20;vim裝不上報錯

###

```
Total: 10 packages (10 new), Size of downloads: 28,500 KiB
​
 * Error: circular dependencies:
​
(media-libs/libsndfile-1.2.0:0/0::gentoo, ebuild scheduled for merge) depends on
 (media-sound/mpg123-1.31.3:0/0::gentoo, ebuild scheduled for merge) (buildtime_slot_op)
  (media-sound/pulseaudio-16.1:0/0::gentoo, ebuild scheduled for merge) (buildtime)
   (media-libs/libpulse-16.1-r2:0/0::gentoo, ebuild scheduled for merge) (buildtime)
    (media-libs/libsndfile-1.2.0:0/0::gentoo, ebuild scheduled for merge) (buildtime)
​
It might be possible to break this cycle
by applying any of the following changes:
- media-libs/libsndfile-1.2.0 (Change USE: +minimal)
- media-sound/mpg123-1.31.3 (Change USE: -pulseaudio)
​
Note that this change can be reverted, once the package has been installed.
​
Note that the dependency graph contains a lot of cycles.
Several changes might be required to resolve all cycles.
Temporarily changing some use flag for all packages might be the better option.
```

#### chatgpt的有效解決方案

這個報錯顯示了 "media-libs/libsndfile-1.2.0" 和 "media-sound/mpg123-1.31.3" 之間存在循環依賴關係，其中 "media-sound/pulseaudio-16.1" 和 "media-libs/libpulse-16.1-r2" 也被列入了其中。

報錯提供了一些可能的解決方案，其中一種方法是通過更改 USE 標誌來打破這個循環依賴關係。在這種情況下，你可以嘗試使用以下命令：

```
​
USE="-pulseaudio" emerge -1 media-sound/mpg123
​
USE="+minimal" emerge -1 media-libs/libsndfile
​
emerge vim
```

這些命令將首先安裝更新 "media-sound/mpg123" 和 "media-libs/libsndfile" 軟件包，通過在安裝時更改它們的 USE 標誌來打破循環依賴關係。然後，你可以使用 emerge 命令安裝 vim 軟件包。

請注意，在成功安裝 vim 之後，你可以使用以下命令恢復 USE 標誌的原始設置：

```
USE="pulseaudio minimal" emerge -1 media-sound/mpg123 media-libs/libsndfile
```

這將更改 USE 標誌以恢復原始設置，同時更新這兩個軟件包，這樣你就可以在不更改系統狀態的情況下重新安裝它們。

### 添加部分驅動（先解鎖許可證

这个错误提示说明 `sys-kernel/linux-firmware` 软件包的所有可用版本都被掩藏了，原因是这个软件包包含了需要满足 `linux-fw-redistributable` 许可证的文件，但这个许可证并没有被 Gentoo 批准。

有两种解决方法：

1. 使用其他发行版的 firmware 或者从其他地方获取到该许可证文件。这种方法需要自己手动获取并安装，不保证能够成功，同时也存在风险。
2.  在 `/etc/portage/package.license` 文件中添加 `linux-fw-redistributable` 许可证，这个许可证可以在 `/var/db/repos/gentoo/licenses/linux-fw-redistributable` 文件中找到。具体做法是，在终端中执行以下命令：

    ```
    bashCopy code
    echo "sys-kernel/linux-firmware linux-fw-redistributable" >> /etc/portage/package.license
    ```

    然后再重新执行安装命令。

    ```
    emerge -av sys-kernel/linux-firmware
    ```

    注意：这个许可证的存在是为了防止某些二进制固件被非法复制和分发，因此在添加许可证之前请仔细阅读许可证内容并确保您的使用方式

    是合法的。

### 检查 iwlwifi 驱动程序是否已经加载。运行以下命令：

```
lsmod | grep iwlwifi
```

`lspci -k`查看Wi-Fi 驅動能否被識別

```
wpa_passphrase YOUR_SSID YOUR_WIFI_PASSWORD > /etc/wpa_supplicant/wpa_supplicant.conf
```

```
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

```
dhclient wlan0
```

如果你在執行`dhclient wlan0` 遇到了報錯

請嘗試 安裝

```
emerge -av net-misc/dhcp
​
```

### 其他

#### `systemctl`說明

是 Systemd 中的命令行工具，用於查看和管理系統上運行的服務（service）、單元（unit）和套件（package）的狀態。

以下是一些常見的 `systemctl` 命令：

* `systemctl status <service>`：查看服務狀態。
* `systemctl start <service>`：啟動服務。
* `systemctl stop <service>`：停止服務。
* `systemctl restart <service>`：重啟服務。
* `systemctl enable <service>`：設置服務在開機時自動啟動。
* `systemctl disable <service>`：設置服務在開機時不自動啟動。

在 `systemctl` 命令後加上 `-a` 選項可以列出所有服務的狀態，加上 `--failed` 選項可以列出失敗的服務。例如：

* `systemctl -a`：列出所有服務的狀態。
* `systemctl --failed`：列出失敗的服務。

#### `resolv.conf` 說明

以下是一些在 `resolv.conf` 文件中常見的設置示例：

```
​
nameserver 8.8.8.8
```

這行設置將使用谷歌的 DNS 伺服器（IP 位址為 8.8.8.8）來查詢網址。

```
​
nameserver 8.8.4.4
```

這行設置將使用谷歌的第二個 DNS 伺服器（IP 位址為 8.8.4.4）來查詢網址。

```
​
domain example.com
```

這行設置指定了本地網路的預設 DNS 域名，也就是如果本地查詢的網址沒有指定域名，就會自動添加該域名。

```
​
search example.com
```

這行設置與 `domain` 相似，但是可以指定多個 DNS 域名，如果本地查詢的網址沒有指定域名，就會按照指定的域名順序逐一嘗試查詢。

```
​
options rotate
```

這行設置會告訴本地 DNS 解析器在每次查詢網址時，輪流使用每個 DNS 伺服器，而不是一直使用同一個 DNS 伺服器，這可以平衡不同 DNS 伺服器的負載，提高解析效率。

```
​
options timeout:2 attempts:3
```

這行設置會告訴本地 DNS 解析器在查詢網址時，如果一個 DNS 伺服器沒有回應，就在 2 秒內等待該伺服器回應，如果等待超時，就嘗試最多 3 次查詢，然後再切換到下一個 DNS 伺服器嘗試查詢。這樣可以避免等待超時造成的查詢延遲，提高解析效率。

### 引用來源

1. [官方](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base/zh-cn)wiki安裝文檔
2. 中文版較權威[文檔 ](https://bitbili.net/gentoo-linux-installation-and-usage-tutorial.html)
3. Gtrush的[教程](https://gtrush.com/2022/06/12/%E6%96%B0%E6%89%8BGentoo%E6%8A%98%E8%85%BE%E8%AE%B0%E5%BD%951-%E5%AE%89%E8%A3%85%E7%AF%87-%E4%BA%8C%E8%BF%9B%E5%88%B6kernel%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95/)
4. chatGPT & Google

### 寫在最後

首先，我希望你可以在觀看別人教程視頻的時候，我的這點微不足道的經驗可以為你理清思路，同時也希望你可以早日體驗到gentoo的樂趣（壞笑）。如果能夠幫到你就太好了！

日後我或許會補充一些其他編譯的踩坑經驗。

對gentoo有興趣的小夥伴可以加入[tg-group](https://t.me/gentoo\_zh\_offtopic)

我如果有錯的表述，歡迎聯繫我與我交流，虛心請大佬指教。

當然，如果您對這種滾動式更新及編譯式安裝我更建議你使用Archlinux，省心又省電，重要你的身體不會因此高負荷感到不適。
