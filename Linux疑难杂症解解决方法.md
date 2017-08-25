---
title: Linux疑难杂症解决方法
date: 2017-04-17 10:47:21
category: Linux
tags: Linux问题解决方法
---
ssh远程主机被拒绝
---
```
epdc@epdc-ThinkPad-T550:~$ ssh localhost
ssh: connect to host localhost port 22: Connection refused
```
sudo apt-get install openssh-server

/etc/sudoers文件被破坏
---	
```
epdc@epdc-ThinkPad-T550:/etc$ sudo gedit sudoers
>>> /etc/sudoers: syntax error near line 1 <<<
>>> /etc/sudoers: syntax error near line 1 <<<
sudo：/etc/sudoers 中第 1 行附近有解析错误
sudo：no valid sudoers sources found, quitting
sudo：无法初始化策略插件
```
* 重启ubuntu，启动时按Esc或Shift键，可以看到引导选项；
* 在引导选项中选择Recovery模式的那一项来引导；
* 进入Recovery Menu页面，选择root，也就是进入试用root用户进行系统恢复，在这里可以执行超级用户的权限的操作，回车后可以看到熟悉的 root@user ~# 命令提示符；
* 设置或者撤销/etc/sudoers文件的权限，也可以将该文件改回到发生错误之前的状态。

    chmod 666 /dev/null
    mount -o remount rw /
    vi /etc/sudoers 
    恢复本文件内容并存盘
* 退出Recovery模式，重新启动ubuntu。

无法解析或打开软件包的列表或是状态文件解决方案
---
	E: Encountered a section with no Package: header
	E: Problem with MergeList /var/lib/apt/lists/cn.archive.ubuntu.com_ubuntu_dists_
	natty_main_i18n_Translation-en
sudo rm /var/lib/apt/lists/* -vf

apt-get安装出现错误
---
	E: Sub-process /usr/bin/dpkg returned an error code (1)
apt-get update
