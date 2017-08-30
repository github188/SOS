## 简洁篇 - FTP并发

#### FTP**并发**

需求：50并发\(此为演示，实际并发支持很高\)

环境：FTP服务器IP为:66.66.66.66/8\(若使用sos作为服务端，则默认带FTP服务\)

步骤：

**\#修改FTP并发脚本/usr/bin/ftp-loader，更改对应的项，为以下内容**

**\#FTP服务器和需要下载的文件**

nohup curl --verbose --interface $ip -O -u "test:test" ftp://66.66.66.66/sos.txt &gt;/dev/null 2&gt;&1 &

**\#执行FTP并发脚本，使用方法：ftp-loader 并发数  起始ip  业务网口**

ftp-loader 50 7.7.7.50 eth0

**\#因为文件很小，很快下完**

ftp count 为 0  表示已完成下载

或者

_**\#建议在客户端和服务端上都执行**_

while :; do netstat -anp\|grep EST\|grep :21\|wc -l;sleep 1;done

