#### 已知缺陷\(issue\)

**bug1：进入sos ftp服务器时，其中sangfor目录无法进入版本：SOS\(v0.0.1 Beta\)**

原因：ftp不支持软链接

解决：

\#删除sangfor链接

rm /sos/server/feitp-server/REV/sangfor

\#创建挂载目录，将sangfor目录挂载

mkdir /sos/server/feitp-server/REV/sangfor

mount --bind /sangfor /sos/server/feitp-server/REV/sangfor

**bug2：/etc/resolv.conf修改后，重启后被恢复为8.8.8.8**

原因：启动脚本做了恢复，以保证dns 8.8.8.8的正常

解决：

\#修改/etc/resolv.conf后，执行copy操作\(要一模一样\)，这样重启就不会被恢复了

cp /etc/resolv.conf /etc/bak.resolv.conf.bak

