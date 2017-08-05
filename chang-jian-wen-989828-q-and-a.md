#### 常见问题\(Q&A\)

---

**Q1：SOS默认密码是什么？**

答疑：

默认账号密码如下：

SSH：root/sos

WebUI：sos/Sangfor123

Webshell：root/sos

**Q2：SOS怎样配置永久静态路由？**

答疑：

第一种方法：使用root登录或者vyos用户执行sudo route add -net 192.168.20.0/24 gw 192.168.10.195

第二种方法：使用configure模式的set protocols命令

set protocols static route 192.168.20.0/24 next-hop 192.168.10.195 distance 1

配置后需要commit save 提交生效并保存

---

**Q3：WOC立华设备装了sos后，网口不亮，bypass灯常亮**

答疑：

那是因为设备之前装过woc系统，代码设置了bypass；

解决方法：做一个DOS启动盘 ，把附件lihua-bh61.rar解压放进去；然后插设备启动 选择U盘启动；分别执行1 ；2；3；4 即可。

---

**Q4：客户端和服务端为局域网同网段时，若跑高并发，会跑不上去，要设置下arp容量**

答疑：

要射那是因为设备之前装过woc系统，代码设置了bypass；

解决方法：做一个DOS启动盘 ，把附件lihua-bh61.rar解压放进去；然后插设备启动 选择U盘启动；分别执行1 ；2；3；4 即可。

