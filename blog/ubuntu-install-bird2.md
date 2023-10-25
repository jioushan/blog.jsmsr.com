# 😄 Ubuntu Install Bird2

事情是这样的，介于我一直想升级bird2，加上一次重置bird配置的时候，birdc 的各种命令反复报错。

Unable to connect to server control socket (/run/bird/bird6.ctl): Connection refused



在备份了config后开始了万能解决大法reboot

真正的解决方式在 文末！（要感谢haimai的帮助）



这里的水一篇update bird2 安装



默认bird安装是1.5

```
apt-get install bird
```

经过各种爬坑，写一手避免反复踩坑案！

当然你想卸载 remove bird&#x20;

在我Reboot 机器之后！



我遇到了 dns无，以至于访问任何域名hosts报错

ubuntu 16/Debian （ubuntu 16以上的不适用）

```
vi /etc/resolv.conf
```

你可以顺手装个vim ，nano我是用不习惯！



我们看到官方的一个安装教程 [链接](https://bird.network.cz/pipermail/bird-users/2021-April/015401.html)

重点在于我们要添加一个私人源

```
sudo add-apt-repository ppa:cz.nic-labs/bird-experimental
```

这一步 sudo add-apt 就先给我报错。

如果您的原因是因为修改了主机名 而hosts没有指向主机名是会被无情 报错的

`vim /etc/hosts`\
编辑它 把127.0.0.1 指向我们的主机名

然后你又遇到sudo: add-apt-repository: command not found 报错

解决的方法是安装software-properties-common。输入命令：

```
apt-get install software-properties-common
```

然后最后你就可以按照官方的步骤 顺利添加源

小提示，记得 `apt-get update`&#x20;

不同步的化可能仍然找不到包哟！

```
sudo add-apt-repository ppa:cz.nic-labs/bird-experimental
sudo apt-get update
sudo apt install bird2
sudo systemctl status bird
bird --version
dpkg -l bird2
```



最终感谢海马的帮忙，排查错误，是bird没启动。

```
systemctl stop bird  
systemctl status bird
systemctl start bird
```

```
service bird6 restart
```

最后的最后 如果如果您也遇到和我一样的烦恼，愿此文能够帮到你！
