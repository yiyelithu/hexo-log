---
title: Hibernate
date: 2017-09-24 16:54:12
categories: server
tags: [Java] 
---
### org.hibernate.MappingException: Unknown entity常见问题

1. 可能原因一

 检查实体类是否导入的是 javax.persistence 下的包
 
2. 可能原因二

 没有在cfg文件中加入 *.hbm.xml造成的
 
3. hibernate版本问题,一代版本一代神

  4.5 版本
  ```
  ServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder().applySettings(conf.getProperties()).build();
  ```
  5.2 版本
  ```
  ServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder().applySettings(conf.getProperties()).configure().build();
  ```
  
  