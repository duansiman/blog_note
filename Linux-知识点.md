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
执行运行文件
---
#### bin文件

	chmod +x xxx.bin
	./xxx.bin -d <install path>

#### deb文件

	dpkg -i

物理CPU、逻辑CPU和CPU核数
---

#### 物理CPU
插槽上的CPU个数,物理cpu数量，可以数不重复的 physical id 有几个。
#### 逻辑CPU
/proc/cpuinfo 文件,用来存储cpu硬件信息的.列出了processor 0 – n 的规格,n不是真实的cpu数
一颗cpu可以有多核，加上intel的超线程技术(HT), 可以在逻辑上再分一倍数量的cpu core
逻辑CPU数量=物理cpu数量 x cpu cores 这个规格值 x 2(如果支持并开启ht)

	Linux下top查看的CPU也是逻辑CPU个数
#### 查看物理CPU的个数
	cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l
#### 查看逻辑CPU的个数
	cat /proc/cpuinfo |grep "processor"|wc -l
#### 查看CPU是几核
	cat /proc/cpuinfo |grep "cores"|uniq
查看内存、CPU使用情况
---
可用内存=系统free memory+buffers+cached

	top 					查看内存和CPU	
	pmap -d processId		查看进程相关信息占用的内存情况
	free -m 				查看内存
	cat /proc/meminfo		查看内存额定值
	ll -h /proc/kcore		查看内存镜像文件的大小

无法解析或打开软件包的列表或是状态文件解决方案
---
	E: Encountered a section with no Package: header
	E: Problem with MergeList /var/lib/apt/lists/cn.archive.ubuntu.com_ubuntu_dists_
	natty_main_i18n_Translation-en
sudo rm /var/lib/apt/lists/* -vf

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

系统使用技巧
---
	
* 执行某命令时忘记加sudo, 可以输入sudo !! 即可，这里的 !! 代表上一条命令
