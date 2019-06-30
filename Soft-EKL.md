---
title: 搜索历史
date: 2018-10-13
category: Java
tags: EKL
---
Filebeat
---
### 功能 

是日志数据转发或汇集的搬运工，把你配置日志文件或者目录，把这些内容转发ES或Logstash，它的工作原理：
![](Soft-EKL/filebeat1.png)

### 配置

它的配置文件filebeat.yml

#### input

指定日志文件的路径，例如：/var/log/*/*.log 会处理/var/log的子目录下以.log结尾的文件，但不会处理/var/log下日志文件

	filebeat.inputs:
	- type: log
	  enabled: true
	  paths:
	    - /var/log/*.log

#### output

指定转发目的地，例如：输出到ES中

	output.elasticsearch:
  	hosts: ["myEShost:9200"]

#### 安全验证配置
es 或 kibana
	
	output.elasticsearch:
	  hosts: ["myEShost:9200"]
	  username: "filebeat_internal"
	  password: "YOUR_PASSWORD" 
	setup.kibana:
	  host: "mykibanahost:5601"
	  username: "my_kibana_user"  
	  password: "YOUR_PASSWORD"

### Logstash input
使用Logstash 对日志进行附加处理

	output.logstash:
  	  hosts: ["127.0.0.1:5044"]

### index template
index template 定义设置或映射 决定了如何分析字段

#### es output
对于output是es, filebeat会自动加载index template.

##### 修改默认index template

	setup.template.name: "your_template_name"
	setup.template.fields: "path/to/fields.yml"

##### 覆盖已存在的template

	setup.template.overwrite: true

##### 失效自动template加载
	
	setup.template.enabled: false

##### index name

filebeat 是将event 写入filebeat-7.1.1-yyyy.MM.dd 索引名。 修改索引名：

	output.elasticsearch.index: "customname-%{[agent.version]}-%{+yyyy.MM.dd}"
	setup.template.name: "customname"
	setup.template.pattern: "customname-*"

#### 手动导入 index template

如果output 只是logstash, 需要手动导入index template。 需要临时关闭logstash, 开启es
	
	./filebeat setup --template -E output.logstash.enabled=false -E 'output.elasticsearch.hosts=["localhost:9200"]'

如果运行Filebeat的机器没有链接es,需要导出index template,去链接es的主机上安装index template

	导出index template
	./filebeat export template > filebeat.template.json
	
	安装index template
	curl -XPUT -H 'Content-Type: application/json' http://localhost:9200/_template/filebeat-7.1.1 -d@filebeat.template.json

导入index template, 删除旧的使kibana看到最新的

	curl -XDELETE 'http://localhost:9200/filebeat-*' 

### set up kibana

	./filebeat setup --dashboards

使用Lagstash 作为output 使用下面命令

	./filebeat setup -e \
	  -E output.logstash.enabled=false \
	  -E output.elasticsearch.hosts=['localhost:9200'] \
	  -E output.elasticsearch.username=filebeat_internal \
	  -E output.elasticsearch.password=YOUR_PASSWORD \
	  -E setup.kibana.host=localhost:5601

### 运行
如果以root运行filebeat, 需要改变配置文件的分组用户
	
	sudo chown root filebeat.yml

	运行
	sudo ./filebeat -e

### module

filebeat 提供内置的module方便快速实现日志的监控和数据的可视化展示，例如：Nginx, Apache2, and MySQL

安装 module
	
	./filebeat modules enable system nginx mysql

查看 module

	./filebeat modules list

设置初始环境， setup 命令加载推荐的index template, -e将错误输出到标准错误上，而不是syslog
	
	./filebeat setup -e

> The ingest pipelines used to parse log lines are set up automatically the first time you run the module, assuming the Elasticsearch output is enabled. If you’re sending events to Logstash, or plan to use Beats central management, also see Load ingest pipelines manually.
> 
>./filebeat setup --pipelines --modules system,nginx,mysql

#### 设置module的path

modules.d目录里yml文件

	- module: nginx
  	  access:
        var.paths: ["/var/log/nginx/access.log*"]

命令行设置
	
	./filebeat -e -M "nginx.access.var.paths=[/usr/local/var/log/nginx/access.log*]"

### 工作原理

filebeat 有两个主要组件 inputs 和 harvesters

#### 什么是harvesters?

负责读取文件的内容，发送到指定的output

#### 什么是input?

它负责管理所有的harvesters, 还有找到所有资源去读取它们。

#### filebeat 怎么保持文件的状态？

filebeat会维持每一个文件的状态，并且会定期把状态保存到注册文件中。

#### filebeat 怎么保证至少一次交付？

filebeat 会把每个事件的交付状态保存到注册文件中

Elasticsearch  
---
需要开启 automatic creation of X-Pack indices

	action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*

`不能以root用户启动es` 