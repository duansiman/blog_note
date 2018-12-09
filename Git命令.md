---
title: Git
date: 2017-05-05 17:03:22
category: Git
tags: Git
---
	git init		初始化 git 仓库
	git fsck		仓库一致性检查
	git gc			压缩
	git describe 	显示提交易记名称(最近的里程碑名字 tag_name-<n>-ID)
	git mv <file> <file>	改名操作
git rebase
---
对提交执行变基操作，实现将指定范围的提交嫁接到另外一个提交之上
git archive
---
归档打包
	
	git archive -o <name>.zip HEAD
	git archive -o <name>.zip HEAD <path>...
	git archive --format=tar --prefix=1.0/ v1.0 | gzip > foo-1.0.tar.gz

使用tar格式和提交ID或里程碑ID，ID会记录在文件的文件头中，可以用下面命令获取提交ID

	git tar-commit-id

忽略文件或目录 export-ignore
	
.gitignore
---
作用范围是其在的目录及其子目录
	
	git status --ignored		可以看到忽略的文件
	git add -f				强制添加忽悠的文件

`独享式`忽悠
* 在.git/info/exclude来设置文件忽略(针对具体版本库)
* 配置变量core.excludesfile设置文件忽略（所有本地版本库）

忽略语法
	
	* 代表任意多字符
	? 一个字符
	/path 忽略此目录下文件，非子目录
	path/ 忽略整个目录，包括子目录
	!<file/path> 不忽略

git add
---
	git add -u		把工作区文件加到暂存区（被版本库追踪的）
	git add -A		把工作区文件加到暂存区（包括未被版本库追踪的）
	git add -i		选择性添加到暂存区
git status
---
根据文件时间戳，长度判断文件是否改变，如果时间戳改变，再看内容是否改变。`文件内容保存再.git/objects目录下`

|参数|描述|
|:---|:---|
|-s|查看精简状态，第一列M表示：版本库和暂存区的文件比较，第二列M表示：工作区和暂存区|
|-b|显示当前分支名|

git commit
---
	git commit -e -F .git/COMMIT_EDITIMSG		重新提交(.git/COMMIT_EDITIMSG 是上次提交日志	)
	git commit -a -m 				偷懒提交，不用git add
	git commit --amend -m				修改最新提交说明
	git commit -C 提交ID			重用提交的提交说明
	
git log
---
默认是指HEAD

|参数|描述|
|:---|:---|
|--stat|查看提交文件变更记录|
|--oneline,--pretty=oneline|查看精简日志，前面显示提交哈希值前几位，后面显示全部|
|--graph|查看提交关系图(树结构显示)|
|--pretty=raw|显示commit的原始数据（parent属性）|
|--pretty=fuller|显示作者和提交者|
|--decorate|显示提交关联引用|
|-<n>|最近几条日志|
|-p|显示文件具体改动|

	
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
git diff
---
默认是把工作区和暂存区的文件做比较
	
	git diff HEAD	把工作区和版本库的文件做比较
	git diff --cached/--staged		把暂存区和版本库的文件做比较
	git diff 提交ID1 提交ID2
	git diff 提交ID1 提交ID2 --<paths>
--word-diff 对每个词比较

git checkout
---
默认是暂存区，覆盖工作区文件

	git checkout ./--file		撤销工作区全部文件或某个文件未提交的修改
	git checkout HEAD ./<file>	用版本库的全部或部分文件替换暂存区和工作区中的文件
创建和切换到新的分支
	
	git checkout <new_branch> <branch>
	git checkout -b test       基于当前分支新建分支test，并切换到test分支下

切换到某个提交，是分离头指针，head指向具体提交
	
	git checkout 提交ID

汇总工作区、暂存区、与HEAD的差异
	
	git checkout

git reset
---
没有指定提交，默认是HEAD

	git reset HEAD <paths>		把版本库的全部文件替换暂存区的
git reset 

	--hard HEAD^		替换暂存区和工作区的全部文件(重置引用指向)
	--soft HEAD^		不改变暂存区和工作区内容(仅重置引用指向)
	--mixed HEAD^		重置暂存区，不改变工作区（重置引用执行，默认是这个参数）
git rm
---
	git rm --cached <file> 从暂存区删除文件，不影响工作区
git ls-tree、ls-files
---
	git ls-tree -l 		HEAD 查看版本库的目录树（-l显示文件大小）
	git ls-files -s 	查看暂存区的目录树（第三个字段表示暂存区编号）
	git ls-files --with-tree=HEAD^	查看历史版本的文件列表
git write-tree可以把暂存区的目录树写入git对象库，然后用git ls-tree查看。如果是tree对象，可以加上-r -t参数显示tree树内容	
git clean
---
	git clean -fd		清除未加入版本库的文件和目录
	git clean -nd		强制删除目录和文件

git stash
---
保存工作区和暂存区的修改，然后改动全不见了。
	
	git stash	保存进度
	save 指定说明
	--patch 显示工作区和HEAD的差异
	-k 不会重置暂存区

查看所以进度
	
	git stash list
	git reflog show refs/stash
从最近保存的进度进行恢复
	
	git stash pop	会删除进度
	[--index] 恢复工作区，且尝试恢复暂存区
	git stash apply 不会删除进度
其他命令
	
	git stash clear 清除所有进度
	git stash branch <branch name> <stash> 基于这个进度创建分支

git对象
---
对象保存在.git/objects目录下，对象的40位sha1哈希值前2位作为目录，后38作为文件名

git cat-file

	git cat-file -t 查看对象类型
	git cat-file -p	查看对象内容
	git cat-file -p HEAD^:file > file	从某个提交中恢复文件
	git show HEAD^:file > file

符号
	
	^			指父提交
	^+数字		表示第几个父提交
	~+数字		指祖父提交
	^{对象}		表示提交对应的对象
	提交ID:file	对应提交的文件
	:file		暂存区的文件
git reflog
---
.git/logs记录分支的变更，查看git config core.logallrefupdates是否开启。
	
	git reflog show master	查看变更记录(<ref>@{<n>}引用之前第几次改变)

git rev-parse
---	
	git rev-parse HEAD		查看引用对应sha1哈希值
	git rev-parse HEAD:file

|参数|描述|
|:---|:---|
|--symbolic --branches|显示分支|
|--symbolic --tags|显示里程碑|
|--git-dir|显示版本库位置|
|--show-cdup|工作区目录的深度|
|--symbolic --glob=refs/*|显示所有引用|
|:/"commit a"|在提交日志中查找提交|

git rev-list
---
|参数|描述|
|:---|:---|
|--oneline A|显示A版本库以来所有的历史提交|
|--oneline D F|提交历史并集|
|--oneline ^D F（D..F）|排除D的历史提交|
|--oneline D...F|排除共同的历史提交|
|--oneline D^@|D的历史提交，自身除外|
|--oneline D^!|提交本身，不包括历史|
git blame
---
显示文件内容，包括每行提交版本，提交者
	
	-L n, m 从第n行开始，显示m行


授权
---
ssh-keygen -t rsa					生成公钥密钥
git config
---
参数
	
	--global 全局设置
属性
	
	user.email				邮箱
	user.name				名字
	alias.<name> '命令'		设置别名
	core.editor "vim"		默认情况下 git 用的编辑器是 vi，设置Editor使用vim
	color.ui true			开启给 Git 着色
	core.quotepath false	显示中文文件名
	http.postBuffer 524288000 增加Git’s HTTP buffer                                                                                                            

默认这些配置都在 ~/.gitconfig，可以输入 git config -l 命令查看

Proxy

	git config --global http.proxy 'socks5://127.0.0.1:1080'
	git config --global https.proxy 'socks5://127.0.0.1:1080'

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
	git branch --set-upstream-to=origin/<branch> master
		set tracking information for this branch
版本
---
	git tag							查看版本
	git tag v1.0						贴标签，版本控制

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
	
