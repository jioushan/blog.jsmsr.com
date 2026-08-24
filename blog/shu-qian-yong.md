# お気に入り

[https://tld-list.com](https://tld-list.com)  此網站幫助您比對部分Domain的價格 在新註和續費直接找的一個合適的價格.

[https://jemnetworks.com/ipv6ptr](https://jemnetworks.com/ipv6ptr) 此網站及部分工具幫助您計算ipv6的PTR便於您在Bind還是添加PTR

指針時填計算出正確的值

[https://zenn.dev/tacchan5424/books/22d87ed6bc8550/viewer/aabd31](https://zenn.dev/tacchan5424/books/22d87ed6bc8550/viewer/aabd31)  TailwindCSS的Ja教學

## [ピザポテト](https://www.calbee.co.jp/products/detail/?p=20210513141859) ポテト種類の一つ

On the Bilibiliに[Rust 勉強課程](https://www.bilibili.com/video/BV1ePS4YfE3k/?from_spmid=united.player-video-detail.drama-float.0\&plat_id=411\&share_from=season\&share_medium=iphone\&share_plat=ios\&share_session_id=C8CC4604-FEE3-4E8E-BDA8-32F042292272\&share_source=COPY\&share_tag=s_i\&spmid=united.player-video-detail.0.0\&timestamp=1740317610\&unique_k=bCo2OtB\&vd_source=203389464697bae72d71b8214ad1912f\&spm_id_from=333.788.player.switch)

毎週間に必要読サイト　[https://ruanyifeng.com/blog/](https://ruanyifeng.com/blog/)

```
journalctl --vacuum-size=10M
```

```
du -ahx / | sort -rh | head -n 20
```

!!!

```
apt purge $(dpkg -l | awk '/linu[x]-image-[0-9]/ {if ($3 != "'$(uname -r | sed "s/-.*//")'") print $2}')
```

```
# 1. 更新套件清單並重新安裝核心
apt update && apt install -y linux-image-amd64
​
# 2. 更新 GRUB 引導选單
update-grub
```

```
sed -i 's/#SystemMaxUse=/SystemMaxUse=10M/' /etc/systemd/journald.conf
systemctl restart systemd-journald
```

```
apt clean && apt autoremove --purge -y
journalctl --vacuum-size=10M
```

```
# 1. 設定 IP 與子網遮罩，並啟用網卡
sudo ifconfig eth0 192.168.10.100 netmask 255.255.255.0 up
​
# 2. 設定預設網關
sudo route add default gw 192.168.10.1 eth0
​
# 3. 設定 DNS 解析
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

\
<br>
