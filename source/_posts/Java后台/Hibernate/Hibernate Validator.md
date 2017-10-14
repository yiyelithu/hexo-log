---
title: Hibernate Validator
date: 2017-10-14 12:28:12
categories: server
tags: [Java,Hibernate] 
---
### Hibernate Validator
Hibernate Validator 是 Bean Validation 的参考实现 。Hibernate Validator 提供了 JSR 303 规范中所有内置 constraint 的实现，除此之外还有一些附加的 constraint。
在日常开发中，Hibernate Validator经常用来验证bean的字段，基于注解，方便快捷高效。

| 水果        | 价格    |  数量  |
    | --------   | -----:   | :----: |
    | 香蕉        | $1      |   5    |
    | 苹果        | $1      |   6    |
    | 草莓        | $1      |   7    |

#### 1. Bean Validation 中内置的 constraint
|注解            |          作用|
| --------       | :----: |
| @Valid         |   被注释的元素是一个对象，需要检查此对象的所有字段值  |
|@Valid	        |    被注释的元素是一个对象，需要检查此对象的所有字段值
|@Null	 |   被注释的元素必须为 null
|@NotNull	 |   被注释的元素必须不为 null
|@AssertTrue	 |   被注释的元素必须为 true
|@AssertFalse	 |   被注释的元素必须为 false
|@Min(value)	 |   被注释的元素必须是一个数字，其值必须大于等于指定的最小值
|@Max(value)	 |   被注释的元素必须是一个数字，其值必须小于等于指定的最大值
|@DecimalMin(value) |   	被注释的元素必须是一个数字，其值必须大于等于指定的最小值
|@DecimalMax(value) |   	被注释的元素必须是一个数字，其值必须小于等于指定的最大值
|@Size(max, min)	 |   被注释的元素的大小必须在指定的范围内
|@Digits (integer, fraction) |   	被注释的元素必须是一个数字，其值必须在可接受的范围内
|@Past	 |   被注释的元素必须是一个过去的日期
|@Future	 |   被注释的元素必须是一个将来的日期
|@Pattern(value)	 |   被注释的元素必须符合指定的正则表达式

#### 2. Hibernate Validator 附加的 constraint
| 注解            |           作用|
|------------     |:-------:|
|@Email	被注释的元素必须是电子邮箱地址
|@Length(min=, max=)	| 被注释的字符串的大小必须在指定的范围内
|@NotEmpty	| 被注释的字符串的必须非空
|@Range(min=, max=)	| 被注释的元素必须在合适的范围内
|@NotBlank	| 被注释的字符串的必须非空
|@URL(protocol=,host=,    port=, regexp=, flags=)	|被注释的字符串必须是一个有效的url
|@CreditCardNumber | 被注释的字符串必须通过Luhn校验算法， 银行卡，信用卡等号码一般都用Luhn 计算合法性
|@ScriptAssert (lang=, script=, alias=)	|要有Java Scripting API 即JSR 223 ("Scripting for the JavaTM Platform")的实现
|@SafeHtml(whitelistType=, additionalTags=)|classpath中要有jsoup包
#### 举个栗子
```java
public class User {  
      
    @NotBlank  
    private String name;  
      
    //年龄要大于18岁  
    @Min(18)  
    private int age;  
  
    @Email  
    private String email;  
      
    //嵌套验证  
    @Valid  
    private Product products;  
      
    ... //省略getter，setter  
}  
  
public class Product {  
      
    @NotBlank  
    private String name;  
      
    //价格在10元-50元之间  
    @Range(min=10,max=50)  
    private int price;  
      
    ... //省略getter，setter  
} 
```
转自:http://blog.csdn.net/u011851478/article/details/51842157
