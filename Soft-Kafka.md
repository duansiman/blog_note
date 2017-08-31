---
title: Kafka
date: 2017-08-17 14:49:57
tags: Kafka
categories: 
- Java
- 分布式
---
启动服务器
---
Kafka依赖ZooKeeper，Kafka有脚本可以安装并启动一个全新的单节点ZooKeeper实例
	
	bin/zookeeper-server-start.sh config/zookeeper.properties
启动Kafka服务器
	
	bin/kafka-server-start.sh config/server.properties
Topic
---
查看所有Topic
	
	bin/kafka-topics.sh --list --zookeeper localhost:2181

配置多节点集群
---
为每个broker准备配置文件config/server.properties, 修改下面信息
	
	broker.id=1
    listeners=PLAINTEXT://:9093
    log.dir=/tmp/kafka-logs-1
查看broker情况
	
	bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test_topic