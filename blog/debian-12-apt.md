# Debian 12 apt

若你是 直接 安裝的 Debian12 的話 它的新的 apt源 配置文件 稍微有些不一樣。



首先 我們進入 它的路徑

```
cd /etc/apt/sources.list.d
```

目錄下有 `debian.sources` 這樣的一個文件。

我們打開 並觀察它。

```
Types: deb deb-src
URIs: mirror+file:///etc/apt/mirrors/debian.list
Suites: bookworm bookworm-updates bookworm-backports
Components: main

Types: deb deb-src
URIs: mirror+file:///etc/apt/mirrors/debian-security.list
Suites: bookworm-security
Components: main
```

我們 重點 關注 URL 這個路徑，所以我們要切換到 `/etc/apt/mirrors` 路徑下。

然後 修改 它們的兩個文件。\
官方源

目錄下有 `debian.sources` 這樣的一個文件。

```
https://deb.debian.org/debian-security

```

```
https://deb.debian.org/debian

```

\
然後執行 `apt update` 即可 更新 我們的軟件源。

