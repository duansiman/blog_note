---
title: Docker
date: 2017-011-10 10:47:21
category: 应用服务器
tags: Docker
---
#angular-cli

docker run -it --rm -w /root/blog -v /root/blog/blog_front/blog-front:/root/blog alexsuch/angular-cli npm install
docker run -it --rm -w /root/blog -v /root/blog/blog_front/blog-front:/root/blog alexsuch/angular-cli ng build --prod --aot false

#nginx

docker run --name nginx -v /root/blog/conf/nginx:/etc/nginx/conf.d/ -p 80:80 -d nginx "-g" "daemon off;"

docker run --name nginx --link tomcat:tomcat -v /root/blog/conf/nginx:/etc/nginx/conf.d/ -v /root/blog/blog_front/blog-front/dist:/var/www/html:ro  -v /root/blog/log/nginx:/var/log/nginx -p 80:80 -d nginx

#maven

docker run --rm -v /root/blog/blog_server/blog_server:/usr/local/src/blog_server -v /root/blog/m2:/root/.m2 -w /usr/local/src/blog_server maven:3.5.2-ibmjava-8 mvn clean install

docker run --rm -v /root/blog/blog_server/blog_server:/usr/local/src/blog_server -v /root/blog/m2:"$USER_HOME_DIR"/.m2 -w /usr/local/src/blog_server maven:3.5.2-ibmjava-8 mvn clean install

#mysql

docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d -v /root/blog/data/mysql:/var/lib/mysql mysql:5.7.20 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci

docker run -it --link mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'

docker run -it --rm mysql mysql --default-character-set=utf8mb4 -h172.17.0.3 -uroot -p123456
docker run -it --rm --link mysql:mysql mysql mysql --default-character-set=utf8mb4 -hmysql -uroot -p123456

#tomact
docker run --name tomcat --link redis:redis --link mysql:mysql -v /root/blog/log/tomact:/usr/local/tomcat/logs -v /root/blog/data/tomcat:/usr/local/tomcat/webapps -v /root/blog/log/blog:/root/blog/logs -v /root/blog/data/blog:/root/blog/article -p 8080:8080 -d tomcat:8.5.23-jre8-alpine

#redis
docker run --name redis -d -v /root/blog/data/redis:/data -v /root/blog/conf/redis/redis.conf:/usr/local/etc/redis/redis.conf redis redis-server --appendonly yes

docker run --name redis -d -p 6379:6379 -v /root/blog/data/redis:/data -v /root/blog/conf/redis/redis.conf:/usr/local/etc/redis/redis.conf redis redis-server  /usr/local/etc/redis/redis.conf --appendonly yes

docker run -it --link redis:redis --rm redis redis-cli -h redis -p 6379 -a devin123456.

#cp
cp blog_server/blog_server/blog-webs/web-client/target/blog.war data/tomcat

#pyspider

#### mysql
docker run --name pyspider_mysql -d -v /root/blog/data/pyspider_mysql:/var/lib/mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=yes mysql:latest

#### rabbitmq
docker run --name rabbitmq -d rabbitmq:latest

#### phantomjs
docker run --name phantomjs -d binux/pyspider:latest phantomjs

#### result worker
docker run --name result_worker -m 128m -d 
-v /root/blog/data/pyspider/worker:/opt/pyspider/data --link pyspider_mysql:pyspider_mysql --link rabbitmq:rabbitmq binux/pyspider:latest result_worker

#### processor, run multiple instance if needed.
docker run --name processor -m 256m -d --link mysql:mysql --link rabbitmq:rabbitmq binux/pyspider:latest processor
#### fetcher, run multiple instance if needed.
docker run --name fetcher -m 256m -d --link phantomjs:phantomjs --link rabbitmq:rabbitmq binux/pyspider:latest fetcher --no-xmlrpc

#### scheduler
docker run --name scheduler -d 
-v /root/blog/data/pyspider/scheduler:/opt/pyspider/data --link pyspider_mysql:pyspider_mysql --link rabbitmq:rabbitmq binux/pyspider:latest scheduler

#### webui
docker run --name webui -m 256m -d -p 5000:5000 
-v /root/blog/data/pyspider/webui:/opt/pyspider/data --link pyspider_mysql:pyspider_mysql --link rabbitmq:rabbitmq --link scheduler:scheduler --link phantomjs:phantomjs binux/pyspider:latest webui