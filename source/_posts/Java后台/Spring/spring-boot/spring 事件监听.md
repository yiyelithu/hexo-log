---
title: Spring 事件监听
date: 2018-01-14 20:01:44
categories: 服务器
tags: [java,Spring]
---
### Spring 事件监听

#### spring的事件(Application Event)
**为Bean与Bean之间的消息通信提供了支持, 当第一个Bean处理完一件事之后, 需要另外一个Bean知道并能做出相应的处理, 这时可以通过事件监听来讲一个Bean监听另一个Bean**

#### 观察者模式
>Spring 事件监听是观察者模式的一种实现 

意图：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。

何时使用：一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。

如何解决：使用面向对象技术，可以将这种依赖关系弱化。

>事件驱动模型简介

事件驱动模型也就是我们常说的观察者，或者发布-订阅模型；理解它的几个关键点：

1. 首先是一种对象间的一对多的关系；最简单的如交通信号灯，信号灯是目标（一方），行人注视着信号灯（多方）；
2. 当目标发送改变（发布），观察者（订阅者）就可以接收到改变；
3. 观察者如何处理（如行人如何走，是快走/慢走/不走，目标不会管的），目标无需干涉；所以就松散耦合了它们之间的关系。

#### 实现流程
1.自定义事件
```java
package com.smart.boot.event;

import org.springframework.context.ApplicationEvent;

public class DemoEvent extends ApplicationEvent {

    private String msg;

    public DemoEvent(Object source, String msg) {
        super(source);
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    /**
     * 去做一些事
     */
    public void todoSomethings() {
        System.out.println("正在做第一件事 , 做完需要做第二件事");
    }
}
```

2.定义事件监听器
```java
package com.smart.boot.event;

import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;

@Component
public class DemoListener implements ApplicationListener<DemoEvent> {
    /**
     * 对消息进行接受处理
     * @param event
     */
    @Override
    public void onApplicationEvent(DemoEvent event) {
        String msg = event.getMsg();
        System.out.println("DemoListener 接受到了消息" + msg );
        event.todoSomethings();
        System.out.println("正在做第二件事");
    }
}
```
3.发布事件
```java
package com.smart.boot.event;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class DemoPublisher {
    /**
     * 注入ApplicationContext来发布事件
     */
    @Autowired
    ApplicationContext context;

    /**
     * 发布事件
     * @param msg
     */
    public void publish(String msg) {
        context.publishEvent(new DemoEvent(this,msg));
    }
}

```

4.定义配置类
```java
package com.smart.boot.event;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.smart.boot.event")
public class Config {
}

```

5.运行
```java
package com.smart.boot.event;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
        DemoPublisher demoPublisher = context.getBean(DemoPublisher.class);
        demoPublisher.publish("hello songshuiyang");
        context.close();
    }
}

```

6.实现结果
```java
DemoListener 接受到了消息hello songshuiyang
正在做第一件事 , 做完需要做第二件事
正在做第二件事
```
#### 总结
1. 实现事件监听可以使业务解耦, 每个模块做好自己的事情即可, 
2. 可用在用户注册功能, eg: http://jinnianshilongnian.iteye.com/blog/1902886
