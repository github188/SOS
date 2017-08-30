## 简洁篇 - CIFS并发

#### CIFS**并发**

需求：50并发\(此为演示，实际并发支持很高\)

环境：CIFS服务器IP为:66.66.66.66/8\(若使用sos作为服务端，则默认带CIFS服务\)

步骤：

**\#修改CIFS并发脚本/usr/bin/cifs-loader，更改对应的项，为以下内容**

**\#CIFS服务器和需要下载的文件**

nohup curl --verbose --interface $wangduan$ipnum -O -u "test:test" smb://66.66.66.66/test/sos.txt &gt;/dev/null 2&gt;&1 &

**\#执行CIFS并发脚本，使用方法：cifs-loader 并发数 起始ip 业务网口**

cifs-loader 50 7.7.7.50 eth0

**\#因为文件很小，很快下完**

cifs count 为 0 表示已完成下载

或者

_\#建议在客户端和服务端上都执行_

while :; do netstat -anp\|grep EST\|grep :445\|wc -l;sleep 1;done

