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
	java -cp XXX.jar Main
		执行main方法

目标
---
### Dependency 插件
分析
	
	mvn dependency:analyze
查看项目依赖

	mvn dependency:resolve
	mvn dependency:tree			列出直接和传递性依赖，以树形式打印
	
	
### Exec 插件
运行Java类或其他脚本
	
	mvn exec:java -Dexec.mainClass=com.epdc.maven.App
### Surefire插件
处理单元测试的，如下配置
``` xml
			<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration></configuration>
            </plugin>
```
运行到test阶段为止所有生命周期阶段

	mvn test	有测试单元失败，会停止当前的构建
#### configuration

忽略测试失败,或者mvn test -Dmaven.test.failure.ignore=ture
	
	<testFailureIgnore>true</testFailureIgnore>
完成跳过单元测试，或者mvn test -Dmaven.test.skip=ture
	
	<skip>true</skip>

### Assembly插件
创建应用程序特有分发包，包括项目的二进制文件和所有的依赖，配置
``` xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
            </plugin>
```
### Archetype 插件
#### generate
创建项目
	
	mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app	java项目
	mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-webapp	web项目

### Jetty插件
运行web应用，配置
``` xml
			<plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.3.14.v20161028</version>
                <configuration>
                    <scanIntervalSeconds>1</scanIntervalSeconds>
                </configuration>
            </plugin>
```
启动web应用
	
	mvn jetty:run
### Help插件
pom继承超级pom 和 父pom, 得到混合各个pom配置的有效pom。effective-pom目标可以查看有效pom
	

	mvn help:effective-pom