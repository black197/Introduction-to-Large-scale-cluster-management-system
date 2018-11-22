# 使用JAVA调用发展Kafka生产者和消费者
## java端生产数据， kafka集群消费数据
1. 创建maven工程，pom.xml中增加如下：
```
    <dependency>
		<groupId>org.apache.kafka</groupId>
		<artifactId>kafka_2.10</artifactId>
		<version>0.8.2.0</version>
	</dependency>
```
2. Kafka生产者端java代码，zookeeper中已有topic“test”：
```
import java.util.Properties;
import java.util.concurrent.TimeUnit;

import kafka.javaapi.producer.Producer;
import kafka.producer.KeyedMessage;
import kafka.producer.ProducerConfig;
import kafka.serializer.StringEncoder;




public class kafkaProducer extends Thread{

	private String topic;
	
	public kafkaProducer(String topic){
		super();
		this.topic = topic;
	}
	
	
	@Override
	public void run() {
		Producer producer = createProducer();
		int i=0;
		while(true){
			producer.send(new KeyedMessage<Integer, String>(topic, "message: " + i++));
			try {
				TimeUnit.SECONDS.sleep(1);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}

	private Producer createProducer() {
		Properties properties = new Properties();

        //声明zookeeper
		properties.put("zookeeper.connect", 
        "{zookeeper's ip}:2181");
		properties.put("serializer.class", StringEncoder.class.getName());

        // 声明kafka broker
		properties.put("metadata.broker.list",
         "{kafka1's ip}:{kafka1's port},
         {kafka2's ip:{kafka2's port},
         {kafka3's ip:kafka3's port}");

		return new Producer<Integer, String>(new ProducerConfig(properties));
	 }
	
	
	public static void main(String[] args) {
		new kafkaProducer("test").start();// 使用kafka集群中创建好的主题 test 
		
	}
	 
}
```
3. 在kafka集群中运行消费者，并制定topic“test”：
```
bin/kafka-console-consumer.sh --zookeeper {zookeeper's ip}:2181 --topic test --from-beginning
```
4. 启动java代码，则Kafka集群消费者可看到如下数据：
```
message: 0
message: 1
message: 2
message: 3
message: 4
message: 5
message: 6
message: 7
message: 8
message: 9
message: 10
message: 11
message: 12
message: 13
message: 14
message: 15
message: 16
message: 17
message: 18
message: 19
message: 20
message: 21
```
## java端生产数据，java端消费数据
1. Kafka消费者端java代码，同使用zookeeper中已有topic“test”：
```
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;

import kafka.consumer.Consumer;
import kafka.consumer.ConsumerConfig;
import kafka.consumer.ConsumerIterator;
import kafka.consumer.KafkaStream;
import kafka.javaapi.consumer.ConsumerConnector;




public class kafkaConsumer extends Thread{

	private String topic;
	
	public kafkaConsumer(String topic){
		super();
		this.topic = topic;
	}
	
	
	@Override
	public void run() {
		ConsumerConnector consumer = createConsumer();
		Map<String, Integer> topicCountMap = new HashMap<String, Integer>();
		topicCountMap.put(topic, 1); // 一次从主题中获取一个数据
		 Map<String, List<KafkaStream<byte[], byte[]>>>  messageStreams = consumer.createMessageStreams(topicCountMap);
		 KafkaStream<byte[], byte[]> stream = messageStreams.get(topic).get(0);// 获取每次接收到的这个数据
		 ConsumerIterator<byte[], byte[]> iterator =  stream.iterator();
		 while(iterator.hasNext()){
			 String message = new String(iterator.next().message());
			 System.out.println("接收到: " + message);
		 }
	}

	private ConsumerConnector createConsumer() {
		Properties properties = new Properties();

		//声明zookeeper
		properties.put("zookeeper.connect", 
        "{zookeeper's ip}:2181");

        //生产者和消费者需要在不同组，即使用不同的组名
		properties.put("group.id", "group1");

		return Consumer.createJavaConsumerConnector(new ConsumerConfig(properties));
	 }
	
	
	public static void main(String[] args) {
		new kafkaConsumer("test").start();// 使用kafka集群中创建好的主题 test 
		
	}
	 
}
```
2. 先运行kafkaProducer ，在运行kafkaConsumer，即可得到生产者的数据：
```
接收到: message: 10
接收到: message: 11
接收到: message: 12
接收到: message: 13
接收到: message: 14
```