---
description: Raspberry
---

# 树莓派的坑！玩pi必知！

Ubuntu的用法， debian 显示真的简陋到丑。这不重要。熟悉vim编辑器。Nono记下两个命令就好了！&#x20;

Ctrl+O 保存。Enter确认 Ctrl+x退出。如果直接退出（前提修改内容了）也会询问你要不要保存。Enter确认就好了。&#x20;

## 切换到su root 管理员模式！&#x20;

```
sudo passwd root
 执行此命令后系统会提示输入两遍的root密码（用来确保你记住了密码）。 
sudo passwd --unlock root
```

输入密码解锁root用户&#x20;

&#x20;用下面命令切换到root管理员&#x20;

## **树莓派换源！！！（注意坑）**&#x20;

```
su root
```

中科大的源应该要好很多。当然你在国外，哪有换源这毛病。也不会这么毛病！&#x20;

这个帖子是换[中科大的源](https://www.jianshu.com/p/768f0181672b)的方式！按照这个换源就好！！！（适用于树莓派3B）&#x20;

但是注意千万不要&#x20;

## 这是**重启**的命令。

这里的坑的问题就在这你换了源。树莓派**重启失败**。一个红灯常亮。绿灯不闪了。这是系统都不读了啊。这就是毛病。直到我后来找到了问题原因！&#x20;

```
sudo reboot
```

&#x20;在换源的时候不要直接upgrade，加上dist-upgrade就可以了，直接upgrade好像会使pi不能boot，你会发现绿灯规律的闪烁四下。&#x20;

刷好系统后修改了源直接upgrade然后重启，大概率是无法重启的 ！！！&#x20;

我的天，我写入了N次镜像了。又要重新烧录！之前是数据库没法安装，UbuntuMate烧录进去自家电视怎么改就是不显示画面这咋完成初始设置！&#x20;

官方这 debian 坑有不少。之前换回树莓派官方源装数据库能找到。但是那个速率我卸了的想法，太真实了。&#x20;

## **关于树莓派换国内源。因为系统源不同，造成无法下载安装软件冲突！**&#x20;

（适用于树莓派4B）（[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)）&#x20;

将stretch改成buster\
即：deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui&#x20;

```
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui deb-src http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
```

当我再踩到坑我还会回来的！！！继续补的！&#x20;
