# 11. Stream Processing
In reality, a lot of data is unbounded because it arrives gradually over time. In general, a “stream” refers to data that is incrementally made available over time. In a stream processing context, a record is more commonly known as an event. An event is generated once by a producer (also known as a publisher or sender), and then potentially processed by multiple consumers (subscribers or recipients). Related events are usually grouped together into a topic or stream.

A common approach for notifying consumers about new events is to use a messaging system: a producer sends a message containing the event, which is then pushed to consumers. A messaging system allows multiple producer nodes to send messages to the same topic and allows multiple consumer nodes to receive messages in a topic.

When multiple consumers read messages in the same topic, two main patterns of messaging are used:
- Load balancing: Each message is delivered to one of the consumers
- Fan-out: Each message is delivered to all of the consumers.

The two patterns can be combined: for example, two separate groups of consumers may each subscribe to a topic, such that each group receives all messages, but within each group only one of the nodes receives each message. 

In order to ensure that the message is not lost, message brokers use acknowledgments: a client must explicitly tell the broker when it has finished processing a message so that the broker can remove it from the queue. 

























