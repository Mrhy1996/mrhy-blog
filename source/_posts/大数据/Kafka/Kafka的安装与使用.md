---
title: Kafka的安装与使用
toc: true
date: 2021-05-10 15:00:52
tags: kafka
url: kafka-install
---

最近，因为工作原因，要去学习kafka，利用闲余时间搭建了一个kafka的单机demo，现在记录一下。

<!--more-->

# 什么是Kafka

Kafka是由[Apache软件基金会](https://baike.baidu.com/item/Apache软件基金会)开发的一个开源流处理平台，由[Scala](https://baike.baidu.com/item/Scala/2462287)和[Java](https://baike.baidu.com/item/Java/85979)编写。Kafka是一种高吞吐量的[分布式](https://baike.baidu.com/item/分布式/19276232)发布订阅消息系统，它可以处理消费者在网站中的所有动作流数据。 它主要结合了三个功能

1. To **publish** (write) and **subscribe to** (read) streams of events, including continuous import/export of your data from other systems.

   发布订阅事件流，包括不断的从其他系统中导入或者导出数据

2. To **store** streams of events durably and reliably for as long as you want.

   持久可靠的存储事件流，存储时间由你去决定

3. To **process** streams of events as they occur or retrospectively.

   处理事件流的发生或者追溯

# Kafka中的术语和概念

**event**：An **event** records the fact that "something happened" in the world or in your business. It is also called record or message in the documentation. When you read or write data to Kafka, you do this in the form of events. Conceptually, an event has a key, value, timestamp, and optional metadata headers. Here's an example event:

- Event key: "Alice"
- Event value: "Made a payment of $200 to Bob"
- Event timestamp: "Jun. 25, 2020 at 2:06 p.m."

一个事件记录着一个事实，即业务中发生过什么。通常在文档中，也被成为message或者记录。当你在kafka中度或者写数据的时候，你会按照这个事件的格式去做。通常来讲，一个事件通常有一个key，value，时间戳和元数据的操作头。

**Producers**：生产者

**consumers**：消费者

**topic**：Events are organized and durably stored in **topics**. Very simplified, a topic is similar to a folder in a filesystem, and the events are the files in that folder. An example topic name could be "payments". Topics in Kafka are always multi-producer and multi-subscriber: a topic can have zero, one, or many producers that write events to it, as well as zero, one, or many consumers that subscribe to these events. Events in a topic can be read as often as needed—unlike traditional messaging systems, events are not deleted after consumption. Instead, you define for how long Kafka should retain your events through a per-topic configuration setting, after which old events will be discarded. Kafka's performance is effectively constant with respect to data size, so storing data for a long time is perfectly fine.

==事件被有条理和持久性的被存储在topic中，简单的来讲，topic类似于文件系统中的文件夹，事件相当于文件夹中的文件。在kafka中的topics通常有多个生产者和多个订阅者。一个topic通常有0个，1个甚至更多的生产者，同样的，也会有0个，1个，甚至多个消费者去订阅这些事件。与传统的消息系统相比，事件当被消费之后不会被删除。你可以定义多久才会被删除（默认7天）==

**partitioned**：Topics are **partitioned**, meaning a topic is spread over a number of "buckets" located on different Kafka brokers. This distributed placement of your data is very important for scalability because it allows client applications to both read and write the data from/to many brokers at the same time. When a new event is published to a topic, it is actually appended to one of the topic's partitions. Events with the same event key (e.g., a customer or vehicle ID) are written to the same partition, and Kafka [guarantees](http://kafka.apache.org/documentation/#intro_guarantees) that any consumer of a given topic-partition will always read that partition's events in exactly the same order as they were written.

==topics被分区化，意味着一个topic被分成了多个partition，存放在不同的kafka broker上。==

# 安装单机版kafka

官方版本

## 下载并解压Kafka

https://www.apache.org/dyn/closer.cgi?path=/kafka/2.8.0/kafka_2.13-2.8.0.tgz

```bash
#解压
$ tar -xzf kafka_2.13-2.8.0.tgz
$ cd kafka_2.13-2.8.0
```

## 开启kafka环境

NOTE: Your local environment must have Java 8+ installed.

本地必须安装java 8+；

Run the following commands in order to start all services in the correct order:

开启zookeeper（下载下来的kafka中带zookeeper，或者你可以用自己的环境中的zk）

```bash
# Start the ZooKeeper service
# Note: Soon, ZooKeeper will no longer be required by Apache Kafka.
# 不久的将来，zookeeper已经不是kafka必须的环境了
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

打开另外一个命令窗口，运行如下命令

```bash
# Start the Kafka broker service
$ bin/kafka-server-start.sh config/server.properties
```

当所有的环境都启动之后，你的kafka的服务就算是启动起来了。

## 测试

创建一个topic

```bash
$ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```

生产者生产

```bash
$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```

打开另外一个terminal，打开启动消费者

```bash
$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```

在生产者发送消息，可以看到在消费者也会收到，至此，一个单机版的kafka已经搭建完成。

# Kafka配置文件详解

## server.properties

```properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# limitations under the License.

# see kafka.server.KafkaConfig for additional details and defaults
broker.id=0

#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://localhost:9092

# returned from java.net.InetAddress.getCanonicalHostName().
#advertised.listeners=PLAINTEXT://your.host.name:9092


num.network.threads=3

socket.send.buffer.bytes=102400

socket.request.max.bytes=104857600


############################# Log Basics #############################

# A comma separated list of directories under which to store log files
log.dirs=/Users/majiju/develop-tool/Kafka/Kafka1/logs
num.partitions=1

offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1

############################# Log Flush Policy #############################
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.

# The number of messages to accept before forcing a flush of data to disk
# The maximum amount of time a message can sit in a log before we force a flush
#log.flush.interval.ms=1000

# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168

log.segment.bytes=1073741824
# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:12181

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=18000


############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
group.initial.rebalance.delay.ms=0
```

我们来一个个的读配置文件中蓝色字体的注释

| 字段                                     | 解释                                                    |
| ---------------------------------------- | ------------------------------------------------------- |
| broker.id                                | 当前节点的id，集群中唯一                                |
| listeners=PLAINTEXT://localhost:9092     | 监听地址和端口，producter、consumer连接的端口，默认9092 |
| num.network.threads=3                    | broker处理消息的最大线程数，一般情况下数量为cpu核数     |
| socket.send.buffer.bytes=102400          | socket的发送缓冲区                                      |
| socket.request.max.bytes=104857600       | socket请求的最大字节数                                  |
| log.dirs                                 | 日志的目录                                              |
| num.partitions                           | 每个topic分区的个数                                     |
| offsets.topic.replication.factor         | 副本个数                                                |
| transaction.state.log.replication.factor |                                                         |
| transaction.state.log.min.isr            |                                                         |
| log.retention.hours=168                  | 日志保留的最长的时长，默认为7天                         |
| log.segment.bytes                        | 每个segment的大小                                       |
| zookeeper.connect                        | zk的连接地址                                            |
| zookeeper.connection.timeout.ms          | zk连接超时时间                                          |
| group.initial.rebalance.delay.ms         | 消费者分组协调者                                        |

## producer.properties

```properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# see org.apache.kafka.clients.producer.ProducerConfig for more details

############################# Producer Basics #############################

# list of brokers used for bootstrapping knowledge about the rest of the cluster
# format: host1:port1,host2:port2 ...
bootstrap.servers=localhost:9092

# specify the compression codec for all data generated: none, gzip, snappy, lz4, zstd
compression.type=none

# name of the partitioner class for partitioning events; default partition spreads data randomly
#partitioner.class=

# the maximum amount of time the client will wait for the response of a request
#request.timeout.ms=

# how long `KafkaProducer.send` and `KafkaProducer.partitionsFor` will block for
#max.block.ms=

# the producer will wait for up to the given delay to allow other records to be sent so that the sends can be batched together
#linger.ms=

# the maximum size of a request in bytes
#max.request.size=

# the default batch size in bytes when batching multiple records sent to a partition
#batch.size=

# the total bytes of memory the producer can use to buffer records waiting to be sent to the server
#buffer.memory=
```

| 字段               | 解释                                                         |
| ------------------ | ------------------------------------------------------------ |
| bootstrap.servers  | kafka集群列表，用于获取metadata，不必全部指定，多个用","分开 |
| compression.type   | 压缩类型  none，gzip，snappy，lz4，zstd                      |
| partitioner.class  | 分区策略，默认是随机分区                                     |
| request.timeout.ms | 客户端等待请求响应的最大超时时间                             |
| max.block.ms       | 最大的阻塞时长                                               |
| linger.ms          | 生产者将等待到给定的延迟，以允许其他记录被发送，以便发送可以批处理在一起 |
| max.request.size   | 最大的请求字节数                                             |
| batch.size         | 批处理的最大数                                               |
| buffer.memory      | 生产者能够使用的最大空间 （一起发送给server）                |

## consumer.properties

```properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# see org.apache.kafka.clients.consumer.ConsumerConfig for more details

# list of brokers used for bootstrapping knowledge about the rest of the cluster
# format: host1:port1,host2:port2 ...
bootstrap.servers=localhost:9092

# consumer group id
group.id=test-consumer-group

# What to do when there is no initial offset in Kafka or if the current
# offset does not exist any more on the server: latest, earliest, none
#auto.offset.reset=
```

| 字段              | 描述               |
| ----------------- | ------------------ |
| bootstrap.servers | 连接的server的地址 |
| group.id          | 分组id             |

