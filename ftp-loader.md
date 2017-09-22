## 简洁篇 - FTP并发

---

#### FTP并发

环境：FTP服务器IP为:66.66.66.66/8\(若使用sos作为服务端，则默认带FTP服务\)

**\#查看帮助说明  
  ftp-loader -h**

```bash
用法1-命令行方式  : ftp-loader [atoTlpfh] -i interface -s start_ip -d ftp-server
用法2-指定配置文件：ftp-loader -f file
例子1- 下载  a.txt：ftp-loader -t 50 -i eth0 -s 7.7.7.7 -d 9.9.9.9 -o a.txt
例子2- 上传  b.txt：ftp-loader -t 50 -i eth0 -s 7.7.7.7 -d 9.9.9.9 -T b.txt
错误排查日志：/sos/tmp/ftp-loader-日期.error
选项：    
    -a active        使用主动模式下载，默认是被动模式
    -t threads       并发线程数,默认值为1,当起始网段末位ip超过255后,从下一网段继续叠加,注意不要网关ip冲突
    -i interface     业务网口,eg: eth0
    -s start_ip      起始ip,eg: 7.7.7.77
    -d destination   远程ftp服务器
    -o object        下载文件时默认为服务器根目录下的sos.txt，指定路径eg: sangfor/sos.txt；上传时默认是服务器根目录，指定路径eg:-o sangfor or -o sangfor/
    -T upload        上传文件，如下载sos.txt为 -T sos.txt，指定路径eg:-T sangfor/sos.txt
    -l loop          下载次数，默认为循环下载
    -p share_ip_num  共享ip数，eg：-t 100 -p 10，表示用10个ip跑100并发，解决ip资源不足
    -f file          指定配置文件
    -h help          帮助
```

**\#范例1：跑100并发，业务网口为eth7，起始ip为7.7.7.50，ftp服务器为9.9.9.9，下载的文件为test.txt**

```
ftp-loader -t 100 -i eth7 -s 7.7.7.50 -d 9.9.9.9 -o test.txt
```

**\#范例2：50并发，下载100次，默认文件sos.txt  **

```
ftp-loader -t 50 -i eth0 -s 7.7.7.50 -d 66.66.66.66 -l 100
```

**\#范例3：30并发，上传50次test.txt到sangfor目录**

```
ftp-loader -t 30 -l 50 -s 7.7.7.50 -d 66.66.66.66 -T test.txt -o sangfor/
```

**\#范例4：复用自带模板  **

```
cp /sos/case/ftp-loader.conf /sos/case/1000.conf
ftp-loader -f /sos/case/1000.conf
```

**\#查看错误日志**

```
cat /sos/tmp/ftp-loader-日期.error
```

**\#注意事项  **

```
虚拟ip会从起始ip开始叠加，超过255后会从下一网段继续，注意不要和网关ip冲突
```

_**\#建议在客户端和服务端上都执行**_

```
while :; do netstat -anp|grep EST|grep :21|wc -l;sleep 1;done
```



