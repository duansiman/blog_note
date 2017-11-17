---
title: Redis 学习
date: 2017-04-07 14:38:41
tags: Redis
categories: 
- 数据库
- Redis
---
Redis安装
---
	/bin/sh: 1: cc: not found
需要安装gcc

	fatal error: jemalloc/jemalloc.h: No such file or directory
博客http://blog.csdn.net/bugall/article/details/45914867

	You need tcl 8.5 or newer in order to run the Redis test
可以不运行redis test, tcl官网http://www.linuxfromscratch.org/blfs/view/cvs/general/tcl.html

启动

	redis-server start	
	/etc/init.d/redis_6379 start
停止
	
	redis-cli shutdown

Redis代码访问
---
	java.lang.ClassNotFoundException: redis.clients.jedis.GeoUnit
jedis包版本问题

	Cannot get Jedis connection; nested exception is redis.clients.jedis.exceptions.JedisConnectionException: 
	Could not get a resource from the pool
	java.net.SocketTimeoutException: connect timed out
redis配置文件，绑定了id，把bind 127.0.0.1注释掉

Redis 命令学习
---
	(error) NOAUTH Authentication required.
通过auth输入密码，auth "yourpassword"

删除所有的key
	
	redis-cli keys "*" | xargs redis-cli del
	redis-cli -a 123456 keys "*" | xargs redis-cli -a 123456 del
		需要指定密码
	redis-cli -n 0 keys "*" | xargs redis-cli -n 0 del
		指定删除那个库，默认0
	flushdb		
		删掉当前数据库的所有key
	flushall


错误
---

``` bash
redis.clients.jedis.exceptions.JedisConnectionException: Unknown reply: N
```
