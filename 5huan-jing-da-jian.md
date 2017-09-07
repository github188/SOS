## 常用篇 - 基础网络环境

---

##### **\#以下各环境，先进入vyos模式：**

```
root@SOS:~# su - vyos

vyos@SOS:~$ configure
```

##### **\#**配置好命令后，使用以下两个进行保存，重启后依然生效

```
vyos@SOS# commit save

vyos@SOS# save
```

#### 默认服务

| **默认已启动** | **端口** | **根目录** | **公共目录** |
| :--- | :--- | :--- | :--- |
| HTTP/HTTPS | 80/443 | /usr/share/nginx/html/ | /sangfor/ |
| FTP | 21 | /sos/server/feitp-server/REV/ | /sangfor/ |
| CIFS | 445 | /home/samba/test/ | /sangfor/ |

#### 静态路由

**\#方法1：使用root登录或者vyos用户执行sudo**

`route add -net 192.168.20.0/24 gw 192.168.10.195`

**\#方法2：使用configure模式的set protocols命令**

`set protocols static route 192.168.20.0/24 next-hop 192.168.10.195 distance '1'`

#### VLAN

`set interfaces ethernet eth0 vif 10 address 192.168.10.196/24`

#### DHCP

```
set service dhcp-server shared-network-name 'LAN' authoritative enable

set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' start '192.168.10.128' stop '192.168.10.254'

set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' default-router '192.168.10.1'

set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' dns-server '192.168.10.1'

set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' domain-name 'internal-net'

set service dhcp-server shared-network-name 'LAN' subnet '192.168.10.0/24' lease 86400
```

#### 流控

**\#首先新增两个流控策略**

```
set traffic-policy shaper WAN-OUT bandwidth '50Mbit'

set traffic-policy shaper WAN-OUT default bandwidth '50%'

set traffic-policy shaper WAN-OUT default ceiling '100%'

set traffic-policy shaper WAN-OUT default queue-type 'fair-queue'

set traffic-policy shaper LAN-OUT bandwidth '200Mbit'

set traffic-policy shaper LAN-OUT default bandwidth '50%'

set traffic-policy shaper LAN-OUT default ceiling '100%'

set traffic-policy shaper LAN-OUT default queue-type 'fair-queue'
```

**\#将策略应用到网口**

```
set interfaces ethernet eth2 traffic-policy out 'WAN-OUT'

set interfaces ethernet eth0 traffic-policy out 'LAN-OUT'
```

**\#设置延时和丢包（延时100ms、丢包1%）**

```
tc qdisc add dev eth0 root netem delay 100ms loss 1%

tc qdisc add dev eth2 root netem delay 100ms loss 1%
```

#### 广域网环境

**\#在eth0口模拟100ms的时延**

```
tc qdisc add dev eth0 root netem delay 100ms
```

**\#删除在eth0口设置的时延**

```
tc qdisc del dev eth0 root netem delay XXXms
```

**\#在eth0口模拟100ms＋－10ms（随机分布）的时延**

```
tc qdisc change dev eth0 root netem delay 100ms 10ms 25%
tc qdisc change dev eth0 root netem delay 100ms 10ms
```

**\#在eth0口模拟时延，在100ms＋－10ms之间的正态分布**

```
tc qdisc change dev eth0 root netem delay 100ms 10ms distribution normal
```

**\#在eth0口模拟丢包（丢包率为0.1％）**

```
tc qdisc change dev eth0 root netem loss 0.1%
```

**\#在eth0口模拟丢包（丢包率为0.1％－33.33％之间随机分布）**

```
tc qdisc change dev eth0 root netem loss 0.3% 33.33%
```

**\#在eth0口模拟数据包重传（重传率为1％）**

```
tc qdisc change dev eth0 root netem duplicate 1%
```

**\#在eth0口模拟数据失真\(数据能传过去，但数据错误）**

```
tc qdisc change dev eth0 root netem corrupt 0.1%
```

**\#在eth0口模拟重新请求数据包**

```
tc qdisc change dev eth0 root netem gap 5 delay 10ms 
备注：第五、十、十五个（每隔5个）包会立即发送，而其余的包会延时10ms再发送
```

```
tc qdisc change dev eth0 root netem delay 10ms reorder 25% 50% 
备注：25％-50％的数据包会被立即发送，其余的延时10ms再发送，这种可以模拟乱序的情况
```

**\#模拟传输速率**

```
# tc qdisc add dev eth0 root handle 1:0 netem delay 100ms
# tc qdisc add dev eth0 parent 1:1 handle 10: tbf rate 256kbit buffer 1600 limit 3000
```

**\#原理**

```
对互联网而言，一切都是数据包，操控网络实际上是在操控数据包，操控它如何产生，路由，传输，分片等等。
TC在数据包离开系统的时候进行控制，在IP层与网卡之间做手脚，实际上，负责将数据包传递到物理层的正是TC模块，
这意味着在系统内核中，TC作为数据包的调度者是一直运作的，甚至在你不想用他的时候，一般情况下，TC维持一个
先进先出的数据队列。

数据包入队的时候首先调用根队列规定的过滤器，根据过滤器定义的规则将数据包交给某个类，如果该类不是叶子类，
将会调用该类定义的过滤器进一步分类，若该类没有定义过滤器，就会交给包含他的队列规定的默认类来处理，若接
收到数据包的类是叶子类，数据包将进入到叶子类的队列规定里面排队，需要注意的是：过滤器只能将数据包交给某个
类，类再将数据包放入自己的队列规定进行排队，而不能直接交给某个队列规定。

接受包从数据接口进来后，经过流量限制，丢弃不合规定的数据包，然后输入多路分配器判断：若接受包的目的地是
本主机，那么将该包送给上册处理，否则需转发，将接受包交到转发块处理。转发块同时也接收本主机上层产生的包。
转发块通过查看路由表，决定所处理包的下一跳，然后对包排序以便将他们传送到输出接口。

Linux的TC主要是在输出接口排列时进行处理和实现的。
```



