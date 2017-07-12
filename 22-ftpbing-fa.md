## 简洁篇 - FTP并发

#### FTP**并发**

需求：50并发\(此为演示，实际并发支持很高\)

环境：eth1已up，FTP服务器IP为:66.66.66.66/8

步骤：

**\#配置测试口ip**

ifconfig eth1 66.67.67.67 netmask 255.0.0.0

**\#修改FTP并发脚本/usr/bin/ftp-loader，更改对应的项，为以下内容**

wangduan=66.66.66.

curl --verbose --interface $wangduan$ipnum -O -u "test:test" ftp://66.66.66.66/sos.txt &

_\#有两个ipnum要改，别改漏了_

ipnum=\`expr $i + 100\`

**\#执行FTP并发脚本**

ftp-loader 50

**\#可以使用以下命令观察是否在跑（因为文件很小，很快下完）**

iftop -N -n -i eth1

或者

_\#建议在客户端和服务端上都执行_

while :; do netstat -anp\|grep EST\|grep :21\|wc -l;sleep 1;done

