# 11. Stream Processing
In reality, a lot of data is unbounded because it arrives gradually over time. In general, a “stream” refers to data that is incrementally made available over time. In a stream processing context, a record is more commonly known as an event. An event is generated once by a producer (also known as a publisher or sender), and then potentially processed by multiple consumers (subscribers or recipients). Related events are usually grouped together into a topic or stream.

A common approach for notifying consumers about new events is to use a messaging system: a producer sends a message containing the event, which is then pushed to consumers. A messaging system allows multiple producer nodes to send messages to the same topic and allows multiple consumer nodes to receive messages in a topic.

When multiple consumers read messages in the same topic, two main patterns of messaging are used:
- Load balancing: Each message is delivered to one of the consumers
- Fan-out: Each message is delivered to all of the consumers.

The two patterns can be combined: for example, two separate groups of consumers may each subscribe to a topic, such that each group receives all messages, but within each group only one of the nodes receives each message. 

In order to ensure that the message is not lost, message brokers use acknowledgments: a client must explicitly tell the broker when it has finished processing a message so that the broker can remove it from the queue. 

Log-based message brokers combine the durable storage approach of databases with the low-latency notification facilities of messaging. In order to scale to higher throughput than a single disk can offer, the log can be partitioned. Within each partition, the broker assigns a monotonically increasing sequence number, or offset, to every message. Such a sequence number makes sense because a partition is append-only, so the messages within a partition are totally ordered. There is no ordering guarantee across different partitions. Apache Kafka, Amazon Kinesis Streams, and Twitter’s DistributedLog are such log-based message brokers. Even though these message brokers write all messages to disk, they are able to achieve throughput of millions of messages per second by partitioning across multiple machines, and fault tolerance by replicating messages. To achieve load balancing across a group of consumers, instead of assigning individual messages to consumer clients, the broker can assign entire partitions to nodes in the consumer group.

In situations where messages may be expensive to process and you want to parallelize processing on a message-by-message basis, and where message ordering is not so important, the JMS/AMQP style of message broker is preferable. On the other hand, in situations with high message throughput, where each message is fast to process and where message ordering is important, the log-based approach works very well.

If you only ever append to the log, you will eventually run out of disk space. To reclaim disk space, the log is actually divided into segments, and from time to time old segments are deleted or moved to archive storage. In practice, the log can typically keep a buffer of several days’ or even weeks’ worth of messages.

If a consumer does fall too far behind and starts missing messages, only that consumer is affected; it does not disrupt the service for other consumers. This fact is a big operational advantage: you can experimentally consume a production log for development, testing, or debugging purposes, without having to worry much about disrupting production services. When a consumer is shut down or crashes, it stops consuming resources—the only thing that remains is its consumer offset.
















