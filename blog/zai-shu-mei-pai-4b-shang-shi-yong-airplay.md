---
description: 愉快的使用树莓派将IOS设备音频投发至您的音响
---

# 在树莓派4B上使用Airplay

#### 基于Raspberry 4B 我们需要聊一聊这个奇怪的机型！

### 似乎树莓派大部分官网的机型都可以运行Debian11 64位的固件

而4B 真的有点怪 官网的维护可以说这很拖

若您需要固件刷写！

[https://downloads.raspberrypi.org/raspios\_oldstable\_lite\_armhf/images/raspios\_oldstable\_lite\_armhf-2022-04-07/2022-04-04-raspios-buster-armhf-lite.img.xz](https://downloads.raspberrypi.org/raspios\_oldstable\_lite\_armhf/images/raspios\_oldstable\_lite\_armhf-2022-04-07/2022-04-04-raspios-buster-armhf-lite.img.xz)

请下载此固件，建议使用官方的树莓派刷写工具，比起其他市面推广的刷写工具兼容性较强！且在刷写之前就可编辑好 wifi，登录账户密码 等设定！

**Tips：若您在CN大陆，建议尝试清华源！具体更换源教程 不在多阐述！**

[https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)

自行访问 ！注意请选择Debian10 或自行uname -m 检查！选择正确的源！否则apt-get update 后会造成 系统重启后 无法正常载入!

### \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*截止到2022/年8月 发现

接下来 我们来聊一聊 在Debian 上 安装 shairport-sync 实现airplay（即apple 设备可以通过此包将音频通过WiFi播放至您的raspberry，您可以通过3.5mm耳机接口将音响等设备连接您的树莓派

\*\*\*\*\*\*\*\*\*\*警告\*\*\*\*\*\*\*\*\*\*\*默认我的代码都在root用户下操作！若您在其他用户下 请在命令前加上sudo

`apt install shairport-sync`

`service shairport-sync start`

您就可以愉快的通过IOS的设备将音频通过您的树莓派播放出来！

### 如何控制树莓派自己的音量！

`alsamixer`

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*若通过apt安装的包具有问题，官方建议通过

### 手动编译安装！

**Tips：首先若您在CN，由于您的ISP运营商缘由，请自行修改hosts 将正确的GitHub解析地址 填入您的派中！否则通过访问GitHub将出现困难！**

您需要安装以下依赖固件包 以便于 make 编译

`apt install --no-install-recommends build-essential git xmltoman autoconf automake libtool libpopt-dev libconfig-dev libasound2-dev avahi-daemon libavahi-client-dev libssl-dev libsoxr-dev`

通过git clone 官方的最新版本源码

`git clone https://github.com/mikebrady/shairport-sync.git 进入目录 cd shairport-sync 执行以下命令 autoreconf -fi`

`./configure --sysconfdir=/etc --with-alsa --with-soxr --with-avahi --with-ssl=openssl --with-systemd`

`make`

### 请有耐心的等待编译完成！

最终 `make install`

将此包添加至系统启动时 自动启动

`systemctl enable shairport-sync`

_**至此结束**_\*\*\*

在树莓派上搭建DLNA服务 我们以后再讲！留坑！
