---
title: Spring Assert
date: 2017-09-29 00:01:44
categories: 服务器
tags: [java,Spring]
---
## Assert（当要判断一个方法传入的参数时，我们就可以使用断言。）
>package org.springframework.util

### 1. notNull(Object object) 

当 object 不为 null 时抛出异常，notNull(Object object, String message) 方法允许您通过 message 定制异常信息。和 notNull() 方法断言规则相反的方法是 isNull(Object object)/isNull(Object object, String message)，它要求入参一定是 null；

### 2. isTrue(boolean expression) / isTrue(boolean expression, String message) 

当 expression 不为 true 抛出异常；

### 3. notEmpty(Collection collection) / notEmpty(Collection collection, String message) 

当集合未包含元素时抛出异常。

notEmpty(Map map) / notEmpty(Map map, String message) 和 notEmpty(Object[] array, String message) / notEmpty(Object[] array, String message) 分别对 Map 和 Object[] 类型的入参进行判断；

### 4. hasLength(String text) / hasLength(String text, String message)  

当 text 为 null 或长度为 0 时抛出异常；

### 5. hasText(String text) / hasText(String text, String message)  

text 不能为 null 且必须至少包含一个非空格的字符，否则抛出异常；

### 6. isInstanceOf(Class clazz, Object obj) / isInstanceOf(Class type, Object obj, String message)  

如果 obj 不能被正确造型为 clazz 指定的类将抛出异常；

### 7. isAssignable(Class superType, Class subType) / isAssignable(Class superType, Class subType, String message)  

subType 必须可以按类型匹配于 superType，否则将抛出异常；