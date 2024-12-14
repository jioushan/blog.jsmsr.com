---
description: 截止我在写这篇文章部署Zabbix的时候7.2版本尽管已经正式发布了，但是Grafana还没有支持它.
---

# On The Debian12 amd64 install Zabbix + Grafana

截止我在写这篇文章部署Zabbix的时候7.2版本尽管已经正式发布了，但是Grafana还没有支持它.

#### 于是这便让我在安装的时候造成了困扰.

## Zabbix Start/

环境, Zabbix / Mysql （else Datebase）/Nginx

不过这里我没有去配置nginx，关于nginx的教程还烦请自行解决.

#### a.Zabbix install 流程

按照[官方的手册](https://www.zabbix.com/cn/download?zabbix=6.4\&os_distribution=debian\&os_version=12\&components=server_frontend_agent\&db=mysql\&ws=nginx)进行安装即可

**Download Zabbix repository**

[产品手册](https://www.zabbix.com/documentation/6.4/manual/installation/install_from_packages)

```
# wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.4+debian12_all.deb
# dpkg -i zabbix-release_latest_6.4+debian12_all.deb
# apt update
```

#### b. 安装Zabbix server，Web前端，agent

```
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```

**Tips:在Debian12上install Mysql 您应当先手动安装 Mysql 具体请查看 此** [**wiki.**](https://docs.vultr.com/how-to-install-mysql-on-debian-12)

#### c. 创建初始数据库

[产品手册](https://www.zabbix.com/documentation/6.4/manual/appendix/install/db_scripts)

Make sure you have database server up and running.

在数据库主机上运行以下代码.

```
# mysql -uroot -ppassword
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;
```

**至此您将创建了zabbix 数据库 数据表包括password请自行修改.**

导入初始架构和数据,系统将提示您输入新创建的密码.

```
# zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

请不要着急,如果您的本地环境很干净的情况下,通常并不会遇到我这些头疼的问题.

**按照Zabbix的文档 下一步是解压`zabbix-sql-scripts/mysql/server.sql.gz`导入zabbix数据库表,对吧.**

若您曾经install zabbix 抑或着 遇到过不明未知现象,

`/usr/share/zabbix-sql-scripts/mysql/`路径下找不到`server.sql.gz` 文件

哪怕是 `apt reinstall zabbix-sql-scripts`依然提示找不到`server.sql.gz`文件,不导入数据表 继续下去的话，您在web install界面 同步数据库的时候将会遇到报错.

如果解决呢？首先让我们回到Zabbix的官网,找到 [source code 版本](https://www.zabbix.com/cn/download_sources),下载源包到server或者本地也好.

源码下载后解压，在database文件中找到mysql文件夹，将其中的 data.sql、images.sql、schema.sql 手动导入到数据库中

![img](https://img2024.cnblogs.com/blog/2034475/202403/2034475-20240304160048758-1448864043.png)

sftp上传到server上面任意路径下，我们执行导入数据库 命令。

mysql数据库手动导入，导入的顺序：schema.sql > images.sql > data.sql

```
mysql -u root -p -D zabbix < schema.sql        
mysql -u root -p -D zabbix < images.sql
mysql -u root -p -D zabbix < data.sql
```

5、验证数据库导入是否成功，zabbix数据库需要至少有173张表才算导入成功

```
mysql -u root -p
show databases;
use zabbix;
show tables;
```

![img](https://img2024.cnblogs.com/blog/2034475/202403/2034475-20240304160659005-706546695.png)

确认您导入后至少有173表以上。

至此数据库导入这一项我们完成。

让我们继续接下来的Zabbix安装,

#### d. 为Zabbix server配置数据库

编辑配置文件 `/etc/zabbix/zabbix_server.conf`

```
DBPassword=password
```

请利用vim或者其他IDE找到/DBPassword 取消注释修改为zabbix数据库密码

#### e. 为Zabbix前端配置PHP

编辑配置文件 `/etc/zabbix/nginx.conf` uncomment and set 'listen' and 'server\_name' directives.

```
# listen 8080; #自行web 修改端口
# server_name example.com; #绑定域名
```

务必在域名服务商解析server IP 以便于后续访问成功;

#### f. 启动Zabbix server和agent进程

启动Zabbix server和agent进程，并为它们设置开机自启：

```
# systemctl restart zabbix-server zabbix-agent nginx php8.2-fpm #重启此些项
# systemctl enable zabbix-server zabbix-agent nginx php8.2-fpm #开机自启动
```

#### 请访问您的域名:port 访问 Zabbix的web install流程，默认账户 `Admin` 密码 `zabbix`

若您希望Zabbix支持其他语言，请自行调整linux 支持语言包，勾选上对应的区域语言。安装之后，重启zabbix进程即可在web 面板选择其他语言，

#### 修改字符编码/地区语言

```
# dpkg-reconfigure locales
```

```
# systemctl restart zabbix-server zabbix-agent nginx php8.2-fpm #重启此些项
```

## Grafana Start/

install Grafana是一件极其容易的事情。

[官方文档](https://grafana.org.cn/docs/grafana/latest/setup-grafana/installation/debian/)

以下步骤以从 APT 仓库安装 Grafana

#### 安装必备软件包

```
$ apt-get install -y apt-transport-https software-properties-common wget
```

#### 导入 GPG 密钥

```
$ mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```

#### 要添加稳定版本的仓库，运行以下命令

```
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" |  tee -a /etc/apt/sources.list.d/grafana.list
```

#### 要添加测试版本的仓库，运行以下命令

```
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com beta main" |  tee -a /etc/apt/sources.list.d/grafana.list
```

#### 运行以下命令以更新可用软件包列表

```
# Updates the list of available packages
apt-get update
```

#### 要安装 Grafana OSS，运行以下命令

```
# Installs the latest OSS release:
 apt-get install grafana
```

#### 要安装 Grafana Enterprise，运行以下命令

```
# Installs the latest Enterprise release:
 apt-get install grafana-enterprise
```

**我认为 4,6,7安装属于选配，不做强求。让我们继续通过apt 完成安装即可。**

卸载Grafana请阅读上面官方手册。通过apt即可remove

**如何在Grafana面板导入Zabbix数据源,通过Api接口。**

请阅读[aliyun wiki](https://www.alibabacloud.com/help/zh/grafana/user-guide/add-and-use-a-zabbix-data-source#ba08067bbdn0v)或者其他众多wiki资料，

我们需要注意的是URL这一项的API 路径 填写。

| **参数**       | **说明**      | **示例值**                                      |
| ------------ | ----------- | -------------------------------------------- |
| **Name**     | 自定义数据源名称。   | Zabbix                                       |
| **URL**      | Zabbix服务地址。 | `https://[Zabbix Server IP]/api_jsonrpc.php` |
| **Username** | Zabbix服务账号。 | 默认：Admin                                     |
| **Password** | Zabbix服务密码。 | 默认：zabbix                                    |

注意,在未开启https的情况下您的zabbix默认应该是http请求，

遇到的麻烦,Grafana目前插件还不支持Zabbix 7.2版本，详情见[此issue](https://github.com/grafana/grafana-zabbix/issues/1914)我相信在不久之后7.2版本或许会获得支持，这便是为什么此教程是基于6.4版本install。

#### 如果您使用7.2版本zabbix install 对接Grafana一直报错

您就遇到困扰我一天此事的问题。

若您按照Grafana的提示install 插件，在zabbix插件对接地方填写无误。但是save\&test 保存测试的时候一直提示你 `Invalid request. Invalid parameter "/": unexpected parameter "auth".`

请别上火，这不是你的错，是zabbix在新版本更新了auth的路径，过去的Grafana插件目前还不兼容Zabbix 新的api.

**所以这才是我坚持写这篇 博文的结果，无论如何 希望此教程能够帮助您少走许多弯路，节省下来的时间做什么不好的。死磕报错，不断检查流程。**

## 最终祝您有一个美好的 圣诞假期。2024年圣诞假期快乐🎉

\
