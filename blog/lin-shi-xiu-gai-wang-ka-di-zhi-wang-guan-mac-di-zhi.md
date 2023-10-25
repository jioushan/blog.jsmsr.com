# 😆 临时修改网卡地址 网关 Mac地址

1、临时性地修改MAC并设置静态IP(重启networking后设置复原) 首先，必须关闭网卡设备，否则会报告系统忙，无法更改：

&#x20;`sudo ifconfig dummy1 down`&#x20;

然后，修改MAC地址，填写修改后的MAC

&#x20;`sudo ifconfig dummy1 hw ether XX:XX:XX:XX:XX:XX`&#x20;

重新启用网卡

&#x20;`sudo ifconfig dummy1 up`

设置主机静态IP地址、子网掩码的操作：&#x20;

`sudo ifconfig dummy1 xxx.xxx.xxx.xxx netmask xxx.xxx.xxx.xxx`&#x20;

添加默认网关的操作：&#x20;

`sudo route add default gw xxx.xxx.xxx.xxx`

