---
title: Linux 知识点
date: 2017-04-17 10:47:21
category: Linux
tags: Linux知识点
---
源列表
---
Linux存放源有两个地方：/etc/apt/sources.list和/etc/apt/sources.list.d/*.list，后一个通常用来安装第三方的软件
sources.list文件内容：

	开头是deb或者deb-src，通过.deb文件进行安装和通过源文件的方式进行安装。
	URL后面的字符串，分别对应相应的目录结构。进入dists目录，就看到URL后目录结构
	
查看端口占用
---
	lsof -i:端口号							
		查看占用该端口的进程
	netstat -ntlup | grep 端口号		  		
		根据端口搜索进程
	ps -ef | grep 进程号						
		查看进程
	netstat –apn							
		查看所有的进程和端口使用情况
	kill -9 processId						
		杀进程


物理CPU、逻辑CPU和CPU核数
---

物理CPU

	插槽上的CPU个数,物理cpu数量，可以数不重复的 physical id 有几个。
逻辑CPU

	/proc/cpuinfo 文件,用来存储cpu硬件信息的.列出了processor 0 – n 的规格,n不是真实的cpu数
	一颗cpu可以有多核，加上intel的超线程技术(HT), 可以在逻辑上再分一倍数量的cpu core
	逻辑CPU数量=物理cpu数量 x cpu cores 这个规格值 x 2(如果支持并开启ht)

	Linux下top查看的CPU也是逻辑CPU个数
查看物理CPU的个数
	
	cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l
查看逻辑CPU的个数
	
	cat /proc/cpuinfo |grep "processor"|wc -l
查看CPU是几核
	
	cat /proc/cpuinfo |grep "cores"|uniq
查看内存、CPU使用情况
---
可用内存=系统free memory+buffers+cached

	top 					查看内存和CPU	
	pmap -d processId		查看进程相关信息占用的内存情况
	free -m 				查看内存
	cat /proc/meminfo		查看内存额定值
	ll -h /proc/kcore		查看内存镜像文件的大小



apt-get清除文件
---
下面命令已清mysql相关文件

	sudo apt-get purge mysql*
	sudo apt-get autoremove
	sudo apt-get autoclean

Ubuntu系统升级
---

	sudo apt-get update && sudo apt-get dist-upgrade
	sudo reboot
	sudo update-manager –d

配置root权限
---
/etc/sudoers文件中配置了，那些用户可以临时获取root权限
	
	授权给单个用户：
		devin	ALL=(ALL)ALL
	解释：
		devin：允许使用 sudo 的用户名
		ALL：允许从任何终端（任何机器）使用 sudo
		(ALL)：允许以任何用户执行 sudo 命令
		ALL：允许 sudo 权限执行任何命令

	让用户 test 只能在本主机（主机名为epdc）以 root 账户执行/bin/chown、/bin/chmod 两条命令
		test epdc-ThinkPad-T550=(root)/bin/chown,/bin/chmod

	授权某组用户，将“用户名”换成“%组名”即可。

	使用时会要求你输入当前用户的密码，系统确实输入正确即以 root 权限来执行命令，接下来一段时间（默认为5分钟）再次使用 sudo 命令就不需要输密码了

添加用户
---
使用useradd或adduser命令，在centos是一样的，而ubuntu下useradd命令没有创建用户主目录

外部命令
---


	shell 是一个交互式的应用程序，在执行外部命令时通过 fork 来创建一个子进程，再通过 exec 来加载外部命令的程序来执行，但是如果一个命令是 shell 内置命令，那么只能直接由 shell 来运行

	使用 sudo 获得root shell 的权限，然后在root shell 中执行该命令。进入root shell 很简单，输入sudo bash 确认本用户的密码即可，此时你会发现命令提示符显示当前是 root。一旦获得root shell，你可以执行任何命令而不需要在每条命令前输入sudo了。

sudo 操作记录日志
---

	sudo的日志功能就可以用户跟踪用户输入的命令，这不仅能增进系统的安全性，还能用来进行故障检修
	
	第一步：创建/var/log/sudo.log文件
		sudo touch /va/log/sudo.log
	第二步：修改/etc/rsyslog.conf配置文件,加入下面一行：
		local2.debug	/var/log/sudo.log	#空白不能用空格，必须用tab
	第三步：修改/etc/sudoers配置文件,加入下面一行：
		Defaults	logfile=/var/log/sudo.log
	第四步：重启syslog服务
		sudo service rsyslog restart

sudo命令
---
sudo 是linux下常用的允许普通用户使用超级用户权限的工具，允许系统管理员让普通用户执行一些或者全部的root命令，如halt，reboot，su等等。这样不仅减少了root用户的登陆和管理时间，同样也提高了安全性。Sudo不是对shell的一个代替，它是面向每个命令的。它的特性主要有这样几点： 
* sudo能够限制用户只在某台主机上运行某些命令。
* sudo提供了丰富的日志，详细地记录了每个用户干了什么。它能够将日志传到中心主机或者日志服务器。
* sudo使用时间戳文件来执行类似的“检票”系统。当用户调用sudo并且输入它的密码时，用户获得了一张存活期为5分钟的票（这个值可以在编译的时候改变）。
* sudo的配置文件是sudoers文件，它允许系统管理员集中的管理用户的使用权限和使用的主机。它所存放的位置默认是在/etc/sudoers，属性必须为0440。visudo来对该文件进行修改。强烈推荐使用该命令修改 sudoers


