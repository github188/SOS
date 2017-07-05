        curl-loader（也被称为“omes-NIK”和“davilka”）一个功能强大的C语言应用程序测试和负载生成工具。 它使用HTTP，FTP和TLS / SSL堆栈，模拟每个具有自己的IP地址的数万个用户/客户端。 目标是提供一个替代Spirent Avalanche和Ixia IxLoad的工具。



| GENERAL SECTION |  |
| :--- | :--- |
| BATCH\_NAME= 10K | 测试对象的名称 |
| CLIENTS\_NUM\_MAX=10000 | 最大虚拟用户数 |
| CLIENTS\_NUM\_START=100 | 一开始启动用户数 |
| CLIENTS\_RAMPUP\_INC=50 | 每秒钟启动几个用户 |
| INTERFACE =eth0 | 使用的网卡eth0 |
| NETMASK=16 | 子网掩码位数 |
| IP\_ADDR\_MIN= 192.168.1.1 | 起始IP地址 |
| IP\_ADDR\_MAX= 192.168.53.255 | 最后的IP地址范围   \#Actually - this is for self-control |
| CYCLES\_NUM= -1 | -1 表示无限循环 |
| URLS\_NUM= 1 |  |
| **URL SECTION** |  |
| URL=http://localhost/index.html | 要测试的URL |
| URL\_SHORT\_NAME="local-index" |  |
| REQUEST\_TYPE=GET |  |
| TIMER\_URL\_COMPLETION = 5000 |  |
| TIMER\_AFTER\_URL\_SLEEP =20 |  |

配置上面的参数生成：test.conf，使用下面的命令开始测试

`./curl-loader -f test.conf`

详细使用参见GitHub：[https://github.com/aschepis/curl-loader](https://github.com/aschepis/curl-loader)

