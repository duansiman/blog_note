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

##### config

修改服务器的配置，不需要重启服务器

```
config get slow*
```

## 事务

通过multi、exec、watch等命令来实现事务；一次性、按顺序地执行多个命令的机制；在事务执行期间，服务器不会中断事务而改去执行其他客户端的命令请求

##### 开启事务

客户端从非事务状态切换至事务状态

```
multi
```

##### 命令入事务队列

以先进先出（FIFO）的方式保存入队的命令

##### 提交事务

遍历事务队列，执行队列中保存的所有命令

```
exec
```

##### watch命令

EXEC命令执行之前，监视数据库键；在EXEC命令执行时，检查被监视的键是否被修改过，如果是，服务器将拒绝执行事务

```
watch key
```

##### 事务的ACID

###### 原子性

事务队列中的命令要么全部都执行，要么都不执行

```
命令入队出错而被服务器拒绝执行
```

```
不支持事务回滚机制，命令在执行期间出现了错误，整个事务也会继续执行下去
```

###### 一致性

数据库在执行事务之前是一致的，在事务执行之后，无论事务是否执行成功，数据库仍然是一致的

```
“一致”指的是数据符合数据库本身的定义和要求，没有包含非法或者无效的错误数据
```

- 入队错误，拒绝执行这个事务
- 执行错误，出错命令不会对数据库做任何修改
- 服务器宕机，服务器所使用的持久化模式情况不同（RDB、AOF），但数据恢复后是一直的

###### 隔离性

以串行的方式运行的，并且事务也总是具有隔离性的

###### 持久性

事务所得的结果已经被保存到永久性存储介质，即使服务器在事务执行完毕之后停机，执行事务所得的结果也不会丢失

```
Redis事务的耐久性由Redis所使用的持久化模式决定
```

no-appendfsync-on-rewrite配置

```
执行BGSAVE命令或者BGREWRITEAOF命令期间，服务器会暂时停止对AOF文件进行同步，从而尽可能地减少I/O阻塞
```



## 二进制位数组

setbit、getbit、bitcount、bitop四个命令处理二进制位数组

##### 位数组的表示

使用字符串对象来表示位数组，因为字符串对象使用的SDS数据结构是二进制安全的

```
buf[0] 表示一个字节长的位数组，低位到高位存储，buf空间扩展时，不需要移到现有位
```

##### 二进制位统计算法

###### 遍历算法

###### 查表算法

表的键为某种排列的位数组，而表的值则是相应位数组中，值为1的二进制位的数量

```
空间换时间策略
```

###### variable-precision SWAR算法

统计一个位数组中非0二进制位的数量，在数学上被称为“计算汉明重量（Hamming Weight）”

```
思想：通过一系列位移和位运算操作，可以在常数时间内计算多个字节的汉明重量
```

```c
// 32位长度位数组的SWAR算法实现
uint32_t swar(uint32_t i) {
    i = (i & 0x55555555) + ((i >> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >> 2) & 0x33333333);
    i = (i & 0x0F0F0F0F) + ((i >> 4) & 0x0F0F0F0F);
    i = (i & 0x01010101) >> 24;
    return i;
}
```

一次循环中多次执行swar，从而按倍数提升计算汉明重量的效率。一旦循环中处理的位数组的大小超过了缓存的大小，这种优化的效果就会降低并最终消失

###### Redis 实现

用到了查表和variable-precision SWAR两种算法

```
查表使用键长为8位的表，SWAR算法：一次循环载入128位，调用四次swar
```

选择条件

```
二进制位的数量大于等于128使用SWAR算法，否则使用查表
```

监视器
---

实时地接收并打印出服务器当前处理的命令请求

	monitor

## 慢查询日志

记录执行时间超过给定时长的命令请求

##### slowlog-log-slower-than配置

指定执行时间超过多少微秒的命令请求会被记录到日志上

##### slowlog-max-len配置

服务器最多保存多少条慢查询日志

```
服务器使用先进先出的方式保存多条慢查询日志
```

例子：

```sh
config set slowlog-log-slower-than 0
config set slowlog-max-len 5
```

##### slowlog get

查看慢日志

```
4) 1) (integer) 1626					#日志的唯一标识符
   2) (integer) 1612063185				#命令执行时间unix时间戳
   3) (integer) 10						#命令执行时长 微妙
   4) 1) "config"						#命令以及参数
      2) "set"
      3) "slowlog-log-slower-than"
      4) "10"
   5) "172.22.0.2:58656"				#客户端ip和port
   6) ""								#客户端名字
```

