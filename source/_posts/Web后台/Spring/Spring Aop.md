---
title: Spring Aop
date: 2017-11-18 15:47:45
categories: 服务器
tags: [java,Spring]
---
## Spring Aop

### 一: 概念 

#### 1. 什么是AOP
 
在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。

####  2. 为什么要用Aop
利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。在不改变原有方法的基础添加一些功能 , 比如:日志记录，性能统计，安全控制，事务处理，异常处理等等。

<!--more-->

####  3. Aop 术语
> 连接点(JoinPoint)

 程序执行到某个特定位置 , Spring 仅支持方法级的连接点(方法执行前，方法完成后，抛出异常后)

> 切点(Pointcut)

 从连接点的基础上引出的概念，是指特定的连接点，一个类有好多方法,每个方法又有多个连接点，则需要切点来限定一个小范围的连接点
 
> 通知、增强处理(Advice)

   就是指你所需要添加的功能及这个功能什么时候(通知)实现 , 比如一个业务方法需要实现日志功能 , 那么就需要专门在一个地方定义好需要做什么，然后定义什么时候执行(方法执行前？，方法完成后？，抛出异常？。。。)
  
   Spring 切面可应用的 5 种通知类型：
   1. Before——在方法调用之前调用通知
   2. After——在方法完成之后调用通知，无论方法执行成功与否
   3. After-returning——在方法执行成功之后调用通知
   4. After-throwing——在方法抛出异常后进行通知
   5. Around——通知包裹了被通知的方法，在被通知的方法调用之前和调用之后执行自定义的行为

> 引入(introduction)
 
  特殊的增强，为类添加一些属性和方法

> 切面(Aspect)

 切面由切点和增强组成 , 及包括横切逻辑的定义，也包括切点的定义, 

> 目标对象(Target)

 增强逻辑的织入目标类 , 如果没有Aop,那么目标对象就要自己实现(日志记录，性能统计，安全控制，事务处理，异常处理)这些功能，那么一个方法就会变成很杂乱
 
> 织入(Weaing)

 将增强添加到目标对象的具体连接点上, Spring使用动态代理织入
 
 
 Aop有三种织入方式
 1. 编译期织入
 2. 类装载期织入
 3. 动态代理织入: 在运行期间为目标类添加增强生成子类的方式
### 二: Spring Aop 的应用

> Spring Aop的使用一般通过俩种方式:第一种是通过注解的，第二种是通过xml配置

#### 通过注解的方式实现Aop

1. 第一步 Maven 导包
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <parent>
        <groupId>ecut</groupId>
        <artifactId>spring-parent</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <modelVersion>4.0.0</modelVersion>
    <groupId>ecut</groupId>
    <artifactId>spring-aop</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-aop</name>

    <dependencies>
        <!-- spring 核心 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
        </dependency>

        <!-- spring aop -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!--test-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- 日志 -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </dependency>


    </dependencies>
    <build>
        <finalName>spring-aop</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>

            <!--@Override is not allowed when implementing interface method-->
            <!-- 编码和编译和JDK版本 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>utf8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

2. 第二步 编写一个基于 @AspectJ 的切面
```java
package com.aop.learn.aspectj;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

/**
 * @author songshuiyang
 * @title: @Aspect
 * @description:
 * @date 2017/11/15
 */
@Component
@Aspect // 通过该注解将该类标识为一个切面
public class PreGreetingAspect {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    /**
     * 前置增强, greetTo方法执行前触发此方法
     *
     */
    @Before("execution(* greetTo(..))") // 定义切点和增强类型（前置增强,可以带任何参数，和任意的返回值）
    public void beforeGreeting() { // 增强的横切逻辑
        logger.info("How are you Aspect 使用了前置增强");
    } 
}

```
3: 编写目标对象

> Writer.java 接口

```java
package com.aop.learn.service;

/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/11/15
 */
public interface Writer {

    public void greetTo();

}


```
> NativeWaiter.java 实现方法
```java
package com.aop.learn.service.impl;

import com.aop.learn.annotation.NeedTest;
import com.aop.learn.service.Writer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/11/15
 */
@Service
public class NativeWaiter implements Writer {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @Override
    public void greetTo() {
        logger.info("执行方法体: ");
    }
    
}
```

4：Spring配置文件
> applicationContext.xml
```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
    http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
">
    <context:component-scan base-package="com.aop.learn"/>

    <!--基于@AspectJ切面的驱动器,自动为Spring容器中匹配@AspectJ切面的Bean创建代理，完成切面织入-->
    <aop:aspectj-autoproxy/>
    <!--<aop:aspectj-autoproxy proxy-target-class="true"/> 表示使用CGLib动态代理技术织入增强-->


</beans>
```

5: 测试类

测试基类
> BaseTest.java
```java
package com.aop.test;

import org.junit.runner.RunWith;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.AbstractJUnit4SpringContextTests;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/11/15 
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:/spring/applicationContext.xml")
public class BaseTest extends AbstractJUnit4SpringContextTests {

    public Logger logger = LoggerFactory.getLogger(this.getClass());
}

```

测试类
> AspectTest.java

````java
package com.aop.test.service;

import com.aop.learn.service.AgentWriter;
import com.aop.learn.service.Seller;
import com.aop.learn.service.Writer;
import com.aop.test.BaseTest;
import org.junit.Test;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;

/**
 * @author songshuiyang
 * @title: 基于spring配置使用@AspectJ切面
 * @description:
 * @date 2017/11/15 
 */
public class AspectTest extends BaseTest {

 
    @Autowired
    private Writer writer;
    
    /**
     * 基于spring配置使用@AspectJ切面
     */
    @Test
    public void test1() {
        writer.greetTo();
    }
}

````
6: 效果图,完成 aop增强
![logo](/images/server/springaop/aop.png)
 

#### 

#### 
#### 
#### 


#### 通过xml schema的方式实现Aop

> applicationContext-schema.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd


	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!-- aop:config 配置一个基于Schema的切面，aop:config 可以定义多个切面-->
    <aop:config proxy-target-class="true">
        <!--aop:pointcut 配置命名切点,可以被其他增强引用-->
        <aop:pointcut id="greetToPointcut"
                      expression="target(com.aop.learn.service.impl.NativeWaiter) and execution (* greetTo(..))"/>
        <aop:pointcut id="bindParmPointcut"
                      expression="target(com.aop.learn.service.impl.NativeWaiter) and execution (* greetTo(..)) and args(clientName)"/>

        <!-- aop:advisor 是切点和增强的复合体,仅包含一个切点和增强-->
        <aop:advisor advice-ref="advisorMethods"
                     pointcut="target(com.aop.learn.service.impl.NativeWaiter) and execution (* serveTo(..))"/>

        <!--aop:aspect 元素标签定义切面,其内部可以定义多个增强-->
        <aop:aspect ref="adviceMethods">
            <!-- aop:before前置增强 method 增强方法， pointcut 切点表达式-->
            <aop:before method="preGreeting"
                        pointcut-ref="greetToPointcut"/>
            <!-- aop:before后置增强-->
            <aop:after-returning method="afterGreeting"
                                 pointcut="target(com.aop.learn.service.impl.NativeWaiter) and execution (* name(..))"
                                 returning="retVal"/>
            <!-- 测试绑定连接点信息-->
            <aop:after method="bindParmGreet"
                       pointcut-ref="bindParmPointcut"/>
        </aop:aspect>


    </aop:config>

    <!--增强方法所在的Bean-->
    <bean id="adviceMethods" class="com.aop.learn.schema.AdviceMethods"/>
    <bean id="nativeWaiter" class="com.aop.learn.service.impl.NativeWaiter"/>
    <bean id="advisorMethods" class="com.aop.learn.schema.AdvisorMethods"/>

</beans>
```

>AdviceMethods.java
```java
package com.aop.learn.schema;

/**
 * @author songshuiyang
 * @title: Schema 用作增强的方法
 * @description:
 * @date 2017/11/18 
 */
public class AdviceMethods {
    /**
     * 前置增强
     */
    public void preGreeting() {
        System.out.println("-------------前置增强");
    }

    /**
     * 后置增强
     *
     * @param retVal
     */
    public void afterGreeting(String retVal) {
        System.out.println("-------------后置增强,返回的参数" + retVal);
    }

    /**
     * 绑定连接点信息
     *
     * @param clientName
     */
    public void bindParmGreet(String clientName) {
        System.out.println("-------------绑定连接点信息 的参数" + clientName);

    }

}

```

>NativeWaiter.java
```java
package com.aop.learn.service.impl;

import com.aop.learn.annotation.NeedTest;
import com.aop.learn.service.Writer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/11/15
 */
@Service
public class NativeWaiter implements Writer {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @Override
    public void greetTo(String clientName) {
        logger.info("-------------greetTo " + clientName);
    }

    @Override
    public void greetTo(String clientName, Integer age) {
        logger.info("-------------greetTo " + clientName + "  " + age + "岁");
    }

    @Override
    public void serveTo(String clientName) {
        logger.info("-------------serveTo " + clientName);
    }

    @Override
    @NeedTest()
    public void nestTo() {
        logger.info("开始执行 nestTo() 函数");
    }

    @Override
    public String name() {
        return "宋水阳";
    }

    @Override
    public void throwExcetion() {
        throw new IllegalArgumentException("抛出异常了");
    }
}

```

>AdvisorMethods.java
```java
package com.aop.learn.schema;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

/**
 * @author songshuiyang
 * @title: aop:advisor 是切点和增强的复合体,仅包含一个切点和增强
 * @description:
 * @date 2017/11/18
 */
public class AdvisorMethods implements MethodBeforeAdvice {

    @Override
    public void before(Method method, Object[] args, Object taget) throws Throwable {
        System.out.println("--------------执行aop:advisor增强----------------");
        System.out.println("获取的参数" + args[0]);
    }
}

```

