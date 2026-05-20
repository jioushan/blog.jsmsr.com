---
description: On Debian13 BBR3 installed.;)
---

# BBR3

本篇內容文件採用 [xanmod](https://xanmod.org/),如果你還不知道它是什麼 請閱讀它們的官網,\
簡而言之,如果你追求新特性,更強的針對CPU架構特殊優化的內核,那麼xanmod是你一定不可錯過的內核項目之一。

1, 下載內核

```
wget "https://sourceforge.net/projects/xanmod/files/releases/main/7.0.9-xanmod1/7.0.9-x64v2-xanmod1/linux-image-7.0.9-x64v2-xanmod1_7.0.9-x64v2-xanmod1-0~20260517.ga456799_amd64.deb"
```

2,下載內核頭

```
wget "https://sourceforge.net/projects/xanmod/files/releases/main/7.0.9-xanmod1/7.0.9-x64v2-xanmod1/linux-headers-7.0.9-x64v2-xanmod1_7.0.9-x64v2-xanmod1-0~20260517.ga456799_amd64.deb"
```

3.安裝內核

```
dpkg -i linux-image-7.0.9-x64v2-xanmod1_*.deb linux-headers-7.0.9-x64v2-xanmod1_*.deb
```

4.更新grub引導

```
update-grub
```

5. 重啟後：

```
reboot
```

確認內核

```
uname -r
​
#7.0.9-x64v2-xanmod1
```

啟用 BBR

```
echo "net.core.default_qdisc=fq" > /etc/sysctl.d/99-bbr.conf
​
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.d/99-bbr.conf
​
sysctl -p /etc/sysctl.d/99-bbr.conf
```

驗證

```
sysctl net.ipv4.tcp_congestion_control
```

結果

```
root@debian ~ # sysctl -p /etc/sysctl.d/99-bbr.conf
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
root@debian ~ # sysctl net.ipv4.tcp_congestion_control
net.ipv4.tcp_congestion_control = bbr
root@debian ~ # 
```

水一篇博文,BBR2 VS BBR3 測試的話看心情發.

{% embed url="https://embed.music.apple.com/jp/album/iris-out-single/1837658528" %}
