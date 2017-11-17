---
title: Zookeeper
date: 2017-04-26 14:49:57
tags: Zookeeper
categories: 
- Java
- 分布式
---
配置(conf/zoo.cfg)
---
启动时会找zoo.cfg文件作为默认配置文件,所以复制一份zoo_sample.cfg

	tickTime：服务器之间或客户端与服务器之间维持心跳的时间间隔，每个 tickTime 时间就会发送一个心跳
	dataDir ：保存数据的目录，将写数据的日志文件也保存在这个目录里
	clientPort：监听这个端口，接受客户端的访问请求
启动命令

	./zkServer.sh start

客户端
---
#### 启动
	./zkCli.sh -server localhost:2181

占用8080端口
---
zookeeper有个内嵌的管理控制台是通过jetty启动，也会占用8080端口。可以启动脚本中修改
	
	admin.serverPort=port 
或者停止这服务

	admin.enableServer=false

问题
---
	ruok is not executed because it is not in the whitelist.
配置那些命令可用 4lw.commands.whitelist=stat, ruok, conf, isro		配所有命令可用 4lw.commands.whitelist=*
