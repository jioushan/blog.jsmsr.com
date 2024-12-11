---
description: 部署Tiny Tiny RSS 在 servers
icon: rss
---

# Deploy Tiny tiny RSS on server

在Debian12 使用 aplan+Tiny Tiny RSS 部署

## 1.环境

创建website （nginx）和mysql 安装php8.3

## 2.下载项目

git clone

```
git clone https:*//git.tt-rss.org/fox/tt-rss.git tt-rss*
```

## 3.编辑php依赖安装问题

编辑宝塔面板安装的php路径的php安装脚本，找到php的版本号添加编译安装缺少的依赖。

```
vim /www/server/panel/install/php.sh
```

3.1找到截图下的位置，添加--enable-mbstring

![img](https://www.bt.cn/bbs/data/attachment/forum/202401/29/174731wzuynbbxzihwcbi5.png)

然后手动执行安装：

```
bash -x /www/server/panel/install/php.sh install 8.3
```

## 4.删除php函数

重新完成php8.3编译安装后，我们去aplan的php设置面板

```scss
php需要安装fileinfo扩展,并把禁用函数中的
putenv和proc_open删除

```

4.

```
把config.php-dist修改为config.php
添加数据库连接配置
putenv('TTRSS_DB_TYPE=mysql'); #数据库类型
putenv('TTRSS_DB_PORT=3306'); #数据库端口
putenv('TTRSS_DB_HOST=dbhost'); #数据库主机地址
putenv('TTRSS_DB_NAME=dbname'); #数据库名称
putenv('TTRSS_DB_USER=dbuser'); #数据库用户名
putenv('TTRSS_DB_PASS=dbpassword'); #数据库密码
putenv('TTRSS_SELF_URL_PATH=https://example.com/tt-rss'); #安装的站点域名
```

## 5.执行安装,check报错依赖

因为不能使用root执行下面语句,请指定www用户执行更新数据库

```
sudo -u www /www/server/php/83/bin/php ./update.php --update-schema
```

## 6.Done;

至此你将完成tiny tiny RSS的基础安装。

访问域名,默认账户：admin，默认密码：password

## 7.如何更新订阅源

手动执行更新RSS

在domian 的目录下执行下面命令要求php手动更新feeds

```
sudo -u www /www/server/php/83/bin/php  ./update.php --update --feeds
```

## 8.crontal -e

```
*/5 * * * * sudo -u www /www/server/php/83/bin/php /www/wwwroot/domain/update.php --feeds >> /dev/null 2>&1
```

自行替换domain位置的path路径

每五分钟定时任务执行一次rss更新

## [官方文档.](https://tt-rss.org/wiki/InstallationNotesHost/)
