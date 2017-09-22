## 场景篇 - CCTV大并发仿真

---

#### 说明：

```
模拟实时监控视频流，SOS作为服务端，已合入cctv-server，需要手动启动服务
```

#### 用法：

**\#CCTV服务端**

```
cctv-server
```

**\#CCTV客户端**

```
用法1-命令行方式  : cctv-loader [tpfh] -i interface -s start_ip -d ftp-server
用法2-指定配置文件：cctv-loader -f file
例子1-跑50并发连接：cctv-loader -t 50 -i eth0 -s 7.7.7.7 -d 9.9.9.9
例子2-指定配置文件：cctv-loader -f /sos/case/cctv-loader.conf
选项：
    -t threads       并发线程数,默认值为1,当起始网段末位ip超过255后,从下一网段继续叠加,注意不要网关ip冲突
    -i interface     业务网口,eg: eth0
    -s start_ip      起始ip,eg: 7.7.7.77
    -d destination   目标url ip
    -p share_ip_num  共享ip数，eg：-t 100 -p 10，表示用10个ip跑100并发，解决ip资源不足
    -f file          指定配置文件
    -h help          帮助
```

**\#范例1：跑50并发，起始ip为7.7.7.7，目标服务其为9.9.9.9，使用接口eth1**

```
cctv-loader -t 50 -i eth1 -s 7.7.7.7 -d 9.9.9.9
```

**\#范例2：用2个共享ip跑60并发，解决ip资源不足**

```
cctv-loader -t 50 -i eth1 -s 7.7.7.7 -d 9.9.9.9 -p 2
```

**\#范例3：复用自带模板**

```
cp /sos/case/cctv-loader.conf /sos/case/cctv.conf
cctv-loader -f /sos/case/cctv.conf
```

**\#注意事项  **

虚拟ip会从起始ip开始叠加，超过255后会从下一网段继续，注意不要和网关ip冲突



