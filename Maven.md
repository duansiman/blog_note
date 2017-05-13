---
title: Maven
date: 2017-04-26 15:37:02
tags: Maven
category: Java
---
语法
---
	mvn [options] [<goal(s)>] [<phase(s)>]
	options 	可以用mvn -h 或 mvn --help查看
	phase		是生命周期阶段

生命周期阶段
---
有clean、default和site几类，下面出现的顺序就是执行顺序。
#### clean
pre-clean, clean, post-clean
#### default
validate, initialize, generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy
#### site
pre-site, site, post-site, site-deploy
	

命令
---
	mvn package -Dmaven.skip.test=true 
		打war包
	mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
		创建项目