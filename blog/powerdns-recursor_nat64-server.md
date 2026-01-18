---
description: PowerDNS Recursor_NAT64 Server
---

# PowerDNS Recursor\_NAT64 Server

这项服务部署好其实有一阵子了,但是借着配置`Power DNS Recursior`这个模式它的递归模式. 我们来说说这里的配置.

截止到我写这篇文章,我注意到在Debian系统中较新的PowerDns版本已经趋向使用yml格式的配置了,而过去的配置能否继续使用,参数是否仍然正确。这取决你是否要upgrade. 因此借助我下面的配置,各位看官您自行判断利弊.

2.在`Debian`系统中它位于`/etc/powerdns`路径下,

如果您要做一个递归server的话,

```
root@debian# /etc/powerdns # cat recursor.conf
setuid=pdns
setgid=pdns
local-address=0.0.0.0, ::
#local-port=53
allow-from=127.0.0.0/8,::1,0.0.0.0/0,::/0
forward-zones-recurse=.=2606:4700:4700::1111
​
lua-config-file=/etc/powerdns/recursor.lua
```

这样的配置无意是您的传统配置,您只需要从我的配置参数中 借鉴即可,

`systemctl restart pdns-recursor` `systemctl status pdns-recursor`

确保`servers`有在跑起来,利用`ss -tulnp | grep :53`看看port上面的服务没问题,自行使用`dig`之类的工具`dig @::1 google.com AAAA` 测测看就好,不过`Debian`系统默认 `powerdns`不会监听`ipv6`,这点需要手动调试下.

3.但我想这篇文章是来说明它的NAT64配置yml格式的,其实要定义的参数都差不多.

```
root@debian:/etc/powerdns# cat recursor.yml
incoming:
listen:
   - 0.0.0.0
   - '::'
port: 53
allow_from:
   - 0.0.0.0/0
   - '::/0'
​
recursor:
daemon: false
threads: 2
forward_zones_recurse:
   - zone: '.'
    forwarders:
       - '2606:4700:4700::1111'
       - '2606:4700:4700::1001'
dns64_prefix: '<your NAT64 address rage address>/96'
lua_dns_script: '/etc/powerdns/dns64_simple.lua'
​
outgoing:
source_address:
   - '::'
dont_query:
   - '0.0.0.0/0'
​
logging:
disable_syslog: true
loglevel: 4
```

但是我使用的版本已经提示我,传统配置下 不如yml,因为尽管running起来,当我进行测试的时候NAT64的配置在yml格式下 似乎才正常工作,真的奇怪.

```
dns64=yes
setuid=pdns
setgid=pdns
daemon=no
disable-syslog=yes
loglevel=4
​
# 监听地址
local-address=0.0.0.0,::
​
# 允许访问
allow-from=0.0.0.0/0,::/0
​
# 上游 DNS
forward-zones-recurse=.=2001:4860:4860::6464
dnssec=off
# DNS64 配置
dns64-prefix=64:ff9b::/96
# Lua 脚本
#lua-dns-script=/etc/powerdns/dns64.lua
#lua-dns-script=/etc/powerdns/dns64_simple.lua
#lua-config-file=/etc/powerdns/dns64_simple.lua
# 性能设置
threads=2
dns64-synthall=yes
```

这里仅作参考,. 所以我更推荐您使用yml.由于它支持lua脚本特色这是它的一个很大的可玩性. 如您使用NAT64 server请确保您的lua中有以下描述:

```
root@debian:/etc/powerdns# cat /etc/powerdns/dns64_simple.lua
function preresolve(dq)
   -- 讓 PowerDNS 自動處理 DNS64
   return false
end
root@pve:/etc/powerdns# 
```

最后的music

{% embed url="https://embed.music.apple.com/jp/album/lilium-from-elfen-lied/1345117976?i=1345117982" %}
