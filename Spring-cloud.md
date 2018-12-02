### java.lang.TypeNotPresentException: Type javax.xml.bind.JAXBContext not present
因为JAXB-API是java ee的一部分，在jdk9中没有在默认的类路径中；

java ee api在jdk中还是存在的，默认没有加载而已，jdk9中引入了模块的概念，可以使用

模块命令--add-modules java.xml.bind引入jaxb-api;

### com.sun.jersey.api.client.ClientHandlerException: java.net.ConnectException: Connection refused: connect

	eureka:
	  instance:
	    hostname: localhost
	  client:
	    register-with-eureka: false
	    fetch-registry: false
	    service-url:
	      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/


### No plugin found for prefix 'springboot' in the current project and in the plugin groups
[解决方法](https://blog.csdn.net/Mr_OOO/article/details/54341184#commentsedit)
