---
title: web.xml中配置JSP属性
date: 2017-09-15 16:19:45
categories: 服务器
tags: [java,server,jsp]
---
## web.xml中配置JSP属性
>为什么要在web.xml配置JSP属性

如果许多JSP有着相似的属性，那么在每个JSP文件的顶部重复添加page指令是非常麻烦的工作。幸运的是，在部署描述符中可以配置通用的JSP属性。

> web.xml中添加JSP属性样例

    <jsp-config>  
     	<jsp-property-group>  
     		<url-pattern>*.jsp</url-pattern>  
     		<url-pattern>*.jspf</url-pattern>  
     		<page-encoding>UTF-8</page-encoding>  
     		<scripting-invalid>false</scripting-invalid>  
     		<include-prelude>/WEB-INF/jsp/base.jspf</include-prelude>  
     		<trim-directive-whitespaces>true</trim-directive-whitespaces>  
     		<default-content-type>text/html</default-content-type>  
     		</jsp-property-group>  
    </jsp-config> 

<jsp-config>中可以包含任意数目的<jsp-property-group>标签。通过为<jsp-property-group>定义不同的<url-pattern>标签来区分不同的属性组。
<include-prelude>标签，将告诉容器在所有属于改该属性组的JSP的头部添加文件/WEB-INF/jsp/base.jspf。
<include-coda>标签定义了包含在组中所有JSP尾部的文件。

在一个JSP组中可以同时使用这些标签多次。
<page-encoding>与page指令的pageEncoding特性一致。
<default-content-type>标签可以定义内容类型，默认为text/html
<trim-directive-whitespaces>也是一个特别有用的属性，该属性告诉JSP转换器删除响应输出中的空白，只保留指令、声明、脚本和其他JSP标签创建的文本。
<scripting-invalid>标签可以实现完全禁止JSP中的Java
<el-ignored>的作用类似，不过它对应的是page指令中的isELIgnored特性。
除了<url-pattern>，<jsp-property-group>中所有标签都是可选的，但在使用它们时必须按照下面的顺序添加到<jsp-property-group>中(忽略掉部希望使用的标签)：<url-pattern>、<el-ignored>、<page-encoding>、<scripting-invalid>、<is-xml>、<include-prelude>、<include-coda>、<deferred-syntax-allowed-as-literal>、<trim-directive-whitespace>、<default-content-type>、<buffer>、<error-on-undeclared-namespace>。