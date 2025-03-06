---
description: 通过acme.sh选择Google_CA签发证书
---

# ACME.SH签发Google SSL

### 1.在GCP Shell启用账户的`API`

```
 gcloud beta publicca external-account-keys create
```

### 2.`ACME.SH` install

```
curl https://get.acme.sh | sh -s email=your email address
```

`your emaill address`替换成你的email

### 3.(可选)acme.sh写入`PATH`

```
echo 'export PATH="$HOME/.acme.sh:$PATH"' >> ~/.bashrc
```

应用`bashrc`文件

```
source ~/.bashrc
```

### 4.CA设置为`Google`的server

```
acme.sh--set-default-ca --server google
```

### 5.准备工作1

```
acme.sh --register-account -m 刚刚申请key的谷歌账号邮箱 --server google \
        --eab-kid xxxxxx \
        --eab-hmac-key xxxxxxxx
```

将获取的kid和key应用进去以及Google账户的gmail地址

### 6.准备工作2

我们假设您使用的Cloudflare管理您的Domain的Dns解析

请登陆Cloudflare的控制台创建您的API Token,

```
export CF_Token="xxxxxxxxxxx"
export CF_Account_ID="xxxxxxxxxxxxx"
```

也请将您的`Token`和`Account_ID`导入Shell 7.签发证书

```
acme.sh --issue --dns dns_cf -d example.com -d *.example.com
```

`example.com`和`*.example.com`

还请换成自己的Domain和要签发的子域名

### 7.获取签发证书

```
acme.sh --install-cert -d example.com \
--key-file       /your_path/example.key  \
--fullchain-file /your_path/example.crt
```

自行替换`your_path`文件路径,字段

`example.com`欲要申请的Domain和要签发的子域名

### 8.（可选）若遇服务商要求提供CA证书可在签发路径下通过`tail`截取CA片段,这里我输出为`ca-cert.crt`文件

```
tail -n +$(grep -n "END CERTIFICATE" www.example.com.crt | head -1 | cut -d: -f1) www.example.com.crt > ca-cert.crt
```

#### &#x20;Music

