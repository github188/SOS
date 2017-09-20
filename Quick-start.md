## 快速开始\(Quick Start\)

---

#### 登录

```
SSH：root/sos

WebUI：sos/Sangfor123

Webshell：root/sos
```

#### 修改ip

```
默认：
111.111.111.111(eth0口)

修改：
修改/etc/network/interfaces对应的address、netmask、gateway，保存
然后执行/etc/init.d/networking restart

建议eth0作为管理口,其他口作为打流口)
```

#### 增加网口

```
增加eth1：
编辑/etc/network/interfaces
将对应的eth0相关内容，拷贝复制，改成eth1，并修改eth1的address、netmask、gateway(例如下方)，保存

auto eth1
iface eth1 inet static
address 172.15.100.123
netmask 255.255.255.0
gateway 172.15.100.254

然后执行/etc/init.d/networking restart
```

#### DNS修改

```
默认：
8.8.8.8
修改：
修改/etc/resolv.conf(修改或增加你需要的dns)
执行：cp /etc/resolv.conf /etc/bak.resolv.conf.bak
（务必要执行，未执行重启后会被恢复为8.8.8.8）
```



