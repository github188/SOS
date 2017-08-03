#### 常见问题\(Q&A\)

---

**Q1：进入sos ftp服务器时，其中sangfor目录无法进入**

版本：SOS\(v0.0.1 Beta\)

解决：

\#删除sangfor链接

rm /sos/server/feitp-server/REV/sangfor

\#创建挂载目录，将sangfor目录挂载

mkdir /sos/server/feitp-server/REV/sangfor

mount --bind /sangfor /sos/server/feitp-server/REV/sangfor

说明：mount --bind是linux内核从2.4.0开始支持的，可以把一部分文件系统挂载到文件系统中的其他位置，不需要设置，卸载的话，直接umount 全路径，如/sos/server/feitp-server/REV/sangfor

