---
title: Hibernate 例子
date: 2017-09-23 23:28:12
categories: server
tags: [Java,Hibernate] 
---
## 概述:
>面向Java环境的对象/关系数据库映射工具,用于将面向对象模型表示的对象映射到基于SQL的关系模型的数据结构中,消除那些针对特定数据库厂商的SQL代码,并把结果集从表格式的形式转换成值对象的形式

<!--more-->
## Hibernate的数据库操作
### 直接采用了POJO(普通的传统的Java对象)作为持久化类
```$xslt
package com.hibernate.entity;

import javax.persistence.*;

/**
 * @author songshuiyang
 * @title:
 * @description:
 * @date 2017/9/23 23:41
 */
@Entity /*标明持久化类*/
@Table(name = "user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 主键生成策略
    private String id;

    private String name;

    private int sex;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getSex() {
        return sex;
    }

    public void setSex(int sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", sex=" + sex +
                '}';
    }
}

```
Hibernate基本上是使用了JPA的标准注解(javax.persistence)
>JPA
 1. JPA是Java Persistence API的简称，中文名Java持久层API，是JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。
Sun引入新的JPA ORM规范出于两个原因：其一，简化现有Java EE和Java SE应用开发工作；其二，Sun希望整合ORM技术，实现天下归一。

2. JPA是一种规范，而Hibernate是它的一种实现。除了Hibernate，还有EclipseLink(曾经的toplink)，OpenJPA等可供选择，所以使用Jpa的一个好处是，可以更换实现而不必改动太多代码。

###  配置文件 (#.properties , XML配置文件的形式)
```aidl
<?xml version="1.0" encoding="GBK"?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
  <session-factory>
    <!-- 指定连接数据库所用的驱动 -->
    <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
    <!-- 指定连接数据库的url，其中hibernate是本应用连接的数据库名 -->
    <property name="connection.url">jdbc:mysql://localhost/ecut</property>
    <!-- 指定连接数据库的用户名 -->
    <property name="connection.username">root</property>
    <!-- 指定连接数据库的密码 -->
    <property name="connection.password">root</property>
    <!-- 指定连接池里最大连接数 -->
    <property name="hibernate.c3p0.max_size">20</property>
    <!-- 指定连接池里最小连接数 -->
    <property name="hibernate.c3p0.min_size">1</property>
    <!-- 指定连接池里连接的超时时长 -->
    <property name="hibernate.c3p0.timeout">5000</property>
    <!-- 指定连接池里最大缓存多少个Statement对象 -->
    <property name="hibernate.c3p0.max_statements">100</property>
    <property name="hibernate.c3p0.idle_test_period">3000</property>
    <property name="hibernate.c3p0.acquire_increment">2</property>
    <property name="hibernate.c3p0.validate">true</property>
    <!-- 指定数据库方言 -->
    <property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
    <!-- 根据需要自动创建数据表 -->
    <property name="hbm2ddl.auto">update</property><!--①-->
    <!-- 显示Hibernate持久化操作所生成的SQL -->
    <property name="show_sql">true</property>
    <!-- 将SQL脚本进行格式化后再输出 -->
    <property name="hibernate.format_sql">true</property>

    <mapping class="com.hibernate.entity.User"/>

  </session-factory>
</hibernate-configuration>
```

###  测试方法
```aidl
public class UserManagerTest {
    @Test
    public void test1(){
        // 实例化Configuration，
        Configuration conf = new Configuration().configure();
        ServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder().applySettings(conf.getProperties()).configure().build();
        // 以Configuration实例创建SessionFactory实例
        SessionFactory sf = conf.buildSessionFactory(serviceRegistry);
        // 创建Session
        Session sess = sf.openSession();
        // 开始事务
        Transaction tx = sess.beginTransaction();
        // 创建消息对象
        User user = new User();
        // 设置消息标题和消息内容
       user.setName("hibernate");
       user.setSex(12);
       sess.save(user);
        // 提交事务
        tx.commit();
        // 关闭Session
        sess.close();
        sf.close();

    }
}
```

