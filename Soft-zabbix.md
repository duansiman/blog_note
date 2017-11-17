---
title: zabbix
date: 2017-11-17 10:47:21
category: 运维
tags: zabbix
---

Problem
---

用docker-compose 安装zabbix, 一起安装的zabbix-agent用不了提示下面错误

`Received empty response from Zabbix Agent at [107.172.105.140]. Assuming that agent dropped connection because of access permission`

用docker logs zabbix_zabbix-agent_1看日志，发现拒绝信息
`20322:20171117:051807.440 failed to accept an incoming connection: connection from "172.19.0.1" rejected, allowed hosts: "127.0.0.1"`

需要在docker 数据卷挂载地方zbx_env/etc/zabbix/zabbix_agentd.d/ 创建zabbix_agentd.conf，把拒绝ip加入文件就可以了

	Server=172.19.0.1

