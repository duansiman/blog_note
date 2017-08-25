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

脚本启动
---
	
	/etc/init.d/redis_6379 start

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

错误
---

``` bash
redis.clients.jedis.exceptions.JedisConnectionException: Unknown reply: N
```

