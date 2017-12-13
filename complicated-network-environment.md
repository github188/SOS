## 高级篇 - 复杂网络环境

---

##### **\#以下各环境\(WCCP除外\)，先进入SOS网络环境构建模式：**

```
root@SOS:~# sos netenv

vyos@SOS:~$ configure
```

##### **\#配置好命令后，使用以下两个进行保存，重启后依然生效**

```
vyos@SOS# commit save

vyos@SOS# save
```

#### WCCP

**\#在加速等产品配置好WCCP后，然后在SOS配置cisco的WCCP**

```
root@SOS:~# cisco -r       #按照提示一步步配置直到进入 “Router>”
```

**\#思科配置命令**

```
Router>enable
Router#configure terminal
Router(config)#ip routing
Router(config)#ip wccp version 2
Router(config)#ip access-list extended wccp_acl #配置ACL，命名为wccp_acl
Router(config-ext-nacl)#permit tcp any any #配置该条ACL的匹配的源目流量
Router(config-ext-nacl)#exit
Router(config)#ip wccp 66 redirect-list wccp_acl password sos #创建wccp id组66，对list wccp_acl生效，密码设置为sos
Router(config)#interface fastEthernet 0/0
Router(config-if)#ip wccp 66 redirect in #该wccp组重定向从0/0口进来的数据包
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#exit
Router#write
```

**\#查看wccp状态**

```
Router#show ip wccp 66 detail
```

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

