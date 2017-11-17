---
title: Blog-解决问题汇总
date: 2017-011-10 10:47:21
category: Blog
tags: 爬虫, 博客
---

前后端分离
---
##### 跨域问题
* 在前端项目，请求后台接口的URL指定为 http://epdcsiman.top/blog/xxx 或者http://ip/blog/xxx

`出现错误 No 'Access-Control-Allow-Origin' header is present on the requested resource.`

只可以在后台项目接口响应请求头中，增加Access-Control-Allow-Origin的header值为*，允许所有跨域请求。例如：spring项目中，拦截器中统一增加这个header
```java
//在spring-servlet.xml配置拦截器
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.blog.server.client.interceptor.BaseHandlerInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>

//编写拦截器
public class BaseHandlerInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse httpServletResponse, Object o) throws Exception {
        httpServletResponse.setHeader("Access-Control-Allow-Origin","*");//让Angular可以跨域请求
        return true;
    }
}
```
* 在前端项目，请求后台接口用localhost,例如：http://localhost/blog/xxx

`出现错误 net::ERR_CONNECTION_REFUSED`
不会走nginx代理，但服务器用localhost可以访问后台接口，在前端项目中用localhost不行。`不知道原因，没有找到解决方法`

* 在前端项目，请求后台接口的URL指定为 /blog/xxx

通过nginx代理转发到后台接口，这样不会有跨域问题`(推荐使用这个方式)`