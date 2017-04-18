---
title: tomcat
date: 2017-04-18 15:35:52
category: 应用服务器
tags: Tomcat
---
启动多个
---
#### 修改http端口（默认8080）
``` bash
<Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="60000"  
	redirectPort="8443" disableUploadTimeout="false"  executor="tomcatThreadPool" URIEncoding="UTF-8"/>
```
#### 修改远程停服务端口（默认8005）
``` bash
	<Server port="8005" shutdown="SHUTDOWN"> 
```
#### 修改AJP端口（默认8009）
``` bash
	<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" /> 
```