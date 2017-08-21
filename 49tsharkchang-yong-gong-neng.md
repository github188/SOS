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





