---
title: Solr
date: 2017-09-26 14:49:57
tags: Solr
category: Java
---
命令
---
启动服务
	
	./solr start -force
关闭服务
	
	./solr stop -all
导入示例core

	./solr -e techproducts -force
创建solr库(core)
	
	solr create -c <name>
