---
title: Linux 命令
date: 2017-06-17 10:47:21
category: Linux
tags: Linux命令
---

命令通用参数
---
### -R
表示递归,例如:
	
	cp -R 递归复制
	chmod -R 递归修改权限

### -m, -h
以M,K显示大小
	
	ls -lh	查看当前目录下文件大小
	df -m	查看磁盘空间

---
系统相关的
---
系统版本

	lsb_release -a
	cat /etc/issue
	getconf LONG_BIT		查看系统位数
	uname -a

防火墙

	ufw status	
	ufw disable
	ufw enable

Ctrl+Alt+F1-F6 	命令行模式
Ctrl+Alt+F7	回到桌面
Ctrl+Alt+T 	打开终端

文档管理相关的
---
/etc/group文件中, 查看用户分组信息.

### 7z命令
安装方法：

	sudo apt-get install p7zip

解压文件：

	7z x manager.7z -r -o /home/xx
	x 代表解压缩文件，并且是按原始目录解压（还有个参数 e 也是解压缩文件，但其会将所有文件都解压到根下，而不是自己原有的文件夹下）manager.7z 是压缩文件，这里大家要换成自己的。如果不在当前目录下要带上完整的目录
	-r 表示递归所有的子文件夹
	-o 是指定解压到的目录，这里大家要注意-o后是没有空格的直接接目录

压缩文件

	7z a -t7z -r manager.7z /home/manager/*
	a 代表添加文件／文件夹到压缩包
	-t 是指定压缩类型 一般我们定为7z
	-r 表示递归所有的子文件夹，manager.7z 是压缩好后的压缩包名，/home/manager/* 是要压缩的目录，＊是表示该目录下所有的文件。
### 文件解压和压缩
zip文件

	zip -r path 		压缩
	unzip xxx.zip -d path	解压
tar文件

	tar -cvf  xxx.tar /xxx			打包不压缩
	tar -zcvf xxx.tar.gz /xxx		打包gzip压缩
	tar -jcvf xxx.tar.bz2 /xxx		打包bzip2压缩
7z文件
	
	看7z命令

rar文件
	
	sudo apt-get install rar	安装
	rar x xxx.rar			解压
	rar a xxx.rar xxx/		压缩
	

### cp
	-R, -r, --recursive   	递归复制
	-p			连同档案的属性一起复制过去，而非使用预设属性

### 执行运行文件
bin文件

	chmod +x xxx.bin
	./xxx.bin -d <install path>

deb文件

	dpkg -i

命令本身相关的
---

type 命令来查看命令的类型,路径

http网络相关的
---
### curl
在Linux中curl是一个利用URL规则在命令行下工作的文件传输工具，可以说是一款很强大的http命令行工具。它支持文件的上传和下载，是综合传输工具，但按传统，习惯称url为下载工具。

	语法：# curl [option] [url]

	常见参数：
	-A/--user-agent <string>              设置用户代理发送给服务器
	-b/--cookie <name=string/file>    cookie字符串或文件读取位置
	-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中
	-C/--continue-at <offset>            断点续转
	-D/--dump-header <file>              把header信息写入到该文件中
	-e/--referer                                  来源网址
	-f/--fail                                          连接失败时不显示http错误
	-o/--output                                  把输出写到该文件中
	-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名
	-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围
	-s/--silent                                    静音模式。不输出任何东西
	-T/--upload-file <file>                  上传文件
	-u/--user <user[:password]>      设置服务器的用户和密码
	-w/--write-out [format]                什么输出完成后
	-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理
	-#/--progress-bar                        进度条显示当前的传送状态

系统进程相关
---

### top

	PID：进程的ID
	USER：进程所有者
	PR：进程的优先级别，越小越优先被执行
	NInice：值
	VIRT：进程占用的虚拟内存
	RES：进程占用的物理内存
	SHR：进程使用的共享内存
	S：进程的状态。S表示休眠，R表示正在运行，Z表示僵死状态，N表示该进程优先值为负数
	%CPU：进程占用CPU的使用率
	%MEM：进程使用的物理内存和总内存的百分比
	TIME+：该进程启动后占用的总的CPU时间，即占用CPU使用时间的累加值。
	COMMAND：进程启动命令名称
第一行

	系统当前时刻、系统启动到现在的运作时间、终端的数目、当前系统负载的平均值，1分钟前、5分钟前、15分钟前进程的平均数
第二行

	进程总数、运行中的进程数、等待状态中的进程数、被停止的系统进程数、被复原的进程数

时间
--
date
	
	-R 		以RFC 2822 format查看日期和时间，可以看到时区

其他命令
---
wc

	wc -l	统计行数
	wc -c	统计字节数
	wc -m	统计字符数
	wc -w	统计词数
file
查看文件类型
