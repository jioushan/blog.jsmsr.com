# Prometheus Deploy

## prometheus\_node\_exporter Deploy

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.10.2/node_exporter-1.10.2.linux-amd64.tar.gz
```

```
tar -xvf node_exporter-1.10.2.linux-amd64.tar.gz
```

```
cd node_exporter-1.10.2.linux-amd64/
```

```
cp node_exporter /usr/local/bin/
```

```
cat <<EOF | tee /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=root
Group=root
Type=simple
ExecStart=/usr/local/bin/node_exporter
[Install]
WantedBy=multi-user.target
EOF
```

```
systemctl daemon-reload
```

```
systemctl enable --now node_exporter
```

默认node\_exporter running on `9100` port

使用prometheus应确保部署在任意VM时 时间日期应当一致,

## prometheus Deploy

```
wget https://github.com/prometheus/prometheus/releases/download/v3.9.1/prometheus-3.9.1.linux-amd64.tar.gz
```

```
 418  sudo useradd --no-create-home --shell /bin/false prometheus
 419  sudo mkdir /etc/prometheus
 420  sudo mkdir /var/lib/prometheus
 421  sudo chown prometheus:prometheus /etc/prometheus
 422  sudo chown prometheus:prometheus /var/lib/prometheus
```

```
425 tar -xvf prometheus-3.9.1.linux-amd64.tar.gz
 426  ls
 427  cd prometheus-3.9.1.linux-amd64
 428  ls
 429  sudo cp prometheus /usr/local/bin/
 430  sudo cp promtool /usr/local/bin/
 431  sudo cp -r consoles /etc/prometheus
 432  sudo cp -r console_libraries /etc/prometheus
 433  sudo cp prometheus.yml /etc/prometheus/prometheus.yml
 434  sudo chown -R prometheus:prometheus /etc/prometheus/
 435  sudo chown prometheus:prometheus /usr/local/bin/promtool
 436  sudo chown prometheus:prometheus /usr/local/bin/prometheus
 437  vim /etc/systemd/system/prometheus.service
 438  sudo systemctl daemon-reload
 439  sudo systemctl enable --now prometheus
 440  sudo systemctl status prometheus
```

时间同步

```
 442 timedatectl status
 443 timedatectl set-ntp true
 444 timedatectl show-timesync --all
 445 apt install chrony -y
 446 chronyc sources -v
 447 timedatectl show-timesync --all
 448 timedatectl status
```

Config和running状态检查,默认部署在`9090`port

```
 449  vim /etc/prometheus/prometheus.yml
 450  curl -X POST http://localhost:9090/-/reload
```

```
452 sudo systemctl restart prometheus
453 sudo systemctl status prometheus
```

## 对接Node

```
scrape_configs:
 # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
 - job_name: "prometheus"
​
   # metrics_path defaults to '/metrics'
   # scheme defaults to 'http'.
​
  static_configs:
     - targets: ["localhost:9090"]
      # The label name is added as a label `label_name=<label_value>` to any timeseries scraped from this config.
      labels:
        app: "prometheus"
 - job_name: "HKG1"
  static_configs:
     - targets:
       - "[ipv6address]:9100"
```

自行替换`[ipv6address]`

请确保宿主机网络正常可以正常访问node端网络

prometheus采用http方式来获取node信息,可以对接Garafane 通过图表获取漂亮监控<br>

<br>
