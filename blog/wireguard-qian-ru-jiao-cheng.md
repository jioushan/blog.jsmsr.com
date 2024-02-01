---
description: 服务端到另一台服務端/客戶端
---

# 🌏 Wireguard 淺入教程

這是一個我自己都難以詮釋的教程！如有疑問，請私訊，或者我寫的有錯誤，歡迎指正。

聯絡我的方式 請在側欄 找到 About-Me

### 端到端 建立Wireguard（以下本文簡稱為wg

鳴謝教程來源 ZH，感謝曾過去幫助我配置wg，所得此教程。

系統環境Debian11

wg依賴內核，若內核較低 無法使用wg，本篇不在此 過多解釋 內核升級

### Start

安装Wireguard  Debian 11

```
apt install wireguard-dkms wireguard-tools -y
```

Debian 12

```
apt install wireguard
```

進入  `/etc/wireguard` 目錄

生成公钥，秘钥

```
wg genkey | tee privatekey | wg pubkey > publickey 
```

給予此目錄777 權限

```
chmod 777 -R /etc/wireguard
```

利用 touch 創建一個網卡配置

```
touch xxx.conf
```

启动此网卡配置命令

```
wg-quick up xxx.conf
```

结束就是将`up` 替换为 `down`

### 网卡配置详细

以下为服务器A 配置&#x20;

```
[Interface]
PrivateKey = #[此處填寫服務器A生成的 密鑰 PrivateKey]
Address =  #[此處填寫服務器A ifup link local address]
ListenPort = #[此處填寫服務器B 對端的端口]
table = off


[Peer]
PublicKey = #[此處填寫服務器B端的公鑰 PublicKey]
AllowedIPs = ::/0
EndPoint = [255.255.255.255]:00000 #[此處填寫服務器b端的ip/域名：端口]
PersistentKeepalive = 25 #[保活，默認為25]
```

以下為服務器b端配置

```
[Interface]
PrivateKey = #[此處填寫服務器B生成的 密鑰 PrivateKey]
Address =  #[此處填寫服務器B ifup link local address]
ListenPort = #[此處填寫服務器A 對端的端口]
table = off


[Peer]
PublicKey = #[此處填寫服務器A端的公鑰 PublicKey]
AllowedIPs = ::/0
EndPoint = [255.255.255.255]:00000 #[此處填寫服務器A端的ip/域名：端口]
PersistentKeepalive = 25 #[保活，默認為25]
```

至此，您鍵入 wg-quick up xxx.conf

將兩個網卡啟動up上，可通過mtr 等工具 來ping對端的Address 若通，即您的wg tunnel已經建立！



設置開啟自起動 對應網卡

```
systemctl enable wg-quick@xxx
```

您可以通過此命令 來檢測您的wg tunnel 狀態&#x20;

```
wg show
```

### &#x20;一次bug反馈

故事是这样的，我于今日在某一台地域配置wg的时候，up网卡时发生了

`RTNETLINK answers: Operation not supported`&#x20;

这样的报错。

您需要做的就是键入以下命令 检查您的内核&#x20;

```
modprobe wireguard
```

我这边显示的`4.19.0.-22` 所以我键入了以下命令&#x20;

```
apt install linux-headers-4.19.0-22-amd 
```

若您的版本与此不同 自行修改，然后在wg-quick up 网卡就解决此问题！

### 服務端到客戶端

以下為客戶端配置

```
[Interface]
PrivateKey = #[此處填寫服務器B生成的 密鑰 PrivateKey]
Address =  #[此處填寫服務器B ifup link local address]
table = off


[Peer]
PublicKey = #[此處填寫服務器A端的公鑰 PublicKey]
AllowedIPs = ::/0
EndPoint = [255.255.255.255]:00000 #[此處填寫服務器A端的ip/域名：端口]
PersistentKeepalive = 25 #[保活，默認為25]
```

而服務端可以不填寫 對端客戶端端 Endpoint

注意區分 公玥和私玥



教程到此结束，如有其他疑问，可询问其他大佬，也欢迎大佬来给咱指正！



