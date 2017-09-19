---
title: Struts2 的简单实例
date: 2017-09-16 21:30:44
categories: 服务器
tags: [java,struts2]
---
## Struts2 的流程
1. 浏览器发送请求
2. 到达 StrutsPrepareAndExecuteFilter ( 核心控制器 )
3. 分发到指定 XXXAction ( 业务控制器 ) 调用业务方法
4. 返回逻辑视图名
5. StrutsPrepareAndExecuteFilter forward到物理视图,生成响应内容，输出响应