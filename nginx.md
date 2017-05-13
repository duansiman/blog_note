---
title: nginx
date: 2017-04-18 11:34:07
category: 应用服务器
tags: nginx
---
启动、停止和重新加载服务
---
	nginx
	nginx -s stop
	nginx -s reload
	nginx -s quit		退出
	nginx -t 			检查配置文件
配置（/etc/nginx/nginx.conf、/etc/nginx/conf.d/default.conf）
---
	worker_processes
		进程的个数，建议设置为CPU的个数
	worker_connections
		单进程的最大连接数
	server
		listen				监听端口
		server_name			服务的域名，可以配置多个用空格隔开

负载均衡
---
在http中配置
	#设定负载均衡的服务器列表  
    upstream mysvr {  
        #weigth参数表示权值，权值越高被分配到的几率越大    
        #同一机器在多网情况下，路由切换，ip可能不同
        server 127.0.0.1:8080 weight=1;  
        server 127.0.0.1:8090 weight=1;  
        server 127.0.0.1:8070 weight=1;  
    }

代理、反向代理
---

nginx 403 Forbidden
---
nginx的启动用户默认是nginx用户，需要修改web目录的权限

静态文件缓存
---
server中配置缓存，需要指定root值，否则默认是Nginx的目录，会导致找不到文件。
``` bash
	#静态文件缓存时间设置
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
         root   /root/blog_front/blog-front/dist;
         expires 30d;
    }

    #静态文件缓存时间设置
    location ~ .*\.(js|css)?$ {
         root   /root/blog_front/blog-front/dist;
         expires 1h;
    }
```