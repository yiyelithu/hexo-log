---
title: Spring Task
date: 2017-11-07 22:47:45
categories: 服务器
tags: [java,Spring]
---
## Spring Task
>spring task作为定时任务的处理,是Spring自带的一个设定时间自动任务调度,提供了两种方式进行配置，一种是注解的方式，而另外一种就是XML配置方式了。

### 基于XML配置文件的方式
>applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/task
    http://www.springframework.org/schema/task/spring-task.xsd">

    <!--使用配置文件的方式,注册 xmlTaskJob bean中的job1方法,每隔一秒执行 -->
    <task:scheduled-tasks>
         <task:scheduled ref="xmlTaskJob" method="job1" cron="*/1 * * * * ?"/>
    </task:scheduled-tasks>

    <context:component-scan base-package="com.learn.schedule.service"/>
</beans>

```
>XmlTaskJob.Java
```java
package com.learn.schedule.service;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;


/**
 * @author songshuiyang
 * @title: 基于xml文件配置的定时任务
 * @description:
 * @date 2017/11/7 22:20
 */
@Service
public class XmlTaskJob {
    private Logger logger = LoggerFactory.getLogger(this.getClass());

    public void job1() {
        logger.info("基于xml文件配置的定时任务，每隔一秒执行");
    }

}
```
>Test.java
````java
/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/11/7 22:45
 */
public class Test {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("classpath:/spring/applicationContext.xml");
    }
}
````
### 基于注解配置文件的方式更简单
>applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/task
    http://www.springframework.org/schema/task/spring-task.xsd">

    <!-- 启动定时器 基于注解-->
    <task:annotation-driven/>

    <context:component-scan base-package="com.learn.schedule.service"/>
</beans>
```
>AnnotationTaskJob.Java
```java
package com.learn.schedule.service;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

/**
 * @author songshuiyang
 * @title: 基于注解配置的定时任务
 * @description:
 * @date 2017/11/7 22:42
 */
@Component
public class AnnotationTaskJob {
    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @Scheduled(cron = "*/2 * * * * ?") //每2秒执行一次
    public void job() {
        logger.info("基于注解配置的定时任务，每隔俩秒执行");
    }
}
```
>效果:
![logo](/images/server/spring-schedule.gif) 