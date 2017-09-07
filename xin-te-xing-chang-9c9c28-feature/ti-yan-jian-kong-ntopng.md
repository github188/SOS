## 体验监控 - ntopng\(Netflow\)

---

#### **ntopng安装使用说明**

（1）如果没有安装（下一个版本默认安装），请查看官方提供的安装方式：[http://packages.ntop.org/apt-stable/](http://packages.ntop.org/apt-stable/)

（2）安装完成之后，修改配置文件，按照你的监控需求设置不同的参数，配置文件位置：/etc/ntopng/ntopng.conf

（3）执行重启命令使配置生效：service ntopng restart

（4）登录页面：{设备IP地址}:3000  （如果没有使用-l参数，请输入账号密码：admin/admin）

#### **ntopng.conf配置文件常用参数解释**

* 只监控指定的网口的流量

  `--interface|-i = eth0（监控单个网口）`

  `--interface|-i = eth0,eth1（监控多个网口）`

* 监控本地网段的流量

  `--local-networks|-m = "192.168.2.0/24,10.0.0.0/8,8.8.8.0/24"`

* 禁止登录

  `--disable-login|-l = 0   (0 - 使用localhost登录时不用输入账号密码 | 1 - 任意IP访问都不需要输入账号密码)`

* 抓包方向选择

  `--capture-direction = 0   (0=RX+TX (默认), 1=RX only, 2=TX only)`

* 抓包选项（BPF过滤器）

  ```
  --packet-filter|-B "dst port 80"   (就是常用的tcpdump的过滤方法)
  ```

#### **ntopng详情页面截图**![](/assets/ntopng详情页面截图.png)



