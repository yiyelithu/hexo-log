---
title: Spring mvc 前后台传值中文乱码问题
date: 2018-02-28 21:49:44
categories: 服务器
tags: [java,Spring]
---
一： 解决GET请求参数到了后台中文乱码问题

方式一: 修改tomcat配置, 暂时做法，没有找到更好的解决办法，换了tomcat了又要重新配置
```java
把tomcat下，server.xml下，添加如下配置，就解决了．

  <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>
```
方式二: 自定义filter, 解决了get请求中文参数问题，但post请求参数到了后台就gg了

<!--more-->

1. 新建 `CustomEncodingFilter.java`
```java
package com.ecut.core.web.filter;

import org.springframework.cglib.proxy.InvocationHandler;
import org.springframework.cglib.proxy.Proxy;
import org.springframework.web.filter.OncePerRequestFilter;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.lang.reflect.Method;

public class CustomEncodingFilter extends OncePerRequestFilter {
    private String encoding;

    public void setEncoding(String encoding) {
        this.encoding = encoding;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {
        // 设置请求响应字符编码
        request.setCharacterEncoding(encoding);
        response.setCharacterEncoding(encoding);

        // 传递给目标servlet或jsp的实际上是动态代理的对象，而不是原始的HttpServletRequest对象
        request = (HttpServletRequest) Proxy.newProxyInstance(request.getClass().getClassLoader(), request.getClass().getInterfaces(), new MyInvacationHandler(request));
        chain.doFilter(request, response);
    }

    class MyInvacationHandler implements InvocationHandler {
        private HttpServletRequest request;
        MyInvacationHandler(HttpServletRequest request){
            this.request=request;
        }

        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            switch (method.getName()) {
                case "getParameter":
                    String value = request.getParameter((String)args[0]);
                    try {
                        if(value != null){
                            value=new String(value.getBytes("ISO-8859-1"),encoding);
                        }
                    } catch (UnsupportedEncodingException e) {
                        e.printStackTrace();
                    }
                    return value;
                case "getParameterValues":
                    String[] values = request.getParameterValues((String)args[0]);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                values[i] = new String(values[i].getBytes("ISO-8859-1"),encoding);
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                    return values;
                default:
                    return method.invoke(request, args);
            }
        }

    }
}
```
2. 配置`web.xml`
```java
  <filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>com.ecut.core.web.filter.CharacterEncodingFilter</filter-class>
    <async-supported>true</async-supported>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

```
二： 解决POST请求参数到了后台中文乱码问题
```java
  <filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <async-supported>true</async-supported>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```