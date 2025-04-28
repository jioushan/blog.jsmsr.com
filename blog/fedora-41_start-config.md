---
description: 写点开始使用Fedora 41自己使用时的配置
---

# Fedora 41\_start config

写在前面的话,如果你曾是一个名`Debian`系用户,这次的折腾,蛮使人花费些时间.

尽管绝大多数Linux主流发行版本都适用`systemed`来管理进程.`Gentoo`这种奇葩喜欢搞一个`OpenRc`.

今天我们来说说从我昨日夜,install `Fedora`谈起.

有一些厂商会提供Fedora最新的system,我用的这位主机商,它还停留在fedora34.

截止我在写这篇文时,官方最新的支持是fedora41

## 1.DD系统

如果我们遇到我们希望install 的system,我们所采用的云厂商并未有提供,我们已知采用QEMU虚拟化.

我推荐这个项目

```
https://github.com/bin456789/reinstall
```

您可以更简单的在您云服务为您提供的系统上

```
curl -O https://raw.githubusercontent.com/bin456789/reinstall/main/reinstall.sh || wget -O reinstall.sh $_
```

```
chmod + x reinstall.sh
```

```
bash reinstall.sh fedora
```

于是我顺利的安装了Fedora Cloud Edition 版本

## 2.包管理器

在Fedora上包管理器上`dnf`,请忘记Debian系的`apt`

当我一把梭 `dnf install vim mtr htop -y` 时,些许一些软件它们在安装后并不会保持开启自动启动.

```
systemctl enable <init>
```

自行替换`<init>`字段,如果你不知道如何通过system创建/管理软件进程.这是你应该花点时间去了解.在这里不多做赘述.

## 3.网络配置

或许至今我都觉得在Debian上的`ifupdown`真的很方便,写入`interfaces.d`路径下的网卡配置文件.

在`fedora`环境下默认采用`NetworkManager`来管理网卡设备.

由于是Cloud系列的版本,系统默认是通过`Cloud-init`来自行导入下发配置的.

我们需要找到`cloud-init`的配置

```
 vim /etc/cloud/cloud.cfg
```

寻找`network`字段,变更配置为 关闭\<disabled>

```
network:
  config: disabled
```

此处不再过多赘述，如何使用`vim` or `nano`,请自行选择您擅长的编辑器.

重启`cloud-init` 进程

```
systemctl restart cloud-init
```

让我们进入`NetworkManager`网卡路径下

```
cd /etc/NetworkManager/system-connections
```

我们可以查看`cloud-init`为我们创建的网络配置信息,如您对`yaml`类似的格式语言陌生,可以适当熟悉下.

```
nmcli con add type ethernet con-name ens18 ifname ens18 ipv4.method manual ipv4.addresses 192.168.0.2/24 ipv4.gateway 192.168.0.1 ipv4.dns "8.8.8.8 8.8.4.4"
```

我这里的以太网卡是`ens18`,地址是`192.168.0.2/24`,`gateway` `192.168.0.1`还请根据自己实际情况进行操作.

Tips: 若您要添加ipv6相关,您可以自行修改字段ipv4为ipv6,设置您的address,netmask,gateway,我不认为dns是必要字段.

您可以通过 `ip a`来查看,

谨慎重启`NetworkManager` 进程

```
systemctl restart NetworkManager
```

当您处理完基础的以太网卡配置信息,关闭cloud-init的开机自启动和进程.

```
systemctl disable cloud-init
```

```
systemctl stop cloud-init
```

### 3.1`ip tunnel`的创建

默认它没有开启gre内核,我们可以先执行是否支持.

```
lsmod | grep gre
```

手动临时加载,

```
modprobe ip_gre
```

为了确保模块在系统启动时自动加载，可以将其添加到 `/etc/modules-load.d/` 目录下的配置文件中：

```
echo "ip_gre" | tee /etc/modules-load.d/ip_gre.conf
```

使用`nmcli`创建tunnel   !!自行替換gre1名稱為你的接口名稱

```
nmcli connection add type ip-tunnel ifname gre1 mode gre con-name gre1 local 10.0.0.1 remote 10.0.0.2
```

```
nmcli connection modify gre1 ipv6.method manual ipv6.addresses fe80::a::1/64
```

```
nmcli connection modify gre1 ip-tunnel.ttl 255
```

请自行替换`gre1`相关字段为您要创建的gre tunnel接口名称,以及local和remote地址.

```
ip link set gre1 up
```

如果遇到nmcil低版本/高版本命令嚴格，创建配置时tunnel的ttl 報錯

我们可以手动执行

```
ip tunnel change gre1 ttl 255
```

请确保gre tunnel类似协议接口两侧的借口都要建立,才可以建立通信.

nmcil 自動啟用接口

```
nmcli connection modify gre1 connection.autoconnect yes
```

### 3.2 创建dummy接口.

```
nmcli connection modify dummy* ipv6.method manual
```

```
nmcli connection modify dummy* ipv6.method manual ipv6.addresses fe80::a:1:2/128
```

```
nmcli connection up dummy*
```

## 4.DNS配置

```
vim /etc/resolv.conf 
```

长话短说,

```
nameserver 1.1.1.1
```

```
nameserver 2606:4700:4700::1111
```



{% hint style="info" %}
\*.Warnning:此教程会随着我长期使用适当的补充及更新.以及若您发现我哪里的有错误,本人笨拙,还请指出,请赐教！
{% endhint %}



结尾的音乐分享,

{% embed url="https://open.spotify.com/intl-ja/track/7rC3P7tpWriaC4hYWKwGQd?si=a2f8e8dca6c54063" %}
Hey,Lover-Wabie
{% endembed %}



{% embed url="https://open.spotify.com/intl-ja/track/5dOaDkbqvfHnhRbDTL3I2W?si=fb1cc66df5b44938" %}
Memory-Last Goodbye
{% endembed %}



{% embed url="https://open.spotify.com/intl-ja/track/3se9YurPfXqDzGsjURs3hZ?si=ec88e90d0a084d83" %}
Bloody Mary Girl-She Her Hers
{% endembed %}

