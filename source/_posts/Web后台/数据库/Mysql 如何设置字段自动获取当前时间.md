---
title: Mysql 如何设置字段自动获取当前时间
date: 2017-12-17 11:15:12
categories: server
tags: [db] 
---
应用场景：
```sql
1、在数据表中，要记录每条数据是什么时候创建的，不需要应用程序去特意记录，而由数据数据库获取当前时间自动记录创建时间；

2、在数据库中，要记录每条数据是什么时候修改的，不需要应用程序去特意记录，而由数据数据库获取当前时间自动记录修改时间；
```

实现方式:
```sql
--修改CreateTime 设置默认时间 CURRENT_TIMESTAMP 
ALTER TABLE `table_name`
MODIFY COLUMN  `created_date` datetime NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间' ;


--修改 UpdateTime 设置 默认时间 CURRENT_TIMESTAMP   设置更新时间为 ON UPDATE CURRENT_TIMESTAMP 

ALTER TABLE `table_name`
MODIFY COLUMN `last_modified_date` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间' ;
```

转自：https://www.cnblogs.com/lhj588/p/4245719.html