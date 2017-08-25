---
title: SQL语句说明
date: 2017-04-06 15:14:32
tags: SQL语句
categories: 
- 数据库
- MySQL
---
MySQL
---
数据库数据导入导出
``` bash
	mysqldump -u用户名 -p密码  数据库名 > 导出的文件名 
		导出整个数据库
	mysqldump -u用户名 -p密码  数据库名 表名> 导出的文件名 
		导出一个表，包括表结构和数据
	mysqldump -u用户名 -p密码 -d 数据库名 > e:\sva_rec.sql 
		导出一个数据库结构
	mysqldump -u用户名 -p 密码 -d数据库名  表名> 导出的文件名
		导出一个表，只有表结构 
	source sql文件
		导入数据库
```
连接数及状态信息

	show status like '%connect%'
		show status  查看所有状态参数，Threads_connected 当前的连接数，Connections 试图连接，max_used_connections并发过过最大数量
	show processlist;
		显示当前正在执行的mysql连接
	mysqladmin -u -p -h status
		显示当前mysql状态
	mysqladmin -u -p -h extended-status
		显示mysql的其他状态
配置
	
	show variables like 'max_allowed_packet';
		查看MySQL变量
把查询结果导出到文件中
	
	mysql -h 127.0.0.1 -u root -p XXXX -P 3306 blog -Ne "select * from table"  > /tmp/test/txt
	-N 输出到文件内容不带表字段名

表
---
``` bash
	describe t_user		
		查看表结构
	alter table t_user modify id smallint(5) unsigned;	
		修改字段类型
```
查看表结构

	desc 表名;
	show columns from 表名;
	show create table 表名;

	use information_schema
	select * from columns where table_name='表名'

函数
---
	show function status; 				查看自定义函数
	show create function fun_name；		查看函数源码
