---
description: iproute2 for Debian12 & Debian13
---

# Debian12 & Debian13 系統手動路由分表的小插曲

我不知道有多少人依賴iproute2這個工具，但是在Debian12中如果我們要創建Route我們只需要

```
cat /etc/iproute2/rt_tables
```

```
#
# reserved values
#
255     local
254     main
253     default
0       unspec
#
# local
#
#1     inr.ruhep
200 ens5ipv6
```

可以看出`200 ens5ipv6` 這個是我手動創建的table 但是在Debian13 系統中這一個path將得到變更，事實上不只是iproute2這個工具的變動,我們經常使用的`sysctl.conf`所在path位置也有所變更, 首先,我們應當手動創建目錄路徑

```
mkdir -p /etc/iproute2/rt_tables.d
```

在這個文件夾下

```
vim /etc/iproute2/rt_tables.d/200-ens18ipv6.conf
```

```
Root# cat /etc/iproute2/rt_tables.d/200-ens18ipv6.conf
200 ens18ipv6
```

在裡面寫入我們的table值和table name

至此並沒有結束，當你查看這個table的時候會看到報錯

```
ip route show table ens18ipv6 Error: ipv4: FIB table does not exist. Dump terminated
```

請不要擔心，這是我們的table中還沒有任何路由和規則

通常我都設定這樣一個`steup.sh`腳本用作system開機後執行自己的rule\_table規則

```
#!/bin/bash
​
IP=<your VM interafe ip address>
TABLE=ens18ipv6<table name>
​
# add policy rule
ip -6 rule add from ${IP}/128 table ${TABLE} || true
ip -6 route add <VM interafe ip address/64> dev ens18 table ens18ipv6
​
ip -6 route add default via <VM interafe ip address Gateway> dev ens18 table ens18ipv6 || true
```

1.\<your VM interafe ip address> & \<VM interafe ip address>/64(netmask )

2.\<table name>

3.\<VM interafe ip address Gateway>

當您執行腳本後，再次check您的table

```
ip -6 route show table ens18ipv6
```

您會驗證您的表配置成功的,分表正確的.

以下為我個人table用途

通常我都有rule table的用途，tunnel用也罷，不希望目的地址找錯網口鏈路方向，所以這時候我會手動指定一些路由規則。

```
​
ip -6 route add <tunnel remote ipv6 address>/128 via <gateway ipaddress> dev ens18 table ens18ipv6 || true
```

經過這樣的設定後ip6tunnel網路便不會因為系統main\_table中太多路由而造成錯誤的地址尋路,遵循來自ISP的網關路由.

\
<br>
