## 常见问题\(Q&A\)

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

**Q4：客户端和服务端为局域网同网段时，若跑http、https高并发，会跑不上去**

答疑：

若SOS做服务端，则在SOS服务端上，设置下arp容量（重启不会保存）

echo 1280 &gt;  /proc/sys/net/ipv4/neigh/default/gc\_thresh1

echo 5120 &gt;  /proc/sys/net/ipv4/neigh/default/gc\_thresh2

echo 10240 &gt;  /proc/sys/net/ipv4/neigh/default/gc\_thresh3

---

**Q5：服务端做http服务器时，如何启用多cpu和多进程**

答疑：改/etc/nginx/nginx.conf为以下\(2个进程，4个cpu\)，并/etc/init.d/nginx restart

root@SOS:/usr/share/nginx/html\# head -n4 /etc/nginx/nginx.conf

user  nginx;

worker\_processes 2;

worker\_cpu\_affinity 00000001 00000010 00000100 00001000;

---

**Q6：curl-loader如何下载多个url**

答疑：在你指定的配置文件中，添加多个url即可，URLS\_NUM改成你的URL数量

URLS\_NUM= 5

\#\#\#\#\#\#\#\#\#\#\# URL SECTION \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

URL=[http://66.66.66.66/1.html](http://66.66.66.66/1.html)

URL=[http://66.66.66.66/2.html](http://66.66.66.66/2.html)

URL=[http://66.66.66.66/3.html](http://66.66.66.66/3.html)

URL=[http://66.66.66.66/4.html](http://66.66.66.66/4.html)

URL=[http://66.66.66.66/5.html](http://66.66.66.66/5.html)

---

**Q7：curl-loader如何使用少量ip实现多并发？**

答疑：使用IP\_SHARED\_NUM参数即可\(在curl-loader使用的conf文件中加入此参数\)，若IP\_SHARED\_NUM=3，则会在3个ip中不断轮询。

注意

1、IP\_SHARED\_NUM和-t多cpu参数一起使用时，-t需要和IP\_SHARED\_NUM的值相等

2、当不用-t多cpu时，则没有这个限制

