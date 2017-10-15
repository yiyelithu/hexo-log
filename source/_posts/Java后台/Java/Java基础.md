---
title: Java基础
date: 2017-09-21 23:47:12
categories: server
tags: [Java] 
---
>全局变量和static修饰的局部变量默认初始化为 0 。因为全局变量和static静态局部变量存储在静态数据区。在静态数据区，内存中所有的字节默认值都是 0x00。

>Java中的switch-case语句
switch接受的参数类型有10种，分别是基本类型的byte,short,int,char，以及引用类型的String(只有JavaSE 7 和以后的版本 可以接受String类型参数),enum和byte,short,int,char的封装类Byte,Short,Integer,Character
case 后紧跟常量表达式，不能是变量。

>Maps.newHashMap();

 Map<String, Object> result = new HashMap<String,Object>();
 
 
 上面这种是java原生API写法
 下面这种是google的guava.jar提供的写法，目的是为了简化代码。唯一的区别就是简化代码
 
 
 Map<String, Object> result = Maps.newHashMap();
>泛型仅仅是java的一颗语法糖，它不会影响java虚拟机生成的汇编代码，在编译阶段，虚拟机就会把泛型的类型擦除，还原成没有泛型的代码，顶多编译速度稍微慢一些，执行速度是完全没有什么区别的。