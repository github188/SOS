## ![](/assets/SOS_logo_mini.png)

## 

## SOS一站式测试平台

SOS的诞生，拟大幅提高测试能力\(仿真\)与效率，让产品质量好的要命，让质量问题看见它喊救命，有志成为：SuperOS/SangforTestOS！

## **定位**

最终以系统交付产品一站式测试能力\(适用于网关型等产品的性能稳定性、功能测试、可靠性、自动化测试等\)，以促进产品质量、测试效率提升\(后续所有人员参与维护\)，以下为当前使用效果

| **维度** | **使用量** | **使用产品线** | **已服务产品** | **质量产出** | **口碑** | **备注** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 2018-11-22 | 783台 | PR/WOC/AC/AF/SSL/AD | VPN/WOC/DOCKER/AD等 | 127个有效问题(2018/02前) | 待更新 | 已在VPN稳定性改进、Docker项目、WOC9.5、WOC9.1.3、产品线合入稳定性项目中运用，且当前在CASB、多产品双机专项项目中流行起来 |
| 最新 | [实时数据](http://200.200.194.160/Pages/Report) 或 sos -p | 无 | 无 | [BUG产出](http://200.200.192.160/sangfor/SOS/SOS_ROI.xlsx) | 待更新 | 待更新 |



## **解决什么痛点？**

| **维度** | **痛点** |  | **长期** |
| :--- | :--- | :--- | :--- |
| WOC | CIFS/FTP/HTTP/HTTPS等多ip并发协议代理无有效方案测试\(ATP不支持协议栈、历史Docker方案资源消耗高\) | 复杂网络环境构建\(如VLAN、WCCP、链路聚合、动态路由环境等\) | 一站式测试，已支持，待更新 |
| PR | VPN TCP/UDP高并发测试、VPN并发接入测试\(PR开发提供工具\) | Docker项目并发多IP负载测试 |  [VPN一站式测试](http://200.200.192.160/sangfor/SOS/VPN.xlsx)  |
| ALL | 4-7层并发性能测试 | 常规测试环境构建 | 适用网关型产品测试,已支持，待更新 |
| 公共 | 高端测试资源稀缺、真实协议测试不支持 | Windows不适合做性能测试机、自动化难度高、常用测试手段效率极低+分散 | 一站式自动化执行主机(已支持)、性能测试平台(已支持) |

## SOS特点

| **维度** | **特点** |
| :--- | :--- |
| 启动时间 | 极快，30S |
| 磁盘 | 小，最低2G |
| 内存 | 小，最低256M |
| 兼容性 | HCI/ESXi/公司主流硬件 |
| 性能 | 高 |

## SOS安装与更新

本gitlab项目并非全包，使用SOS需要先安装初始版本(路径如下),然后按照以下方式进行一键更新


| **安装** | **下载** | **说明** |
| :--- | :--- | :--- |
| 虚拟化 | [SOSOVA](http://200.200.192.160/sangfor/SOSOVA.ova) | 导入HCI或者ESXi即可 |
| 母盘 | [SOS母盘](http://200.200.192.160/sangfor/SOSMUPAN.zip) | 使用再生龙程序或sos的sos-clone进行Ghost |

| **Docker部署** | **命令** |
| :--- | :--- |
| 镜像拉取 |  docker pull 200.200.1.230/sos-docker/sos-docker:sos-docker |
| 容器启动 |  docker run --net=host --privileged=true -it 200.200.1.230/sos-docker/sos-docker:sos-docker |

<table>
  <tr>
    <th><b>更新</b></th>
    <th><b>方法</b></th>
    <th><b>说明</b></th>
  </tr>
  <tr>
    <td>首次更新 </td>
    <td>curl http://200.200.192.160/install.sh |bash</td>
    <td>更新不会重启设备</td>    
  </tr>
  <tr>
    <td>后续更新</td>
    <td>sos update</td>
    <td>养成sos update的习惯，能让您的SOS保持更好体验及惊喜更新</td>    
  </tr>
</table>

| **帮助文档** | **链接** |
| :--- | :--- |
| 在线文档 | [SOS GitBook](https://testsos.gitbooks.io/sos/content/) |
| 离线文档 | [SOS.PDF](http://200.200.192.160/sangfor/sos.pdf) |

## 定时任务

SOS除了可以管理SOS自己的定时任务，还可以用来调度Windows/Linux进行定时任务，只要把以下程序在对应系统运行，在SOS中加入管理即可！

| **系统** | **下载** | **说明** |
| :--- | :--- | :--- |
| Linux | [下载](http://200.200.192.160/sangfor/SOS/gocron/gocron-node) | ./gocron-node & |
| Windows | [下载](http://200.200.192.160/sangfor/SOS/gocron/gocron-node.exe) |  gocron-node.exe |


## 版本更新

欢迎加入SOS TEAM，成为SOS开发者，将使您的工作发生重大改变！

| **版本号** | **发布时间** | **开发者** | **功能支持** |
| :--- | :--- | :--- | :--- |
| SOS \(v0.0.1 Beta\) | 2017-07-06 | 周兴喜、朱永波、仰桥 | [功能支持](http://200.200.192.160/sangfor/SOS/SOSv0.0.1_Beta.xls) |
| SOS \(v0.0.2 Beta\) | 2017-08-20 | 周兴喜、朱永波、刘宜雄 | 待更新 |
| SOS \(v0.0.2 Release\) | 2017-11-13 | 周兴喜、朱永波、刘宜雄、潘猛、陈国斌、陈凤燕、张玉杨、李秀玉、吴玉婷、李圣悦 | 待更新 |
| SOS \(v0.0.3 Beta\) | 2017-11-15 | 周兴喜、朱永波、潘猛、陈国斌、段永强 | 待更新 |




