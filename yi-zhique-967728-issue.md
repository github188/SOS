#### 已知缺陷\(issue\)

bug1:

版本：SOS\(v0.0.1 Beta\)

描述：进入sos ftp服务器时，其中sangfor目录无法进入

原因：ftp不支持软链接

解决：

\#删除sangfor链接

rm /sos/server/feitp-server/REV/sangfor

\#创建挂载目录，将sangfor目录挂载

mkdir /sos/server/feitp-server/REV/sangfor

mount --bind /sangfor /sos/server/feitp-server/REV/sangfor

