# 复杂网络环境构建

#### OSPF

\#进入vyos配置模式

`root@SOS:~# su - vyos`

\#首先进入配置模式

`vyos@SOS:~$ configure`

\#配置OSPF router-id（用于选取DR和BDR）

`vyos@SOS# set protocols ospf parameters router-id 192.100.0.195`

\#通告区域网络

`vyos@SOS# set protocols ospf area 0.0.0.0 network 192.100.0.0/16`

\#通告区域网络

`vyos@SOS# set protocols ospf area 0.0.0.0 network 100.100.21.0/24`

\#路由重发布

`vyos@SOS# set protocols ospf redistribute connected`

\#保存，并生效

`vyos@SOS# commit save`

\#保存为启动配置

`vyos@SOS# save`

\#查看所配置协议（因为前面配置了RIP协议）

`vyos@SOS# show protocols`

