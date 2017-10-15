---
title: MySql语法
date: 2017-10-15 10:24:12
categories: server
tags: [db] 
---
### limit
```sql
mysql> SELECT * FROM table LIMIT 5,10; // 检索记录行 6-15   
  
//为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1：    
mysql> SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last.   
  
//如果只给定一个参数，它表示返回最大的记录行数目：    
mysql> SELECT * FROM table LIMIT 5; //检索前 5 个记录行   
  
//换句话说，LIMIT n 等价于 LIMIT 0,n。  
```