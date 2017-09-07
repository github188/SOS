## 简洁篇 - CIFS并发

---

#### CIFS**并发**

环境：CIFS服务器IP为:66.66.66.66/8\(若使用sos作为服务端，则默认带CIFS服务\)

**\#查看帮助说明**

**cifs-loader -h**

用法1-命令行方式  : cifs-loader \[tisolfh\] -d cifs-server

用法2-指定配置文件：cifs-loader -f file

例子：cifs-loader -t 50 -i eth7 -s 7.7.7.7 -d 9.9.9.9  or  cifs-loader -f /sos/case/cifs-loader.conf

选项：

```
-t threads      并发线程数,默认值为1,当起始网段末位ip超过255后,从下一网段继续叠加,注意不要网关ip冲突

-i interface    业务网口,默认值为eth7

-s start\_ip     起始ip,默认值为7.7.7.70,掩码为16位

-d destination  目的cifs服务器,默认为空

-o object       指定下载文件,默认为服务器根目录下的sos.txt;指定路径eg: sangfor/sos.txt

-l loop         下载次数，默认为循环下载

-f file         指定配置文件

-h help
```

**\#范例1：跑100并发，业务网口为eth7，开始ip为7.7.7.50，cifs服务器为9.9.9.9，下载的文件为test.txt**

cifs-loader -t 100 -i eth7 -s 7.7.7.50 -d 9.9.9.9 -o test.txt

**\#范例2：50并发，下载100次默认文件sos.txt**

cifs-loader -t 50 -i eth0 -s 7.7.7.50 -d 66.66.66.66 -l 100

**\#范例3：复用自带模板**

cp /sos/case/cifs-loader.conf /sos/case/1000.conf

cifs-loader -f /sos/case/1000.conf

**\#注意事项**

虚拟ip会从起始ip开始叠加，超过255后会从下一网段继续，注意不要和网关ip冲突

_**\#建议在客户端和服务端上都执行**_

while :; do netstat -anp\|grep EST\|grep :445\|wc -l;sleep 1;done

