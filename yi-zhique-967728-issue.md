#### 已知缺陷\(issue\)

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

**bug2：/etc/resolv.conf修改后，重启后被恢复为8.8.8.8**

版本：SOS\(v0.0.1 Beta\)

原因：启动脚本做了恢复，以保证dns 8.8.8.8的正常

解决：

\#修改/etc/resolv.conf后，执行copy操作\(要一模一样\)，这样重启就不会被恢复了

cp /etc/resolv.conf /etc/bak.resolv.conf.bak

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
