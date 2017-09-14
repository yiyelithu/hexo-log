---
title: Servlet
date: 2017-09-14 23:36:44
categories: 服务器
tags: [java,server]
---
## Servlet和Jsp的区别
1. Servlet中没有内置对象,原来JSP中的内置对象都必须有程序显式创建
2. Servlet对于HTML标签只能使用页面输出流逐行输出

## @WebServlet 

从3.0开始配置Servlet可以使用注解的形式
>有些人可能会遇到这种种情况，在采用注解WebServlet配置Servlet的时候，明明在配置了urlPatterns属性，部署应用程序的时候也没有出错。但是就是在浏览器发请求的时候访问不到资源，报404错误request resource is not available。捣腾了半天也不知道，到底是哪而出错了？
 Servlet3.0之后新增了注解，用于简化Servlet、Filter及Listener的声明，这样就在配置Servlet的时候多了一个选择。Servlet3.0的部署描述文件web.xml的顶层标签<web-app>有一个metadata-complete属性，该属性为true，则容器在部署时只依赖部署描述文件，忽略所有标注，如果不配置该属性，或者将其设置为false，则表示启动标注支持。当metadata-complete="false"时，web.xml和注解对于Servlet的影响同时起作用，两种方法定义的url-partten都可以访问到该Servlet，但是当通过web.xml定义的url-partten访问时，注解定义的属性（初始化参数等）将失效。

### 属性值
name	String	指定Servlet 的 name 属性，等价于 <servlet-name>。如果没有显式指定，则该 Servlet 的取值即为类的全限定名。

value	String[]	该属性等价于 urlPatterns 属性。两个属性不能同时使用。

urlPatterns	String[]	指定一组 Servlet 的 URL 匹配模式。等价于<url-pattern>标签。

loadOnStartup	int	指定 Servlet 的加载顺序，等价于 <load-on-startup>标签。

initParams	WebInitParam[]	指定一组 Servlet 初始化参数，等价于<init-param>标签。

asyncSupported	boolean	声明 Servlet 是否支持异步操作模式，等价于<async-supported> 标签。

description	String	该 Servlet 的描述信息，等价于 <description>标签。

displayName	String	该 Servlet 的显示名，通常配合工具使用，等价于 <display-name>标签。
