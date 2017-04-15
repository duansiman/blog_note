---
title: Hexo 搭建个人博客
date: 2017-03-31 14:33:13
category: Blog
tags: Hexo
---
准备环境
---
使用[Hexo](https://hexo.io/zh-cn/)搭建个人博客，需要安装[Node.js](http://nodejs.org/)和[Git](http://git-scm.com/)，最后安装Hexo

``` bash
npm install -g hexo-cli
```

建站
---
生产博客网站需要的资源，随便指定一个目录名字
``` bash
hexo init <folder>
cd <folder>
npm install
```
执行命令，出现错误Error: Cannot find module '/root/hexo_blog/node_modules/dtrace-provider/scripts/install.js'，可能是Node.js版本兼容，可以低版本。我的安装环境：
``` bash
$ hexo -v
hexo: 3.2.2
hexo-cli: 1.0.2
os: Windows_NT 6.1.7601 win32 x64
http_parser: 2.7.0
node: 6.9.5
v8: 5.1.281.89
uv: 1.9.1
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 57.1
modules: 48
openssl: 1.0.2k
```
关联GitHub
---
把博客网站资源放到Github上，这样可以通过互联网访问。在GitHub上创建一个仓库，名字为youname.github.io，必须是这格式。然后在hexo资源目录下的_config.yml文件最后加上：
``` bash
deploy: 
    type: git
    repository: https://github.com/duansiman/duansiman.github.io.git
    branch: master
```
最后生产资源文件，部署到GitHub上。
``` bash
hexo g
hexo d
```
访问地址duansiman.github.io
发表文章
---
执行如下命令,会生成md文件。然后生成资源文件，部署到Github上。
``` bash
hexo new "article title"
```
推荐编辑md文件工具[MarkdownPad](http://markdownpad.com/)，它的[注册码](http://www.jianshu.com/p/9e5cd946696d)