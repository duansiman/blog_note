---
title: Linux-运维
date: 2017-05-05 18:01:21
category: Linux
tags: Linux运维
---
修改SSH端口和禁止Root远程登录
---
修改端口

	/etc/ssh/sshd_config文件中
	修改SSH端口，修改port后的端口
禁止Root远程登录：
	
	把PermitRootLogin yes 改为 PermitRootLogin no
	重启服务，service ssh restart

流量监控
---
#### vnstat
安装

	apt-get install vnstat
创建监听数据库

	vnstat -u -i eth0	->	eth0是系统网卡
启动服务

	service vnstat start
开机自启动，在/etc/rc.local文件中加：

	service vnstat start
查看流量情况命令

	每天的，vnstat -d
	每月的，vnstat -m	
	实时流量，vnstat -l -i eth0
	查看五秒平均流量，vnstat -tr -i eth0

不能统计流量，需要修改.eh0权限，在/var/lib/vnstat目录中

	chown vnstat:vnstat .eth0 -R
	chmod 0640 .eth0

rz，sz命令
---
sz：将选定的文件发送（send）到本地机器
rz：运行该命令会弹出一个文件选择窗口，从本地选择文件上传到服务器(receive)

`注意：`单独用rz会有两个问题：上传中断、上传文件变化（md5不同），解决办法是上传是用rz -be，并且去掉弹出的对话框中“Upload files as ASCII”前的勾选。
	
	-b binary 用binary的方式上传下载，不解释字符为ascii
	-e 强制escape 所有控制字符，比如Ctrl+x，DEL等
安装
	
	下载地址	http://www.ohse.de/uwe/software/lrzsz.html
	配置		./configure --prefix=/usr/local/lrzsz
	编译		make
	安装		make install
创建连接	

	cd /usr/bin  
	sudo ln -s /usr/local/lrzsz/bin/lrz rz  
	sudo ln -s /usr/local/lrzsz/bin/lsz sz 

/boot分区不足,清理boot分区
---
查看系统已安装内核	
	
	dpkg --get-selections|grep linux-image

查看系统使用内核

	uname -a	

删除不使用内核
	
	sudo apt-get purge 内核名称
	sudo apt-get remove linux-image-(版本号)
清理

	清理/usr/src下的内核目录
	sudo apt-get autoremove 删除残留文件

xshell连接linux系统，操作卡
---
关闭DNS解析，修改/etc/ssh/sshd_config文件中：
	
	UseDNS no
	/etc/init.d/sshd restart

系统使用技巧
---
	
* 执行某命令时忘记加sudo, 可以输入sudo !! 即可，这里的 !! 代表上一条命令
