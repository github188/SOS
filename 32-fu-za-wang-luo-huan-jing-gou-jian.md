# 复杂网络环境构建

##### **\#以下各环境，先进入vyos模式**

```
root@SOS:~# su - vyos

vyos@SOS:~$ configure
```

##### **\#配置好命令后，使用以下两个进行保存，重启后依然生效**

```
vyos@SOS# commit save

vyos@SOS# save
```

#### WCCP

**待更新**

#### OSPF

**\#配置OSPF router-id（用于选取DR和BDR）**

`vyos@SOS# set protocols ospf parameters router-id 192.100.0.195`

**\#通告区域网络，并进行路由重发布**

```
vyos@SOS# set protocols ospf area 0.0.0.0 network 192.100.0.0/16

vyos@SOS# set protocols ospf area 0.0.0.0 network 100.100.21.0/24

vyos@SOS# set protocols ospf redistribute connected
```

**\#查看所配置协议（因为前面配置了RIP协议）**

`vyos@SOS# show protocols`

**\#OSPF配置成功，实际可以抓取OSPF路由报文（如hello，LSA等）来证明**

#### RIP

**\#宣告直连网络，并进行路由重发布**

```
vyos@SOS# set protocols rip network 192.100.0.0/16

vyos@SOS# set protocols rip network 100.100.21.0/24

vyos@SOS# set protocols rip redistribute connected
```

**\#查看所配置协议（因为前面配置了RIP协议）**

`vyos@SOS# show protocols`

**\#RIP配置成功，实际可以抓取RIP 路由报文来证明**

#### VRRP

**待更新**

