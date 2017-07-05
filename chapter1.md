_**解释：**_

Iperf是一个网络性能测试工具。可以测试TCP和UDP带宽质量，可以测量最大TCP带宽，具有多种参数和UDP特性，可以报告带宽，延迟抖动和数据包丢失。Iperf在linux和windows平台均有二进制版本供自由使用。

* 参数说明

-s 以server模式启动，eg：iperf -s

-c host以client模式启动，host是server端地址，eg：iperf -c172.16.10.20

* 通用参数

-f \[k m K M\]分别表示以Kbits,Mbits,KBytes,MBytes显示报告，默认以Mbits为单位,eg：iperf -c 172.16.10.20 -f K

-i sec以秒为单位显示报告间隔，eg：iperf -c 172.16.10.20 -i 2

-l 缓冲区大小，默认是8KB,eg：iperf -c 172.16.10.20 -l 16

-m 显示tcp最大mtu值

-o 将报告和错误信息输出到文件eg：iperf -c 172.16.10.20 -o ciperflog.txt

-p 指定服务器端使用的端口或客户端所连接的端口eg：iperf-s-p9999;iperf-c172.16.10.20-p9999

-u 使用udp协议

-w 指定TCP窗口大小，默认是8KB

-B 绑定一个主机地址或接口（当主机有多个地址或接口时使用该参数）

-C 兼容旧版本（当server端和client端版本不一样时使用）

-M 设定TCP数据包的最大mtu值

-N 设定TCP不延时

-V 传输ipv6数据包

* server专用参数

-D 以服务方式运行iperf，eg：iperf -s -D

-R 停止iperf服务，针对-D，eg：iperf -s -R

* client端专用参数

-d 同时进行双向传输测试

-n 指定传输的字节数，eg：iperf -c 172.16.10.20 -n 100000

-r 单独进行双向传输测试

-t 测试时间，默认10秒,eg：iperf -c 172.16.10.20 -t 5

-F 指定需要传输的文件

-T 指定ttl值

* 应用实例

（1）使用服务端和客户端的默认设置进行测试

使用iperf -s命令将Iperf启动为server模式

`iperf –s`

`------------------------------------------------------------`

`Server listening on TCP port 5001`

`TCP window size:8.00KByte (default)`

`------------------------------------------------------------`

在客户机上使用iperf-c启动client模式

`iperf -c 192.168.1.19`

（2）服务器端和客户端的TCP窗口都开到300KB

`iperf -s -w 300K`

`------------------------------------------------------------`

`Server listening on TCP port 5001`

`TCP window size:300KByte`

`------------------------------------------------------------`

客户端设定报告间隔为2秒

`iperf -c 192.168.1.19 -f K -i 2 -w 300K`

测试传输约1MB数据

`iperf -c 192.168.1.19 -f K -i 2 -w 300K –n 1000000`

测试持续36秒

`iperf -c 192.168.1.19 -f K -i 2 -w 300K –t 36`

测试双向的传输

`iperf -c 192.168.1.19 -f K -i 2 -w 300K -n 10400000 –d`

UDP测试

`iperf -c 192.168.1.19 -f K -i 2 -w 300K –u`

其中-i参数的含义是周期性报告的时间间隔（interval），单位为秒；在上面的例子中，表示每隔2秒报告一次带宽等信息。

* 启动一个iperf服务器进程

首先要介绍的命令用来启动iperf服务器监听进程以便监听客户端连接

`iperf.exe -s -P 2 -i 5 -p 5999 -f k`

启动iperf，-p设定监听5999端口\(默认端口是5001\)；-P设定iperf只允许两个连接；-i设定每5秒汇报一次连接情况；

连接限制参数\(-P参数\)非常重要，当两个连接建立后，服务器进程就会退出。如果这个参数设定为0，那么iperf进程将持续监听端口，并且不限制连接数量

* 启动一个iperf客户端连接

iperf的另一半就是客户端，用来连接到服务器监听端口

`iperf.exe -c s-network1.amcs.tld -P 1 -i 5 -p 5999 -f B -t 60 -T 1`

连接到一台地址为s-network1.amcs.tld的服务器，端口为5999，连接60秒并且每5秒显示一次状态，命令启动后，s-network1主机被用来进行网络性能检测

