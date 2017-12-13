## 高级篇 - 复杂网络环境

---

##### **\#以下各环境，先进入SOS网络环境构建模式：**

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

**\#使用模拟器允许cisco镜像实现**

root@SOS:~\# cisco -r       \#按照提示一步步配置直到进入 “Router&gt;”

**\#思科配置命令**

Router&gt;enable

Router\#configureterminal      \#进入配置模式

Enterconfigurationcommands,oneperline.EndwithCNTL/Z.

Router\(config\)\# ip access-list extended 195  \#配置ACL，命名为195

Router\(config-ext-nacl\)\#permit tcp xx.xx.xx.xx 0.0.0.255 yy.yy.yy.yy 0.0.0.255 \#配置该条ACL的匹配的源目流量

Router\(config\)\#ip wccp 70 redirect-list 195 password 123  \#创建一个id组为70，对list195生效、连接密码为123的wccp配置

Router\(config\)\#interfacefastEthernet0/0

Router\(config-if\)\#ip wccp 70 redirect in  \#该wccp组重定向从0/0口进来的数据包

Router\(config-if\)\#exit

Router\(config\)\#interfacefastEthernet1/0

Router\(config-if\)\#ip wccp 70 redirect out  \#该wccp组将数据包从1/0口重定向出去

Router\(config-if\)\#end

Router\#

Router\#write

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

