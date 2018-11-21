# Experiment of Infrastructure & Messaging System

吴幸融 516030910253 Experiment to measure the metrics of stand-alone computer's storage and network

叶东諴 516030910020 ??

吴怜颐 516030910252 How to set up a Kafka cluster

张宇航 516030910273 ??


# Stand-alone Computer's Storage & Network Metrics Measurement

## Storage

### Key Indicators

OSD Throughput, Write Latency,Data Distribution and Scalability

Metadata Update Latency, Metadata Read Latency, Metadata Scaling

### Experiment

1) 根据硬盘品牌和型号查询厂商所给数据。

以TOSHIBA DT01ACA200为例，厂商官网和手册提供了尺寸、接口、容量、缓存大小、转速、最大传输速度、功率、重量等参数。

详见：https://toshiba-semicon-storage.com/cn/product/storage-products/client-hdd/dt01acaxxx.html

2) 使用测试软件测试硬盘在不同种类任务下的性能。

将读写的数据段的长短和数据位置的连续或随机作为变量组合成不同情况下的测试，模拟的是不同的常见情况。

Seq：例如拷贝电影等，连续存储的大文件。

4k单线程：程序启动初期。windows启动时，有100多个进程要启动，它们每一个都是一个4k单线程，加在一起有没有64线程同时读盘不知道，但性能应该介于4k单线程和4k-64线程之间。

4k-64线程：作为服务器。每个用户只需要查询很少的数据（例如图书馆查查书目）但用户多，并发量大。

（以AS SSD Benchmark，ATTO Disk Benchmark和HD Tune Pro为例）

## Network

### Key Indicators

bit rate, bandwidth, throughput, latency/delay, RTT, utilization

### Experiment

1) 根据网卡和路由器品牌和型号查询厂商所给数据。

以Killer™ Wireless-AC 1535为例，厂商官网和手册提供了WLAN频率带宽、IEEE无线上网标准等数据。

详见：https://www.killernetworking.com/products/killer-wireless-ac-1535/#1538447688051-e5859717-920b

以华为WS550为例，厂商官网和手册提供了无线速率、无线频段、无线协议、无线安全、天线等数据。

2) 使用测试软件测试网络的性能。

i. 使用iperf工具测试单线程TCP

开启两个终端，分别输入：

iperf -s -p [server-port] -i 1

iperf -c [server-ip] -p [server-port] -i 1 -t [second(s)] -w 20K

可以模拟单线程客户端与单线程服务器之间的TCP网络，并计算出规定时间内的平均带宽。

ii. 使用iperf工具测试单线程UDP

开启两个终端，分别输入：

iperf -s -u -p [server-port] -i 1

iperf -c [server-ip] -p [server-port] -u -i 1 -t [second(s)]

可以模拟单线程客户端与单线程服务器之间的UDP网络，并计算出规定时间内的平均带宽。

参考教程：https://www.cnblogs.com/yingsong/p/5682080.html