---
title: Git
date: 2017-05-05 17:03:22
category: Git
tags: Git
---
	git status		查看状态
	git init		初始化 git 仓库
	git fsck		仓库一致性检查
	git gc			压缩
日志
---	
	git log 查看日志
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
授权
---
ssh-keygen -t rsa					生成公钥密钥
设置
---
设置全局账号

	git config --global user.email "you@example.com"
	git config --global user.name "Your Name"
  							        
设置账号

	git config  user.email "you@example.com"
	git config  user.name "Your Name"
                                               
设置别名，即 git co == git checkout
          	
	git config --global alias.co checkout   # 别名
	git config --global alias.psm 'push origin master'
                                              
默认情况下 git 用的编辑器是 vi，设置Editor使用vim                  
	
	git config --global core.editor "vim"
                                                                
开启给 Git 着色
	
	git config --global color.ui true
                                  
设置显示中文文件名                              
	
	git config --global core.quotepath false
                                       
增加Git’s HTTP buffer                         
	
	git config http.postBuffer 524288000    
                                                                

默认这些配置都在 ~/.gitconfig，可以输入 git config -l 命令查看

提交
---
	git diff                                    			
		提交前查看有哪些改动
	git add							
		提交的文件
	git commit -m ‘注释’					
		提交
	git add & git commit					
		把改动添加到一个「暂存区」然后提交，防止误提交
	git rm --cached						
		移除未提交的缓存
分支
---
	git branch					        	
		查看下本地分支情况
	git branch -a						
		查看所有分支，包括本地和远程
	git branch -r						
		查看远程分支
	git branch test						
		新建分支test
	git branch -d test					
		删除分支test
	git branch -D test					
		强制删除分支test，如果test分支没有整合
版本
---
	git tag							查看版本
	git tag v1.0						贴标签，版本控制
切换
---
	git checkout test					切换分支
	git checkout origin/test				把远程分支test，拉到本地
	git checkout -b test					新建分支test，并切换到test分支下
	git checkout v1.0					切换到每个标签，即版本
恢复
---
	git checkout a.md					
		把原文件还原，只能撤销还没有 add 进暂存区的文件
	git checkout file1 file2 ... fileN 			
		一次回滚多个文件，中间用空格隔开即可
	git checkout . 						
		直接回滚当前目录一下的所有working tree内的修改，会递归扫描当前目录下的所有子目录
合并
---
	git merge	test						合并分支， 在master分支执行这命令
同步
---
	git push origin master				本地代码推到远程 master 分支
	git push -f                               			强推，覆盖相同的内容
	git pull origin master				远程最新的代码更新到本地
	git clone url						把项目 clone 到了本地
关联
---
	git remote add origin url				与 GitHub 上的项目进行关联,origin 是给这个项目的远程仓库起的名字
	git remote remove <name>             	取消关联
	git remote rm 分支					删除关联
	git remote -v						当前项目有哪些远程仓库	