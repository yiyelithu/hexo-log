---
title: Ascii 160空格问题
date: 2018-01-09 21:24:12
categories: server
tags: [Java] 
---
### 场景
>获取从http传输过来的字符串的时候，碰到解析字符串不能分割字符串的情况
```java
String str = doc.get(0); // str = "江西省 赣州市"
String [] area = str.spilt("\\s+");
```
运行上面的代码的时候发现不能截取字符串, 初步怀疑是编码问题，然而经过验证发现并不是,然后就通过字符串截取成 char 字符发现，该char字符ASCII是160,
马上查找资料发现是空格分两种编码格式: 1: 普通的空格,ASCII码为32  2:第二种是 网页上的 &nbsp 空格,ASCII为160, 才发现空格也是有多种情况

解决方法:
```java
// 需要将ASCII为160的空格转成普通的空格
str = str.replaceAll("[\\s\\u00A0]+", " ");

```


