## 场景篇 - WOC稳定性测试仿真

---

#### 说明：

```
构造ftp，cifs，http三种背景流量，间断性重启加速设备waccd进程测试，执行日志信息保存在当前目录的stability_log下，
建议在/sos/tmp下执行
用法1-命令行方式  ：stability-loader -s start_ip -d stability-server
用法2-指定配置文件：stability-loader -f file
```

#### 举例：

```
以30.1.1.1为源100.0.0.1为目的，ftp，cifs，http连接数默认，指定ftp，cifs下载文件为/sangfor/1M，http下
载文件为/sangfor/100KB，持续打流量且不重启加速；指定监控WOC设备地址为200.200.140.63，循环执行脚本50次

stability-loader -s 30.1.1.1 -d 100.0.0.1 -o /sangfor/1M -p /sangfor/100KB -w 200.200.140.63 -n -1 -l 50
```

#### 选项：

可输入参数可指定配置文件使用。

选项：

```
-t threads        ftp并发线程数,默认值为50,为0表示不构造ftp流量
-a amount         cifs连接数,默认值为50,为0表示不构造cifs流量
-q quantity       http连接数,默认值为1000,为0表示不构造http流量
-n number         http流量回放执行次数,默认50次,-1表示持续回放且不重启加速
-i interface      业务网口,默认值为eth1
-s start_ip       起始ip,默认值为空
-d destination    目的服务器,默认为空
-o object_ftp_cifs  指定ftp,cifs下载文件，默认为跟目录下的sos.txt；文件大小建议100M左右
-p papers_http    指定http下载文件,默认为服务器根目录下的sos.txt；文件大小建议100KB左右
-w woc_ip         测试加速设备IP地址
-l loop           脚本循环执行次数,默认-1,-1表示持续循环执行
-f file           指定配置文件
-h help           帮助
```

## 



