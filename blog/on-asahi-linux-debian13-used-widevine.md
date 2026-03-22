# On Asahi Linux Debian13 used Widevine

俗話說，一臺工作PC若是連播放音樂和視頻都無法做到，未免也太可憐些！

這裡只是一個思路解決在16k aarh64 M1 Soc的Debian上面解決DRM播放問題，

直到目前我沒有很好的解決方案，大抵是Debian下面的chromium在打包的時候，無論你如何使用官方的repo [https://github.com/AsahiLinux/widevine-installer/issues](https://github.com/AsahiLinux/widevine-installer/issues)

去嘗試解決此問題，無終後，由於Disk屬實有限，沒能力自己rebuild chromium,不得已下自己採用podman運行Fedora39鏡像運行chromium和widevine修補得以解決DRM播放問題，

```
sudo podman run --rm -it \
--privileged \
--tmpfs /tmp \
--tmpfs /var/tmp \
--tmpfs /root \
--tmpfs /home \
-e DISPLAY=$DISPLAY \
-e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
-e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
-v /run/user/$(id -u):/run/user/$(id -u) \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v /usr/share/fonts:/usr/share/fonts \
--device /dev/dri \
fedora:39 \
bash
```

```
dnf install -y chromium widevine-installer google-noto-sans-cjk-fonts
widevine-installer
chromium-browser \
  --no-sandbox \
  --ozone-platform=wayland \
  --disable-gpu \
  --disable-dev-shm-usage \
  --user-data-dir=/tmp/chrome
```

這裡我添加了中文和日文字體支持以便瀏覽器字體不顯示問題, 官方的widevine修補程式需要您在安裝過程中按下兩次Enter確認後方可下載, 由於磁盤空間有限故每次打開都要rm -rf此鏡像 可以自己做長期持久化

目前我沒有特別好的思路，若有更好的方式此blog此篇文章會得以支持更新。

順帶一提如您可以透過waydriod運行Android15以上版本的16k鏡像您可以可以通過Google Play商店安裝您在智能手機上面所需的軟體從而避開widevine不支持造成的Spotify/AP music/Neflix之類的串流媒體無法正常運作。

以上皆Arhc64 16k芯片在Delian系 linux上述遇到的問題，如您使用Fedora應該無此影響。

<br>
