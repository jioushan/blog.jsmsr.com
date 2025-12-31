# 裝eve-ng社區版踩個小坑

首先官網的意思說

[https://www.eve-ng.net/index.php/community/](https://www.eve-ng.net/index.php/community/)

21.05.2022年之後發行的eve5版本，首先不能從ubuntu16直接升級，已經不兼容了。

（為什麼官方不用Debian呢，惱）

截止到我當下在官方哪裡下載的版本，如果你在它安裝過程中網路不暢，你國網路堪憂。非常大可能安裝失敗。

開機呢，腳本初始化也失敗，

root/eve 這默認帳戶密碼登不進去w？怎麼辦

重啟shift 在grub的界面按e編輯，進`單用戶模式`，具體自行搜。{`ctrl+x` 保存退出}

通過單用戶模式，重置root密碼，`passwd`

最後的最後，正常啟動，進入系統，登進去之後

然後

```
cd /etc
```

目錄下

```
./eve-setup.sh
```

繼續執行eve安裝咯。

eve官方[安裝手冊](https://www.eve-ng.net/index.php/documentation/community-cookbook/)

順便補倆 鼓搗pve 能幫到你的url \
pve-sant[教程](https://foxi.buduanwang.vip/virtualization/pve/2787.html/)

pve 中文 [手冊](https://pve-doc-cn.readthedocs.io/zh_CN/latest/index.html)<br>
