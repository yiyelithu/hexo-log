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