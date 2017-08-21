# **tshark使用**

## 说明：

tshark非常适用于linux环境下对数据包的批量分析处理，是wireshark的命令行版本，所以几乎所有的wireshark数据包过滤命令对于tshark都适用。

tshark官方使用说明文档：[https://www.wireshark.org/docs/man-pages/tshark.html](https://www.wireshark.org/docs/man-pages/tshark.html)

## 常用参数：

-2  进行双程分析

-s  限制抓取报文大小

-f  设置抓包过滤条件

-T  解码数据包输出格式，只能是以下之一： fields \| json \| jsonraw \| pdml \| ps \| psml \| tabs \| text

-r  读取本地文件进行分析

-R  读取过滤器，非捕获（抓包）过滤器

-E  当-T字段指定时，设置输出选项，header=y意思是头部要打印

-e  当-T字段指定时，设置输出哪些字段

-c   抓多少个包后停止抓包

-i   指定抓包接口

## SOS集成功能：pcap-analysis

使用方式：pcap-analysis   \[数据包\]

可直接显示出数据包的五元组信息、TCP标记位与TCP选项信息

## 例子：

使用技巧：可以先使用wireshark过滤，得到过滤条件，然后在tshark使用

su - vyos -c "/usr/bin/tshark  -2 -r test.pcap -T fields -e eth.src -e eth.dst -e ip.src -e ip.dst -e ip.proto -e tcp.flags -e tcp.options"

\\只显示test.pcap报文的源目MAC地址，源目IP地址，协议，TCP标记 ，TCP选项

