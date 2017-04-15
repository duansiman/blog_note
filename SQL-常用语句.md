---
title: SQL  常用语句
date: 2017-04-06 15:14:32
tags: SQL语句
categories: 
- 数据库
- MySQL
---
MySQL
---
``` bash
	mysql --version		
		mysql版本
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
表
---
``` bash
	describe t_user		
		查看表结构
	alter table t_user modify id smallint(5) unsigned;	
		修改字段类型
```