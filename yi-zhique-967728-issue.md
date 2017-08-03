#### 已知缺陷\(issue\)

---

**bug1：进入sos ftp服务器时，其中sangfor目录无法进入**

版本：SOS\(v0.0.1 Beta\)

原因：ftp不支持软链接

解决：

\#删除sangfor链接

rm /sos/server/feitp-server/REV/sangfor

\#创建挂载目录，将sangfor目录挂载

mkdir /sos/server/feitp-server/REV/sangfor

mount --bind /sangfor /sos/server/feitp-server/REV/sangfor

说明：mount --bind是linux内核从2.4.0开始支持的，可以把一部分文件系统挂载到文件系统中的其他位置，不需要设置，卸载的话，直接umount 全路径，如/sos/server/feitp-server/REV/sangfor

---

**bug2：/etc/resolv.conf修改后，重启后被恢复为8.8.8.8**

版本：SOS\(v0.0.1 Beta\)

原因：启动脚本做了恢复，以保证dns 8.8.8.8的正常

解决：

\#修改/etc/resolv.conf后，执行copy操作\(要一模一样\)，这样重启就不会被恢复了

cp /etc/resolv.conf /etc/bak.resolv.conf.bak

---

**bug3：系统时间错误**

版本：SOS\(v0.0.1 Beta\)

原因：由于安装系统时采用了UTC，UTC就是0时区的时间，是国际标准，而中国处于UTC+8时区

解决：

\#进入vyos的configure模式修改时区，并同步硬件时钟\(重启依然生效\)

su - vyos

configure

set system time-zone Asia/Shanghai

commit save

save

exit

exit

hwclock --systohc

\#可以使用date和hwclock --show查看时间是否一致，且是否是中国当前时间

---

**bug4：使用vpn并发工具时，重启lmdlan报错**

版本：SOS\(v0.0.1 Beta\)

原因：备份libstdc++.so.6.0.20为bak，并使用libstdc++.so.6.0.21替换libstdc++.so.6.0.20时，libstdc++.so.6仍链接到bak文件

root@SOS:/\# ls -l  /usr/lib/x86\_64-linux-gnu/libstdc++.so.6

lrwxrwxrwx 1 root root 26 Jul  5 09:10 /usr/lib/x86\_64-linux-gnu/libstdc++.so.6 -&gt; libstdc++.so.6.0.20.bakzxx

解决：

\#删除链接libstdc++.so.6，再次链接

cd /usr/lib/x86\_64-linux-gnu/

rm libstdc++.so.6

ln -s libstdc++.so.6.0.20 libstdc++.so.6

\#检查libstdc++.so.6，链接到libstdc++.so.6.0.20 ，说明解决成功\(可以重启lmdlan测试下\)

root@SOS:/usr/lib/x86\_64-linux-gnu\# ls -l  /usr/lib/x86\_64-linux-gnu/libstdc++.so.6

lrwxrwxrwx 1 root root 19 Jul 12 15:36 /usr/lib/x86\_64-linux-gnu/libstdc++.so.6 -&gt; libstdc++.so.6.0.20

---

**bug5：远程ssh很慢**

版本：SOS\(v0.0.1 Beta\)

原因：ssh开启了dns解析

解决：在vyos的configure模式下运行以下命令：

set service ssh disable-host-validation

commit save

save

---

**bug6：ftp删除临时ip失败**

版本：SOS\(v0.0.1 Beta\)

原因：ifconfig eth0:$i down命令删除ip失败

解决：使用命令：ip add del $wangduan$ipnum/16 dev eth0:$i

---

**bug7：ftp、cifs删除临时ip过块，导致下载出错**

版本：SOS\(v0.0.1 Beta\)

原因：文件在后台下载未完成，就删除临时ip

解决：增加curl进程判断，等所有下载完成再删除

while true

do

curl\_num=\`ps -ef\|grep curl\|wc -l\`

echo $curl\_num

if \[ $curl\_num == 1 \];then

for\(\(i=1;i&lt;=$loadernum;i++\)\)

do

ipnum=\`expr $i + 129\`

ip add del $wangduan$ipnum/16 dev eth9:$i

\#ifconfig eth9:$i down

done

exit 0

fi

sleep 1

done

---

**bug8：使用帮助中未指明ftp、cifs服务器根目录**

版本：SOS\(v0.0.1 Beta\)

原因：使用帮助中未指明ftp、cifs服务器根目录

解决：补充服务器根目录

---

**bug9：curl-loader跑高http、https并发时，请求报告中有ERR:超过0、3XX：超过0**

版本：SOS\(v0.0.1 Beta\)

原因：需要设置一些参数

解决：

**客户端：**

有些是因为客户端设备的默认参数、cpu高低，会影响客户端跑的结果

1、第一步

在SOS客户端下执行以下命令（重启不会保存）：

echo 1048576 &gt; /proc/sys/net/core/wmem\_max

/sbin/sysctl net.ipv4.tcp\_mem="1048576 1048576 1048576"

echo 0 &gt; /proc/sys/net/ipv4/conf/eth1/rp\_filter

echo 0 &gt; /proc/sys/net/ipv4/conf/all/rp\_filter

2、第二步

根据客户端cpu占用情况来执行第二步，如果单个cpu满，则启用curl-loader多cpu执行参数，例如：

curl-loader -t2 -f 配置文件\(注意：-t后没有空格，意思就是使用2个cpu，如果想用3个cpu，则-t3\)

**服务端：**

**ERR：超过0**

服务端默认参数，不能跑高并发，所以需要进行以下操作

1、第一步

备份配置文件

cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.sos

修改配置文件

修改/etc/nginx/nginx.conf，增加以下内容

在worker\_processes 1;下加上

worker\_rlimit\_nofile 65536;

将

events {

    worker\_connections 1024;

}

修改为

events {

    use epoll;

    worker\_connections 65536;

}

最后保存，执行：/etc/init.d/nginx restart

**3XX:超过0**

服务端的web默认会重定向，需要做一下修改：

1、第一步

备份配置文件

cp /etc/nginx/conf.d/default.conf  /etc/nginx/conf.d/default.conf.sos

修改配置文件

修改/etc/nginx/conf.d/default.conf，参照一下内容，注释成一致\(标注\#的\)，并保存

    \#location  /sangfor/ {

       \#proxy\_pass http://127.0.0.1/;

       \#root   /usr/share/nginx/html/;

      \#index  index.html index.htm;

   \#}



\#    location  /webshell/ {

\#        proxy\_pass https://127.0.0.1:4200/;

\#       root   /usr/share/nginx/html/webshell;

\#      index  webshell.html index.htm;

\#   }



    location / {

\#        proxy\_pass http://127.0.0.1:5920;

        root   /usr/share/nginx/html;

        index  index.html index.htm;

    }



2、第一步

备份配置文件

cp /etc/nginx/conf.d/example\_ssl.conf  /etc/nginx/conf.d/example\_ssl.conf.sos

修改配置文件

修改/etc/nginx/conf.d/example\_ssl.conf，参照一下内容，注释成一致\(标注\#的\)，并保存

    location /webshell/ {

\#        proxy\_pass https://127.0.0.1:4200;

\#  }



    location / {

\#        proxy\_pass http://127.0.0.1:5920;

        root   /usr/share/nginx/html;

        index  index.html index.htm;

  }

修改完后，执行：/etc/init.d/nginx restart

进行重跑验证（也有可能因为其他原因，例如客户端、服务端性能不够，如果还未解决，请联系周兴喜）





