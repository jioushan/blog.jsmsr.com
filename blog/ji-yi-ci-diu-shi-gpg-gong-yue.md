# 记一次丢失GPG公钥

记一次丢失GPG公钥，而导致ubuntu/debian 的apt频繁提示报错。



```
W: GPG error: http://security.ubuntu.com trusty-security Release: 
The following signatures couldn't be verified because the public key is not available: 
NO_PUBKEY 40976EAF437D05B5 NO_PUBKEY 3B4FE6ACC0B21F32
```

如果未修復這些錯誤，apt 在安裝或升級軟體包時會遇到問題。

apt 打包系統具有一組可信密鑰，用於確定是否可以對軟體包進行身份驗證，從而信任安裝在系統上。

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 40976EAF437D05B5
```

让我们键入这样的命令，将后面recv-keys 后面值替换为您报错的公钥值。

再次apt-get update 即可解决！
