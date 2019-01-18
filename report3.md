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