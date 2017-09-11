---
title: Spring mvc整合velocity
date: 2017-09-11 16:12:12
categories: server
tags: [Spring,velocity,Spring Mvc] 
---
# 添加Maven依赖: #
        <!--springmvc集成 velocity-->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity</artifactId>
            <version>1.7</version>
        </dependency>
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-tools</artifactId>
            <version>2.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>

# applicationContext-mvc配置文件: #
    <!--》》》》》》》》》》》》》》》   添加velocity显示技术 》》》》》》》》》》》》》》》》》-->
    <!-- velocity环境配置 -->
    <bean id="velocityConfig" class="org.springframework.web.servlet.view.velocity.VelocityConfigurer">
        <!-- velocity配置文件路径  或者直接用velocityProperties属性 -->
        <property name="configLocation" value="classpath:velocity.properties"/>
        <!-- velocity模板路径 -->
        <property name="resourceLoaderPath" value="/WEB-INF/templates/"/>
    </bean>
    <!-- velocity视图解析器 -->
    <bean id="velocityViewResolver" class="org.springframework.web.servlet.view.velocity.VelocityLayoutViewResolver">
        <property name="order" value="0"/>
        <property name="contentType" value="text/html;charset=UTF-8"/>
        <property name="cache" value="true"/>
        <property name="suffix" value=".vm"/>
        <property name="layoutUrl" value="layout/layout.vm"/>
        <property name="exposeSpringMacroHelpers" value="true" /><!--是否使用spring对宏定义的支持-->
        <property name="exposeSessionAttributes" value="true" /><!--是否开放request属性-->
        <property name="requestContextAttribute" value="request"/><!--request属性引用名称-->
        <property name="dateToolAttribute" value="dateTool"/>
        <property name="numberToolAttribute" value="numberTool"/>
    </bean>
# velocity.properties 配置文件 #
该文件velocity.properties 在下面的包路径可以找到 org.apache.velocity.runtime.defaults.velocity.properties


    #设置字符集
    #encoding
    input.encoding  =UTF-8
    output.encoding=UTF-8
    contentType=text/html;charset=UTF-8
    
    
    #autoreload when vm changed
    file.resource.loader.cache=false
    file.resource.loader.modificationCheckInterval  =1
    velocimacro.library.autoreload=false
# 显示文件目录结构 #
![](/images/server/velocity-layout.png)

## header.mv 

    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
    <meta http-equiv="Cache-Control" content="no-store"/>
    <meta http-equiv="Pragma" content="no-cache"/>
    <meta http-equiv="Expires" content="3600"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" name="viewport">

## layout.mv ##
    <html>
    <head>
    <title>$!page_title</title>
    #parse("default/header.vm")
    </head>
    <body>
    
    <div>
    
    $screen_content
    
    </div>
    
    </body>
    </html>
## velocity.mv ##
    <html>
    <head>
    <title>Spring MVC and Velocity</title>
    </head>
    <body>
    <h1>Spring MVC and Velocity</h1>
    
       Hello  ${hello}
    
    <hr />
    Copyright &copy 2014 lm
    </body>
    </html>
## velocity 基本语法 ##
Velocity的基本语法：
1、"#"用来标识Velocity的脚本语句，包括#set、#if 、#else、#end、#foreach、#end、#iinclude、#parse、#macro等；
如:
#if($info.imgs)
<img src="$info.imgs" border=0>
#else
<img src="noPhoto.jpg">
#end


2、"$"用来标识一个对象(或理解为变量)；如
如：$i、$msg、$TagUtil.options(...)等。


3、"{}"用来明确标识Velocity变量；
比如在页面中，页面中有一个$someonename，此时，Velocity将把someonename作为变量名，若我们程序是想在someone这 个变量的后面紧接着显示name字符，则上面的标签应该改成${someone}name。


4、"!"用来强制把不存在的变量显示为空白。
如当页面中包含$msg，如果msg对象有值，将显示msg的值，如果不存在msg对象同，则在页面中将显示$msg字符。这是我们不希望的，为了把不存 在的变量或变量值为null的对象显示为空白，则只需要在变量名前加一个“!”号即可。
如：$!msg

5、循#foreach( $info in $list) $info.someList #end，环读取集合list中的对象
#foreach( $info in $hotL包含文件#inclue("模板文件名")或#parse("模板文件名")st1) 
<a href="/blog/list?&cid=$!info.cid" target="_blank">$!info.title</a><br>
#end 
上面的脚本表示循环遍历hotList1集合中的对象，并输出对象的相关内容。

6、包含文件#inclue("模板文件名")或#parse("模板文件名")
主要用于处理具有相同内容的页面，比如每个网站的顶部或尾部内容。
使用方法，可以参考EasyJF开源Blog及EasyJF开源论坛中的应用！
如：#parse("/blog/top.html")或#include("/blog/top.html")
parse与include的区别在于，若包含的文件中有Velocity脚本标签，将会进一步解析，而include将原样显示。