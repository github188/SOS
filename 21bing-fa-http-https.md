## 简洁篇 - HTTP/HTTPS并发

#### **HTTP并发**

需求：1000并发\(此为演示，实际并发支持很高\)

环境：eth1已up，web服务器IP为:66.66.66.66/8

步骤：

**\#配置测试口ip**

ifconfig eth1 66.67.67.67 netmask 255.0.0.0

**\#复用自带模板**

cp /sos/client/curl-loader-0.56/conf-examples/10K.conf /sos/case/1000.conf

**\#修改HTTP并发配置，修改1000.conf中对应项，改为以下内容\(未列出的内容，不要删除\)，保存**

BATCH\_NAME= 1000

CLIENTS\_NUM\_MAX=1000

CLIENTS\_NUM\_START=20

CLIENTS\_RAMPUP\_INC=20

INTERFACE   =eth1

NETMASK=8

IP\_ADDR\_MIN= 66.67.67.68

IP\_ADDR\_MAX= 66.167.167.167

URL=http://66.66.66.66/index.html

**\#进入/sos/tmp目录，因为在此执行，会生成测试日志**

cd /sos/tmp

**\#执行，并发任务为一直跑**

curl-loader -f ../case/1000.conf

