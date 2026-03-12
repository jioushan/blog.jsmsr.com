# OpenClaw Deploy On Debian Linux

## 始める

\
最近的`OpenClaw`可谓是沸沸扬扬, 本质上这只是一个工具,却被各种商业/媒体非理性炒作,<br>

## 試験環境:

[Lightsail ](https://lightsail.aws.amazon.com/ls/webapp/home)+[Amazon Bedrock](https://ap-northeast-1.console.aws.amazon.com/bedrock/home?region=ap-northeast-1)+Litellm+OpenClaw+Telegram\_Bot<br>

在这里我用`Amazon`的[Lightsail ](https://lightsail.aws.amazon.com/ls/webapp/home)配合litellm+awscliv2(通过[Amazon Bedrock](https://ap-northeast-1.console.aws.amazon.com/bedrock/home?region=ap-northeast-1))让所谓的这只蝦運作起來。

機器配置我採用 `Lighsail`最小配置,512M雙棧網路.原則上`Amazon`的光帆是不允許長時間過高負載的,這讓我回憶起如果你用Lighsail進行iper3測速你可能會面臨boom的停機斷開ssh的事故,而再次恢復可要去web的portal自行reboot了.<br>

## 紹介: 必要な環境前に

\
言歸正傳,我是用的是向來順手的`Debian12 Linux`（ummm,lighsail沒提供Debian13,我也懶得upgrade了）主要是Debian13相比Debian12有部分軟件包不再可以尋找到,以及配置文件的路徑進行變更,

由於512M的內存在我們進行第一步,`npm build package`就給咱們內存不夠,安裝停止了

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

所以我們先不要著急執行它的安裝腳本,我們先吧內存和API在本地跑起來

### Swap<br>

先臨時給2G RAM

```
root@ip-172-26-9-118:~# fallocate -l 2G /swapfile
root@ip-172-26-9-118:~# chmod 600 /swapfile
root@ip-172-26-9-118:~# mkswap /swapfile
```

由於我們使用`Amazon Bedrock`.所以我們還要順手install下`pip`

```
apt install python3-pip -y
```

### Litellm

緊接著我們安裝`litellm`

```
pip install litellm --break-system-packages
```

我這種這種在root用戶下安裝包在下很不推薦,

### AWS IAM

由於我們使用`Amazon Bedrock`我們需要提前在我們的AWS控制台IAM-users-建立人員 隨便起個名字,密碼保存下 建議下載csv. 但是這裡的帳戶密碼並不是我們要用的，建立完成後， 讓我們 `新增許可`

```
AmazonBedrockLimitedAccess
AmazonBedrockMarketplaceAccess
```

點擊存取密鑰,創建存取密鑰，務必下載`csv文件`，這裡的ID\_Key和密鑰才是我們接下來配置存取`Amazon Bedrock`關鍵,順帶一提,令和7年註冊的AWS帳戶開通`Amazon Bedrock`需要手動填寫工單向客戶說明提高配額，否則極大概率無法使用AWS提供的`Amazon Bedrock`服務,更無法調取各種模型.

### Config awscli

讓我們繼續，由於Debian12這裡發行版的版本較長遠

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
./aws/install
```

```
root@debian:~# aws --version
aws-cli/2.34.7 Python/3.13.11 Linux/6.1.0-43-cloud-amd64 exe/x86_64.debian.12
root@debian:~# 
```

確保使用最新的版本 我們可以強制通過path來指定調用新的工具包

```
vim ~/.bashrc
export PATH="/usr/local/aws-cli/v2/current/bin:$PATH"
source ~/.bashrc
```

至此我們配置`aws configure`

```
aws configure
```

example:

```
aws_access_key_id = AKIXXXXXXXXXX
aws_secret_access_key = XXXXXXXXX
Default region name [None]: ap-northeast-1
Default output format [None]: json
```

如果這一步沒有問題,我們便可以通過

```
aws bedrock list-foundation-models --region ap-northeast-1
```

### litellm config

查看到aws的`bedrock models list`

我們在我們的root路徑下

```
vim config.yaml
```

我這裡使用的的是sonnet和haiku

```
root@Debian:~# cat config.yaml 
model_list:
- model_name: sonnet
  litellm_params:
    model: bedrock/jp.anthropic.claude-sonnet-4-5-20250929-v1:0
​
- model_name: haiku
  litellm_params:
    model: bedrock/anthropic.claude-3-haiku-20240307-v1:0
​
router_settings:
routing_strategy: simple-shuffle
fallbacks:
  - sonnet:
      - haiku
root@Debian:~# 
```

讓litellm通過配置在本地開啟一個API

```
litellm --config /root/config.yaml
```

順利的話你就可以看到

```
​
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:4000 (Press CTRL+C to quit)
```

順利的運作起來了.我們新開一個shell

```
curl http://127.0.0.1:4000/v1/chat/completions \
-H "Content-Type: application/json" \
-d '{
"model":"sonnet",
"messages":[
{"role":"user","content":"hello"}
]
}'
```

鍵入如下便可以看到順利運作的API返回的信息

❕warning

我在對接telegram bot這一步遇到了nodejs的內存不夠報錯,所提起把環境變量調整一下

```
export NODE_OPTIONS="--max-old-space-size=1024"
```

### Openclaw install

至此我們進入配置Openclaw的第一步,

```
curl -fsSL https://openclaw.ai/install.sh | bash
```

當我們執行完這裡之後等待它啟動我們首先yes確認知曉這是一個實驗項目,

如您完成openclaw的安裝但是第一次的部署是失敗的不要擔心,您可以使用以下任意初始化命令從頭我們的配置

```
openclaw onboard
```

```
openclaw config set agents.defaults.model sonnet
```

```
◇ I understand this is personal-by-default and shared/multi-user use requires lock-down. Continue?
│ Yes
│
◇ Onboarding mode
│ QuickStart
◇ Config handling
│ Reset
◇ Reset scope
│ Full reset (config + creds + sessions + workspace)
Failed to move to Trash (manual delete): ~/.openclaw/openclaw.json
Failed to move to Trash (manual delete): ~/.openclaw/agents/main/sessions
│
```

這裡對接我們自己的本地API以及調用模型名稱sonnet 順帶一提我們並未向litellm設定key因此如您遇到要求填寫key的情況下您可以任意填寫字符進行下一步

```
◇ Model/auth provider
│ LiteLLM
│
◇ Model configured ─────────────────────────────╮
│                                               │
│ Default model set to litellm/claude-opus-4-6 │
│                                               │
├────────────────────────────────────────────────╯
│
◇ Default model
│ Enter model manually
│
◇ Default model
│ sonnet
```

按照順序配置好必要的AI（如果在這裡失敗的話也不要著急） 以及channel 我選擇了telegrambot,如果不知道如何創建bot的話自行Google,

我這裡選擇的是pairing,這樣可以確保只需要邀請加入.

讓我頭大的還是

### gateway running

```
openclaw gateway run
```

啟動網關

頻繁報錯提示網關未安裝,其實網關是早就部署好了的,

我只能通過`bg`這樣的命令放入後台.

## 終

### Commands

```
openclaw config set model.baseURL http://127.0.0.1:4000/v1
#指向本地API端口
openclaw doctor --fix
#check openclaw debug
​
```

```
1. 检查端口是否被占用：lsof -i :18789
2. 查看错误日志：openclaw logs --follow
3. 运行诊断：openclaw doctor
​
```

### Document

關於更多還清查看官方文檔, 還清查看官方文檔, [openclaw.ai](https://docs.openclaw.ai/)

限制網路環境下參考部署[文檔](https://www.eet-china.com/mp/a479272.html)

[文檔](https://note.com/zephel01/n/n030ff0b67081)

最後的最後請務必為port `18789` 做好防火牆,或者轉移其他port

最後は、ここで読んだ人は、誠にありがとうございます。 以上です。

\
<br>
