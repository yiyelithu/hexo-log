---
title: 解决CSS和JS文件的客户端缓存问题
date: 2018-02-10 11:48:12
categories: client
tags: [js,css] 
---
## 场景
做项目的时候，发现自己修改了一个css文件但样式并没有应用，查看http请求(如下图)，注意这个参数`Status Code:200 OK (from disk cache)` , 说明文件是是之前浏览器缓存的文件，浏览器并没有请求我们新改的文件
```java
Request URL:http://localhost:8080/static/layui/build/css/app.cssRequest Method:GET
Status Code:200 OK (from disk cache)
Remote Address:127.0.0.1:8080
Referrer Policy:no-referrer-when-downgrade
```
## 解决方法
> 发现问题了，现在就是要解决如果是服务器js css等文件修改了，怎样让浏览器能够请求我们最新的文件, 通过查看其他人的解决方法，还有看了一下大厂`百度, 淘宝 , 新浪` 对这个问题的处理，总结了一下下面几种方法:

#### 方法一: 在css文件上, js文件后面加上版本号`?v=1245365`
```html
<link rel="stylesheet" href="${ctx}static/admin/css/main.css?v=1245365" media="all" />
```
1. 如果是经常更新的css文件版本号可以取当前时间的时间戳 `v=1518237859338` ,这样就可以每次都获取到最新的文件，但缺点就是每次刷新页面都会请求该文件，在项目开发过程中可以使用这种方式
2. 如果是更新频率不高的的文件，可以取: `v=20180210` , 这样的话刷新页面就不会每次请求这个文件了，可以减轻服务器的压力 
3. 如果是项目稳定了基本没有改动了，可以取一个固定值:`v=0.0.1`

### 方法二：一个版本一个文件夹
> 淘宝的做法: 用一个文件 `6.2.3`
```html
https://g.alicdn.com/kg/??component/6.2.3/extension/content-box/xtpl/view.xtpl-min.js
```




