# Some Email Services Guide

Sendgaid conf 模版

```
notifier:
disable_startup_check: false
smtp:
  address: "submission://smtp.sendgrid.net:587" #发信domian&port
  timeout: 15s
  username: "apikey" #通常默认账户为apikey
  password: "SG.**************"       # 3. 換成剛才獲取的「授權碼」
  sender: "noreply <noreply@exm.com>" # 4. 發件人顯示名稱
  identifier: localhost
  subject: "[Authelia] {title}"
  startup_check_address: "admin@exm.com" # 啟動時發送測試郵件的目標（可選）
  tls:
    server_name: "smtp.sendgrid.net"      # 5. 必須與上面的 address 域名一致
    skip_verify: false
    minimum_version: "TLS1.2" #开启TLS,有些发信应当设置为STARTTLS摸索
```

AWS SES conf SMTP 模版

```
notifier:
disable_startup_check: false
smtp:
  address: "smtp://email-smtp.[region].amazonaws.com:587"
  timeout: 15s
  username: "AK**************" #这里的账户&密码并不是IAM的账户和key而是来自SES STMP 的IAM建立账户时的账户和密码
  password: "******************"       # 3. 換成剛才獲取的「授權碼」
  sender: "noreply <noreply@exm.com>" # 4. 發件人顯示名稱
  identifier: localhost
  subject: "[Authelia] {title}"
  startup_check_address: "admin@exm.com" # 啟動時發送測試郵件的目標（可選）
  tls:
    server_name: "email-smtp.[region].amazonaws.com"      # 5. 必須與上面的 address 域名一致
    skip_verify: false
    minimum_version: "TLS1.2"
```

\
<br>
