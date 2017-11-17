---
title: MySQL 知识点
date: 2017-04-17 18:40:55
tags: MySQL知识点
categories: 
- 数据库
- MySQL
---
配置
---
配置文件在/etc/mysql/my.cnf,一些配置内容如下：

	max_connections		
		最大支持连接数，设置到500～1000比较合适
	wait-timeout		
		数据库连接闲置最大时间值，默认值为8小时 

URL 格式
---
	
	jdbc:mysql://[host][,failoverhost...][:port]/[database]
	?[propertyName1][=propertyValue1][&propertyName2][=propertyValue2]...
常用参数：

|参数名称|说明|默认值|
| :--- | :--- | :--- |
|useUnicode|是否使用Unicode字符集|false|
|characterEncoding|当useUnicode设置为true时，指定字符编码||
|autoReconnect|当数据库连接异常中断时，是否自动重新连接|false|
|autoReconnectForPools|是否使用针对数据库连接池的重连策略|false|
|failOverReadOnly|自动重连成功后，连接是否设置为只读|true|
|maxReconnects|autoReconnect设置为true时，重试连接的次数|3|
|initialTimeout|autoReconnect设置为true时，两次重连之间的时间间隔，单位：秒|2秒|
|connectTimeout|和数据库服务器建立socket连接时的超时，单位：毫秒。 0表示永不超时|0|
|socketTimeout|socket操作（读写）超时，单位：毫秒。 0表示永不超时|0|
|useAffectedRows|是否用受影响的行数替代查找到的行数来返回数据，例如：当修改的内容与原数据相同，mysql自己返回受影响行数为0，但jdbc/mybatis 返回的却是1，返回的是 sql语句 的匹配行数||
[官方参数配置参考](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-configuration-properties.html)

配置主从数据库
---
主ip:192.186.0.101,从ip:192.186.0.102
#### 主数据库配置
在/etc/mysql/my.cnf文件增加如下配置：

	[mysqld]
	#主数据库id
	server-id=1
	#需要同步的database
	binlog-do-db=blog
	#忽略同步的database
	binlog-ignore-db=mysql
	#开启二进制日志机制
	log-bin=/var/lib/mysql/binlog

查看日志信息，得到二进制日志的文件名(file)和位置信息（position）
	
	show master status

#### 导主数据库已存在的数据
主数据库导出数据
	
	mysqldump -uname -ppass database > database.sql

从数据库导入数据
	
	source database.sql

#### 从数据库配置
在/etc/mysql/my.cnf文件增加如下配置：

	[mysqld]
	#从数据库id，不能和主数据库的相同
	server-id=2

配置同步参数
	
	CHANGE MASTER TO 
	MASTER_HOST='192.186.0.101',
	MASTER_USER='root',
	MASTER_PASSWORD='123456',
	MASTER_LOG_FILE='binlog.000001',
	MASTER_LOG_POS=154;

MASTER_HOST 主数据库所在服务器ip
MASTER_USER,MASTER_PASSWORD链接主数据库的用户密码
MASTER_LOG_FILE,MASTER_LOG_POS是从主数据库查到日志信息

启动同步进程
	
	start slave
	show slave status\G
		查看进程状态
进程状态有两个值，都为YES表示配置成功
	
	Slave_IO_Runing 负责去主数据库读取二进制日志，并写入从数据库的中继日志
	Slave_SQL_Runing 负责把中继日志转换sql执行

允许外网访问
---
注释/etc/mysql/mysql.conf.d/mysqld.cnf中 
	
	bind-address	= 127.0.0.1
查询用户
	
	use mysql;
	select user,host from user;

授权任意主机都可以以用户root和密码pass连接
	
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'pass' WITH GRANT OPTION;
	flush privileges;

授权192.186.0.102主机都可以以用户root和密码pass连接

	GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.186.0.102' IDENTIFIED BY 'pass' WITH GRANT OPTION;
	flush privileges;

编码问题
---
创建数据库表时指定编码和排序编码
	
	CREATE DATABASE `test`  CHARACTER SET 'utf8'  COLLATE 'utf8_general_ci'; 
	CREATE TABLE `test` () ENGINE=InnoDB DEFAULT CHARSET=utf8;
查看默认编码

	 show variables like "%char%";

设置编码
	
	SET character_set_client='utf8';  
	SET character_set_connection='utf8';  
	SET character_set_results='utf8';    

设置默认编码

	set names utf8;

在/etc/mysql/my.cnf配置默认编码
	
	[mysqld]
	default_character_set=utf8;

问题
---
	--initialize specified but the data directory has files in it. Aborting.
把/var/lib/mysql目录中文件删掉
	
	ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement
flush privileges;
	
	ERROR 3009 (HY000): Column count of mysql.user is wrong. Expected 45, found 42. Created with MySQL 50557, now running 50719. Please use mysql_upgrade to fix this error.
mysql_upgrade --force -uroot -p
	
	ERROR 1054 (42S22): Unknown column 'Password' in 'field list'
update user set authentication_string=password('1111') where user='root';
	
	ERROR 1682 (HY000): Native table 'performance_schema'.'session_variables' has the wrong structure
sudo mysql_upgrade -u root -p, then restart mysql

	ERROR 1467 (HY000): Failed to read auto-increment value from storage engine
auto-increment到最大值了。
use information_schema;
select * from TABLES where TABLE_NAME='t_article_label';

函数
---
#### 字符串连接函数
concat(str1,str2)
	
	有一个参数为null,就返回null
CONCAT_WS(separator,str1,str2,...)
	
	以分隔符separator链接字符，如果有参数为null,跳过这个参数。
group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])
	
	这是一个聚合函数，把有多个值的字段连接起来成一行。	

#### 时间函数
from_unixtime()/unix_timestamp()
	
	把Unix时间戳转换为日期/把日期转换为Unix时间戳
inet_aton()/inet_ntoa()
	
	用无符号整数存储IP地址，把IP转换成整数/把整数转换成IP
date +/- interval expr unit
	
	日期计算，例如：一天之前，select now() - interval 24 hour;

#### 字符截取函数

left(str, len)
	
	截取字符串中前len个字符

