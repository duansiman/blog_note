---
title: 搜索历史
date: 2018-10-13
category: Java
tags: EKL
---
Filebeat
---
如果output 只是logstash, 需要手动导入index template。 需要临时关闭logstash, 开启es
	
	./filebeat setup --template -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'

如果以root运行filebeat, 需要改变配置文件的分组用户
	
	sudo chown root filebeat.yml

Elasticsearch  
---
需要开启 automatic creation of X-Pack indices

	action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*

`不能以root用户启动es` 