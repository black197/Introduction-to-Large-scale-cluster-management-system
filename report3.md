# Experiment of Infrastructure & Messaging System

吴幸融 516030910253 Experiment to measure the metrics of stand-alone computer's storage and network

吴怜颐 516030910252 How to set up a Kafka cluster

张宇航 516030910273 A scenairo to demonstrate the streaming process


# Stand-alone Computer's Storage & Network Metrics Measurement

## Storage

### Key Indicators

OSD Throughput, Write Latency,Data Distribution and Scalability

Metadata Update Latency, Metadata Read Latency, Metadata Scaling

### Experiment

1) 根据硬盘品牌和型号查询厂商所给数据。

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/QQ%E6%88%AA%E5%9B%BE20181121174622.png?raw=true)

以TOSHIBA DT01ACA200为例，厂商官网和手册提供了尺寸、接口、容量、缓存大小、转速、最大传输速度、功率、重量等参数。

详见：https://toshiba-semicon-storage.com/cn/product/storage-products/client-hdd/dt01acaxxx.html

2) 使用测试软件测试硬盘在不同种类任务下的性能。

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/as-ssd-bench%20KXG50ZNV256G%20NVM%202018.11.21%2017-31-04.png?raw=true)

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/Untitled.jpg?raw=true)

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/21-______-2018_18-17.png?raw=true)

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

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/QQ%E6%88%AA%E5%9B%BE20181121182727.png?raw=true)

以Killer™ Wireless-AC 1535为例，厂商官网和手册提供了WLAN频率带宽、IEEE无线上网标准等数据。

详见：https://www.killernetworking.com/products/killer-wireless-ac-1535/#1538447688051-e5859717-920b

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/QQ%E6%88%AA%E5%9B%BE20181121183234.png?raw=true)

以华为WS550为例，厂商官网和手册提供了无线速率、无线频段、无线协议、无线安全、天线等数据。

2) 使用测试软件测试网络的性能。

i. 使用iperf工具测试单线程TCP

开启两个终端，分别输入：

iperf -s -p [server-port] -i 1

iperf -c [server-ip] -p [server-port] -i 1 -t [second(s)] -w 20K

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/QQ%E6%88%AA%E5%9B%BE20181121210950.png?raw=true)

可以模拟单线程客户端与单线程服务器之间的TCP网络，并计算出规定时间内的平均带宽。

ii. 使用iperf工具测试单线程UDP

开启两个终端，分别输入：

iperf -s -u -p [server-port] -i 1

iperf -c [server-ip] -p [server-port] -u -i 1 -t [second(s)]

![avatar](https://github.com/black197/Introduction-to-Large-scale-cluster-management-system/blob/milky/pics/QQ%E6%88%AA%E5%9B%BE20181121215203.png?raw=true)

可以模拟单线程客户端与单线程服务器之间的UDP网络，并计算出规定时间内的平均带宽。

参考教程：https://www.cnblogs.com/yingsong/p/5682080.html

## A scenairo to demonstrate the streaming process
##### 516030910273 张宇航

Kafka Stream的数据源只能是Kafka。但是处理结果并不一定要出到Kafka。

KStream和Ktable的实例化都需要指定Topic。
```
KStream<String, String> stream = builder.stream("words-stream");
KTable<String, String> table = builder.table("words-table", "words-store");
```
另外，Consumer和Producer并不需要开发者在应用中显示实例化，而是由Kafka Stream根据参数隐式实例化和管理，从而降低了使用门槛。开发者只需要专注于开发核心业务逻辑。

#### 接下来将用Low level API可实现精确的消息消费控制，并进一步使用Kafka Stream的Low-level Processor API实现word count

基于Kafka Stream的流式应用的业务逻辑全部通过一个被称为Processor Topology的地方执行。它与Storm的Topology和Spark的DAG类似，都定义了数据在各个处理单元（在Kafka Stream中被称作Processor）间的流动方式，或者说定义了数据的处理逻辑。

下面是Processor的实例，它实现了Word Count功能，并且每秒输出一次结果：
process定义了对每条记录的处理逻辑，也印证了Kafka可具有记录级的数据处理能力。  
context.scheduler定义了punctuate被执行的周期，从而提供了实现窗口操作的能力。  
context.getStateStore提供的状态存储为有状态计算（如窗口，聚合）提供了可能。

```
public class WordCountProcessor implements Processor<String, String> {

	private ProcessorContext context;
	private KeyValueStore<String, Integer> kvStore;

	@SuppressWarnings("unchecked")
	@Override
	public void init(ProcessorContext context) {
		this.context = context;
		this.context.schedule(1000);
		this.kvStore = (KeyValueStore<String, Integer>) context.getStateStore("Counts");
	}

	@Override
	public void process(String key, String value) {
		Stream.of(value.toLowerCase().split(" ")).forEach((String word) -> {...
	}

	@Override
	public void punctuate(long timestamp) {...
	}

	@Override
	public void close() {
		this.kvStore.close();
	}

}
```
#### 说明：
Kafka Stream的Processor Topology完全由Processor组成，因为它的数据固定由Kafka的Topic提供。  
Kafka Stream的Processor Topology的不同Processor完全运行于同一个Task中，也就完全处于同一个线程，无需网络通信。  
Kafka Stream的一个物理Topology只包含非Shuffle部分，而Shuffle部分需要通过through操作显示完成，该操作将一个大的Topology分成了2个子Topology。  
Kafka Stream的子Topology内，所有Processor的并行度完全一样。
Kafka Stream的一个Task包含了一个子Topology的所有Processor。

#### 总结：
Kafka Stream的并行模型完全基于Kafka的分区机制和Rebalance机制，实现了在线动态调整并行度  
同一Task包含了一个子Topology的所有Processor，使得所有处理逻辑都在同一线程内完成，避免了不必的网络通信开销，从而提高了效率。  
KTable的引入，使得聚合计算拥用了处理乱序问题的能力