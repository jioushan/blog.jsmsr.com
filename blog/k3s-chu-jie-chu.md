---
description: 集群化作業
---

# k3s初接觸

初次接觸k3s 1.首先要有一個集群化概念,大抵就是

肯定有主master有woker的節點.

Install !!waring⚠️本安裝僅在Linux amd/x86環境下執行,若您為Arch64,mpls等其他架構請具體參考不同工具的安裝文檔.

1.我們使用官方的 https://k3s.io 安裝腳本

```bash
curl -sfL https://get.k3s.io | sh - 

k3s kubectl get node 
```

1.1若您在國內請參考官方的文檔 https://docs.k3s.io/zh/quick-start

中国用户，可以使用以下方法加速安装：

```bash
curl -sfL https://rancher-mirror.rancher.cn/k3s/k3s-install.sh | INSTALL_K3S_MIRROR=cn sh -
```

2.add woker 節點如何安裝

首先從我們的master獲取我們的對接連結

```bash
export K3S_URL="https://ip address:6443"
```

請自行替換掉`ipaddress` 這一項

```bash
export K3S_TOKEN="token"
```

請自行替換`token的值`

具體查找請在master執行下面命令

```bash
cat /var/lib/rancher/k3s/server/node-token
```

然後再執行安裝腳本

```bash
curl -sfL https://get.k3s.io | sh -
```

若您有信心

```
curl -sfL https://get.k3s.io | K3S_URL=https://ip address:6443 K3S_TOKEN="token" sh -
```

請自行替換其中的 `ip address``,token`的值

安裝卡住,如果網路環境沒問題的情況下 通常通過`systemctl status k3s-agent` 自行分析日誌是否成功對接master還是token錯誤.

3, 如果正確啟用 我們在master可以執行下面命令觀察到 我們added的wokers數量

```bash
k3s kubectl get node
```

4.  Kubernetes KREW和包管理器 helm

    krew插件請參考官方的安裝文檔，要求github網路環境,以及 `apt install git tar -y`

    確保當前Debian系已安裝git和 tar https://krew.sigs.k8s.io/docs/user-guide/setup/install/

可以使用github加速類的網址修改github下載這裡的url,完成特殊環境下VM的安裝.

Helm的安裝請參考官方的文檔 https://helm.sh/ja/docs/intro/install/

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```

```bash
chmod 700 get_helm.sh
```

```bash
./get_helm.sh
```

如果想直接执行安装，运行\`

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

\`

helm採用Azure.cn的mirror解決你國訪問問題

```bash
helm repo add stable http://mirror.azure.cn/kubernetes/charts/
```

```bash
helm repo update
```

查看添加的倉庫

```bash
helm repo list
```
