---
layout:     post
title:      kafka SparkStreaming
---
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
								            <div id="content_views" class="markdown_views prism-atom-one-dark">
							<!-- flowchart 箭头图标 勿删 -->
							<svg xmlns="http://www.w3.org/2000/svg" style="display: none;"><path stroke-linecap="round" d="M5,0 0,2.5 5,5z" id="raphael-marker-block" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);"></path></svg>
							<p>发送的时候：kafka client 包里的KafkaProducer，发送的ProducerRecord<br>
接受的时候：spark.streaming.kafka包里的，0.10版本，KafkaUtils.createDirectStream</p>
<p>Topic：kafka里的AdminUtils.createTopic</p>
<p>kafka client 包里的KafkaConsumer和KafkaProducer</p>
<p>sparkstreaming和kafka集成的两种方式<br>
-1,基于接收者的方法</p>
<p>算子：KafkaUtils.createStream<br>
方法：PUSH，从topic中去推送数据，将数据推送过来<br>
API：调用的Kafka高级API<br>
效果：SparkStreaming中的Receivers，恰好Kafka有发布/订阅 ，然而：此种方式企业不常用，说明有BUG，不符合企业需求。因为：接收到的数据存储在Executor的内存，会出现数据漏处理或者多处理状况<br>
解释：这种方法使用Receiver来接收数据。Receiver是使用Kafka高级消费者API实现的。与所有的接收者一样，通过Receiver从Kafka接收的数据存储在Spark执行程序exector中，然后由Spark Streaming启动的作业处理数据。但是，在默认配置下，这种方法可能会在失败时丢失数据。为了确保零数据丢失，您必须在Spark Streaming（在Spark 1.2中引入）中额外启用写入日志，同时保存所有接收到的Kafka数据写入分布式文件系统（例如HDFS）的预先写入日志，以便所有数据都可以在失败时恢复。<br>
缺点：<br>
①、Kafka中的主题分区与Spark Streaming中生成的RDD的分区不相关。因此，增加主题特定分区KafkaUtils.createStream()的数量只会增加在单个接收器中使用哪些主题消耗的线程的数量。在处理数据时不会增加Spark的并行性<br>
②、多个kafka输入到DStream会创建多个group和topic，用于使用多个接收器并行接收数据<br>
③、如果已经使用HDFS等复制文件系统启用了写入日志，则接收到的数据已经在日志中复制。因此，输入流的存储级别为存储级别StorageLevel.MEMORY_AND_DISK_SER</p>
<p>-2,直接方法（无接收者）</p>
<p>算子：KafkaUtils.createDirectStream<br>
方式：PULL，到topic中去拉取数据。<br>
API：kafka低级API<br>
效果：每次到Topic的每个分区依据偏移量进行获取数据，拉取数据以后进行处理，可以实现高可用<br>
解释：在Spark 1.3中引入了这种新的无接收器“直接”方法，以确保更强大的端到端保证。这种方法不是使用接收器来接收数据，而是定期查询Kafka在每个topic+分partition中的最新偏移量，并相应地定义要在每个批次中处理的偏移量范围。当处理数据的作业启动时，Kafka简单的客户API用于读取Kafka中定义的偏移范围（类似于从文件系统读取文件）。请注意，此功能在Spark 1.3中为Scala和Java API引入，在Spark 1.4中针对Python API引入。<br>
优势：<br>
①、简化的并行性：不需要创建多个输入Kafka流并将其合并。与此同时directStream，Spark Streaming将创建与使用Kafka分区一样多的RDD分区，这些分区将全部从Kafka并行读取数据。所以在Kafka和RDD分区之间有一对一的映射关系，这更容易理解和调整。</p>
<p>②、效率：在第一种方法中实现零数据丢失需要将数据存储在预写日志中，这会进一步复制数据。这实际上是效率低下的，因为数据被有效地复制了两次，一次是由Kafka，另一次是由预先写入日志（Write Ahead Log）复制。此方法消除了这个问题，因为没有接收器，因此不需要预先写入日志。只要你有足够的kafka保留，消息可以从kafka恢复。</p>
<p>③、精确语义：第一种方法是使用Kafka的高级API在Zookeeper中存储消耗的偏移量。传统上这是从Kafka消费数据的方式。虽然这种方法（合并日志）可以确保零数据丢失，但在某些失败情况下，很小的几率两次数据都同时丢失，发生这种情况是因为Spark Streaming可靠接收到的数据与Zookeeper跟踪的偏移之间的不一致。因此，在第二种方法中，我们使用不使用Zookeeper的简单Kafka API。在其检查点内，Spark Streaming跟踪偏移量。这消除了Spark Streaming和Zookeeper / Kafka之间的不一致性，因此Spark Streaming每次记录都会在发生故障时有效地接收一次。</p>
<p>请注意，这种方法的一个缺点是它不会更新Zookeeper中的偏移量，因此基于Zookeeper的Kafka监控工具将不会显示进度。但是，您可以在每个批次中访问由此方法处理的偏移量，并自己更新Zookeeper</p>
<p><a href="https://www.cnblogs.com/xlturing/p/6246538.html" rel="nofollow">https://www.cnblogs.com/xlturing/p/6246538.html</a></p>

            </div>
						<link href="https://csdnimg.cn/release/phoenix/mdeditor/markdown_views-9e5741c4b9.css" rel="stylesheet">
                </div>