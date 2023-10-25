---
description: 僅適用Linux
---

# 搭建Alist於您的服務器

#### 緣由

最近想在自己的network上運行雲盤，找了一圈，發現這個能滿足我自己的需求。

#### 使用wget（我這裡下載的是amd64架構的版本）

使用cd到您要下載install的目錄（我這裡使用的是 `/data1/www`

通過wget命令下載此VM架構的安裝包 官方[releases](https://github.com/alist-org/alist/releases)

```
wget -4 https://github.com/alist-org/alist/releases/download/v3.8.0/alist-linux-amd64.tar.gz
```

然後通過tar -zxvf 解壓到當下路徑

```
tar -zxvf alist-linux-amd64.tar.gz
```

賦予執行權限

```
chmod +x alist
```

```
./alist server
```

執行alist進程 然後您會看到 有一行 admn 的密碼 ，請務必copy，然後查詢下面的本地端口(默認端口為5244)，使用 本地/ip :5244訪問

點擊頁面下腳管理 輸入user：admin 密碼為你剛才copy的密碼

#### 所以第一件事是先修改密碼。

若您忘記密碼執行此命令。

```
./alist admin
```

#### 守護進程

```
vim /usr/lib/systemd/system/alist.service
```

vim 創建並編輯此內容

```
[Unit]
Description=alist
After=network.target
 
[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=path_alist/alist server
Restart=on-failure
 
[Install]
WantedBy=multi-user.target
```

我們將文中的

`path_alist`替換為我們剛剛下載解壓的路徑

查看當前路徑`pwd`

最後鍵入`:wq`保存並退出vim編譯器

最後我們輸入`systemctl daemon-reload`

啟動：`systemctl start alist`

停止：`systemctl stop alist`

開機啟動：`systemctl enable alist`

查看狀態：`systemctl status alist`

重啟alist進程：`systemctl restart alist`

最後您可以通過nginx反代本地端口 然後通過域名訪問！

以及如何配置存儲閱讀以下文檔/自行檢索 網絡有不少教程！

參考文檔：[https://alist.nn.ci/](https://alist.nn.ci/)
