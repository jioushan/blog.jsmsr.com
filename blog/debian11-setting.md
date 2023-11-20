---
description: 每次安装系统都要配置这些文件，留个存档！方便好copy！
---

# Debian11 setting

## 每当我install Debian11时总要做以下操作，不得已写份易于copy代码的文章！

### 首先是SSH

默认我是认定您在最后一步 安装的时候ssh是自己安装的，我就不再过多赘述

假设您没安装SSH 请复制这个先安装，您要是ssh都不知道为何物，或者您用的VNC，您可以忽略掉！

咱得建议是您直接关闭此页面就好，省的您浪费时间！

```
apt install openssh-server
```

所以您需要此命令 来修改并开启root登录！这很重要以下有些命令，我们需要root登录的环境下才可以更改！

```bash
nano /etc/ssh/sshd_config
```

我知道您可能有什么vi /vim的需求，但是你知道的Debian 默认是有吗？先用nano 凑和一下！请继续

寻找 以下两行 将后面的文字修改为yes

```
PermitRootLogin yes 
```

```
PasswordAuthentication yes
```

### nano的保存并退出是！以下命令！

ctrl+o「保存询问」Enter 确认保存！

ctrl+x 退出！

最后 重启SSH 服务

```
/etc/init.d/ssh restart
```

### 接下来是软件包更新！

首先，我不是不相信 系统自己带的源，但是十有八九 它一定有个毛病。

\##以下为 Debian11 官方完整源！！！！⚠️不是Debian11别用，当然你能看懂可以自己在此上面修改下也可以食用！

第一步骤

```
nano /etc/apt/sources.list
```

```
deb http://deb.debian.org/debian/ bullseye main contrib non-free
# deb-src http://deb.debian.org/debian/ bullseye main contrib non-free
deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free
# deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free

deb http://deb.debian.org/debian/ bullseye-backports main contrib non-free
# deb-src http://deb.debian.org/debian/ bullseye-backports main contrib non-free

deb http://deb.debian.org/debian-security bullseye-security main contrib non-free
# deb-src http://deb.debian.org/debian-security bullseye-security main contrib non-free
```

然后保存，您就可以apt-get update

然后不得不安装的

```
apt-get install mtr curl vim
```

### 你要是使用vim

```
apt-get install vim
```

```
:wq
```

保存退出！

### 然后是网卡修改

```
vim /etc/network/interfaces
```

```
auto lo
iface lo inet loopback
    dns-nameservers 1.1.1.1
iface lo inet6 static
    address xxx::1/128

# The primary network interface
# This is an autoconfigured IPv6 interface
auto ens18
allow-hotplug ens18
iface ens18 inet static
      address xx.xxx.xxx.xxx/24
      gateway xxx.xxx.xxx.1
iface ens18 inet6 static
      address ::0
      netmask 64
      gateway ::1
```

地址请自己填写！

### gre/ip6gre Tunnel例子

```
// GRE code

auto hk1
 iface hk1 inet6 static
  address xxx::2/64
  pre-up ip link add hk1 type gre local ip——address remote ip——address ttl 255
  post-down ip link del hk1
```

ipv6gre就是将gre更改为ipv6gre&#x20;

ip——address 请更改为您的地址/对端地址

！！！⚠️注意 tunnel 并不是一方建立就通，需要双方都要建立！

若對端使用 只是需要通過GRE tunnel做為外部ipv6使用\
請記得添加默認路由

tunnel 也不只有 GRE還有sit,vlan,等等再複雜加密既 VPN這類，gateVPN，openvpn，wireguard等等



```
ip -6 route add default via (gateway address)

```



### 修改字符编码/地区语言

```
dpkg-reconfigure locales
```

选择合适的地区，为了减少报错易于理解，请改为 `en_US.UTF-8`

```
locale
```

检查！

### 开启内核转发

```
vim /etc/sysctl.conf
```

找寻下面两行，取消注释，或者你自己加上

```
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding=1
```

刷新并启用

```
sysctl -p /etc/sysctl.conf
```

### 添加某网卡为默认路由

```
ip -6 ro add default via {网关} dev {接口名}
```

### apt-get 切换选择ipv4/ipv6 update or update

#### apt-get使用IPv4

```
sudo apt-get -o Acquire::ForceIPv4=true install xxx
sudo apt-get -o Acquire::ForceIPv4=true update
sudo apt-get -o Acquire::ForceIPv4=true upgrade
sudo apt-get -o Acquire::ForceIPv4=true dist-upgrade
```

#### apt-get使用IPv6

```
sudo apt-get -o Acquire::ForceIPv6=true install xxx
sudo apt-get -o Acquire::ForceIPv6=true update
sudo apt-get -o Acquire::ForceIPv6=true upgrade
sudo apt-get -o Acquire::ForceIPv6=true dist-upgrade
```



{% hint style="info" %}
目前先到此为止！日后有新的会更新！
{% endhint %}

### Bird2 install debian11 bird2.0.12

```
echo "deb http://deb.debian.org/debian bullseye-backports main" >> /etc/apt/sources.list
```

```
apt update
```

```
apt install  bird2/bullseye-backports
```

移除雲計算 的自動網路配置腳本

```
apt-get remove cloud-init

```

```
apt-get purge cloud-init
```

但是請手動將`50-cloud-init` 網卡文件寫入到`interface`以免造成網卡無法啟動。

