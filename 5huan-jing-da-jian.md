# 常用环境构建

以下各环境，先进入vyos模式：

`root@SOS:~# su - vyos`

`vyos@SOS:~$ configure`

配置好命令后，使用以下两个进行保存，重启后依然生效

`vyos@SOS# commit save`

`vyos@SOS# save`

#### 静态路由

配置静态路由：

第一种方法：使用root登录或者vyos用户执行sudo

`route add -net 192.168.20.0/24 gw 192.168.10.195`

第二种方法：使用configure模式的set protocols命令

`set protocols static route 192.168.20.0/24 next-hop 192.168.10.195 distance '1'`

#### VLAN

设置Vlan信息：

`set interfaces ethernet eth0 vif 10 address 192.168.10.196/24`

#### DHCP

搭建DHCP：

`set service dhcp-server shared-network-name 'LAN' authoritative enable`

`set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' start '192.168.10.128' stop '192.168.10.254'`

`set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' default-router '192.168.10.1'`

`set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' dns-server '192.168.10.1'`

`set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' domain-name 'internal-net'`

`set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' lease 86400`

#### TC

TC流量控制功能：

（1）首先新增两个流量策略：

`set traffic-policy shaper WAN-OUT bandwidth '50Mbit'`

`set traffic-policy shaper WAN-OUT default bandwidth '50%'`

`set traffic-policy shaper WAN-OUT default ceiling '100%'`

`set traffic-policy shaper WAN-OUT default queue-type 'fair-queue'`

`set traffic-policy shaper LAN-OUT bandwidth '200Mbit'`

`set traffic-policy shaper LAN-OUT default bandwidth '50%'`

`set traffic-policy shaper LAN-OUT default ceiling '100%'`

`set traffic-policy shaper LAN-OUT default queue-type 'fair-queue'`

（2）将增加的策略应用到网口上：

`set interfaces ethernet eth2 traffic-policy out 'WAN-OUT'`

`set interfaces ethernet eth0 traffic-policy out 'LAN-OUT'`

（3）设置延时和丢包（延时100ms、丢包1%）：

`tc qdisc add dev eth0 root netem delay 100ms loss 1%`

`tc qdisc add dev eth2 root netem delay 100ms loss 1%`

