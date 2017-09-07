# 广域网环境构建

## 工作原理：

对互联网而言，一切都是数据包，操控网络实际上是在操控数据包，操控它如何产生，路由，传输，分片等等。TC在数据包离开系统的时候进行控制，在IP层与网卡之间做手脚，实际上，负责将数据包传递到物理层的正是TC模块，这意味着在系统内核中，TC作为数据包的调度者是一直运作的，甚至在你不想用他的时候，一般情况下，TC维持 一个先进先出的数据队列。

数据包入队的时候首先调用根队列规定的过滤器，根据过滤器定义的规则将数据包交给某个类，如果该类不是叶子类，将会调用该类定义的过滤器进一步分类，若该类没有定义过滤器，就会交给包含他的队列规定的默认类来处理，若接收到数据包的类是叶子类，数据包将进入到叶子类的队列规定里面排队，需要注意的是：过滤器只能将数据包交给某个类，类再将数据包放入自己的队列规定进行排队，而不能直接交给某个队列规定。

接受包从数据接口进来后，经过流量限制，丢弃不合规定的数据包，然后输入多路分配器判断：若接受包的目的地是本主机，那么将该包送给上册处理，否则需转发，将接受包交到转发块处理。转发块同时也接收本主机上层产生的包。转发块通过查看路由表，决定所处理包的下一跳，然后对包排序以便将他们传送到输出接口。

Linux的TC主要是在输出接口排列时进行处理和实现的。

## TC命令行

参考网站：[http://www.linux-foundation.org/en/Net:Netem](http://www.linux-foundation.org/en/Net:Netem)

* [在eth0口模拟100ms的时延](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc add dev eth0 root netem delay 100ms
```

* [删除在eth0口设置的时延](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc del dev eth0 root netem delay XXXms
```

* [在eth0口模拟100ms＋－10ms（随机分布）的时延](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem delay 100ms 10ms 25%
tc qdisc change dev eth0 root netem delay 100ms 10ms
```

* [在eth0口模拟时延，在100ms＋－10ms之间的正态分布](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem delay 100ms 10ms distribution normal
```

* [在eth0口模拟丢包（丢包率为0.1％）](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem loss 0.1%
```

* [在eth0口模拟丢包（丢包率为0.1％－33.33％之间随机分布）](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem loss 0.3% 33.33%
```

* [在eth0口模拟数据包重传（重传率为1％）](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem duplicate 1%
```

* [在eth0口模拟数据失真\(数据能传过去，但数据错误）](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem corrupt 0.1%
```

* [在eth0口模拟重新请求数据包](http://www.linux-foundation.org/en/Net:Netem)

```
tc qdisc change dev eth0 root netem gap 5 delay 10ms 
第五、十、十五个（每隔5个）包会立即发送，而其余的包会延时10ms再发送
```

```
tc qdisc change dev eth0 root netem delay 10ms reorder 25% 50% 
25％-50％的数据包会被立即发送，其余的延时10ms再发送，这种可以模拟乱序的情况
```

* [模拟传输速率](http://www.linux-foundation.org/en/Net:Netem)

```
# tc qdisc add dev eth0 root handle 1:0 netem delay 100ms
# tc qdisc add dev eth0 parent 1:1 handle 10: tbf rate 256kbit buffer 1600 limit 3000
```



