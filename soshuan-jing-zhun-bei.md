#### 快速开始\(Quick Start\)

**修改ip**

默认：默认ip为111.111.111.111\(eth0口\)

修改：

修改/etc/network/interfaces

然后执行/etc/init.d/network restart

**增加网口**

增加eth1：

若要添加网口，如添加eth1，编辑/etc/network/interfaces

将对应的eth0相关内容，拷贝复制，改成eth1，并修改eth1的address、netmask、gateway\(例如下方\)，保存

auto eth1

iface eth1 inet static

address 172.15.100.123

netmask 255.255.255.0

gateway 172.15.100.254

然后执行/etc/init.d/network restart


