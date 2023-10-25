---
description: root登录环境下！
---

# 😆 ssh-key来访问SSH

首先咱应该利用 Linux自带的SSH-key来生成公钥和私钥 （加密RSA）

```
ssh-keygen -t rsa
```

然后回车三次 （嗯，是回车三次

我们进入这个隐藏的目录

```
cd /root/.ssh
```

`ls`查看目录下文件

你会看到有两个文件  .pub的是公钥，自然另一个就是私钥，想办法自己下载下来私钥的内容，保存好，我们ssh登录要用到它。

键入下面命令

```
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

```
chmod 600 authorized_keys
```

然后vim编辑一下ssh\_d文件

```
vim /etc/ssh/sshd_config
```

别告诉我 你ssh还没安装（噗 (题外话\
apt-get install ssh

然后找到config下面的内容 取消注释 或者 没有内容咱就自己在最下面 加上

`RSAAuthentication yes`&#x20;

`PubkeyAuthentication yes`&#x20;

`AuthorizedKeysFile .ssh/authorized_keys`

如果你想取消root 通过密码登录，可以把 yes 改为no

`PasswordAuthentication no`

`然后键入这步命令 重启ssh 进程`

```
/etc/init.d/ssh restart
```

`「OK」`&#x20;

`状态为ok 我们就可以通过我们的私钥来登录我们的ssh会话服务器啦！`
