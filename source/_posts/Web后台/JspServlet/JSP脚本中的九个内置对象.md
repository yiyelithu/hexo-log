---
title: JSP脚本中的九个内置对象
date: 2017-09-14 15:25:44
categories: 服务器
tags: [java,server,jsp]
---
JSP脚本中包含九个内置对象，这九个内置对象都是Servlet API接口的实例，只是JSP规范对它们进行了默认初始化（由JSP页面对应的Servlet的_jspService()方法来创建这些实例),也就是说它们已经是对象，可以直接使用

    1	pageContext	javax.servlet.jsp.PageContext
    
    2	request	javax.servlet.http.HttpServletRequest
    
    3	response	javax.servlet.http.HttpServletResponse
    
    4	session	javax.servlet.http.HttpSession
    
    5	application	javax.servlet.ServletContext
    
    6	config	javax.servlet.ServletConfig
    
    7	out	javax.servlet.jsp.JspWriter
    
    8	page	java.lang.Object
    
    9	exception	java.lang.Throwable

<!--more-->
### page对象
page对象表示当前一个JSP页面，可以理解为一个对象本身，即：把一个JSP当作一个对象来看待。page对象在开发中几乎不用，了解一下即可
### out对象
out对象代表一个页面输出流，通常用于在页面上输出变量值及常量。一般在使用输出表达式的地方都可以使用out对象达到同样的效果。out是个页面输出流，负责输出页面的内容，但是用out需要编写更多的代码。<%=  %>表达式的本质就是out.write(…);
对于页面上的某个html标签来讲
<table><tr></tr></table>
如果使用了out即

      <%
    	out.println(“<table>”);
    	out.println(“<tr>”);
    	out.println(“</tr>”);
    	out.println(“</table>”);
      %>

### pageContext对象
pageContext对象是JSP技术中最重要的一个对象，它代表JSP页面的运行环境，这个对象不仅封装了对其它8大隐式对象的引用，它自身还是一个域对象(容器)，可以用来保存数据。并且，这个对象还封装了web开发中经常涉及到的一些常用操作，例如引入和跳转其它资源、检索其它域对象中的属性等。

    getException方法		返回exception隐式对象
    getPage方法			返回page隐式对象
    getRequest方法		返回request隐式对象
    getResponse方法		返回response隐式对象
    getServletConfig方法 返回config隐式对象
    getServletContext方法返回application隐式对象
    getSession方法		返回session隐式对象
    getOut方法			返回out隐式对象

>pageContext 封装其它8大内置对象的意义

　　如果在编程过程中，把pageContext对象传递给一个普通java对象，那么这个java对象将可以获取8大隐式对象，此时这个java对象就可以和浏览器交互了，此时这个java对象就成为了一个动态web资源了，这就是pageContext封装其它8大内置对象的意义，把pageContext传递给谁，谁就能成为一个动态web资源，那么什么情况下需要把pageContext传递给另外一个java类呢，什么情况下需要使用这种技术呢，在比较正规的开发中，jsp页面是不允许出现java代码的，如果jsp页面出现了java代码，那么就应该想办法把java代码移除掉，我们可以开发一个自定义标签来移除jsp页面上的java代码，首先围绕自定义标签写一个java类，jsp引擎在执行自定义标签的时候就会调用围绕自定义标签写的那个java类，在调用java类的时候就会把pageContext对象传递给这个java类，由于pageContext对象封装了对其它8大隐式对象的引用，因此在这个java类中就可以使用jsp页面中的8大隐式对象(request，response，config，application，exception，Session，page，out)了，pageContext对象在jsp自定义标签开发中特别重要。

>pageContext 作为域对象

pageContext对象可以作为容器来使用，因此可以将一些数据存储在pageContext对象中。

pageContext对象的常用方法

    1 public void setAttribute(java.lang.String name,java.lang.Object value)
    2 public java.lang.Object getAttribute(java.lang.String name)
    3 public void removeAttribute(java.lang.String name)
    4 public java.lang.Object findAttribute(java.lang.String name)



### application 对象
 - 在整个Web应用的多个JSP、Servlet之间的共享数据。通常被定义为数据字典来使用。通常在一处实现application.setAttribute(“name”,value);来定义一个变量，在JSP中使用application.getAttribute(“name”);获取值；在Servlet中使用一个实例的ServletContext对象sc.getAttribute(“name”);获取值。
我们可以把application理解成一个Map对象，任何JSP、Servlet都可以把某个变量放入application中保存，并指出一个属性名；而该应用的其他JSP、Servlet就可以根据该属性名来得到这个变量。由于application对象代表整个Web应用，所以只应该把Web应用的状态数据放入到application中。
 - 访问Web应用的配置参数，在web.xml中配置类似的参数，该标签是<web-app></web-app>下的子标签。即
    

    <context-param>
       <param-name>name</param-name>
       <param-value>value</param-value>
    </context-param>
    

 在JSP中可以通过 application.getInitParameter(“name”);取得配置的参数，在Servlet中可以先实例个ServletContext对象即：
 final javax.servlet.ServletContext application;
然后就可以取值了，即：

    application = pageContext.getServletContext();
    application.getInitParameter("name");

这里通常被用作普通java Web开发中数据库用户名，密码的获取时使用，因为在项目开发用的密码不一定和部署在服务器上的密码一致，但是把它写到这里便于修改这些有关项目的参数。 

### config 对象
config对象代表当前的JSP配置信息，但JSP页面通常无需配置，因此也就不存在配置信息，该对象在JSP页面用的比较少，但在Servlet中用处则相对较大，因为Servlet需要在web.xml文件中进行配置，可以指定配置参数。但是如果说要为某个JSP配置一些参数的话，也是跟配置Servlet一样需要在web.xml中配置，也就说吧JSP当成Servlet配置

    <servlet>
       <servlet-name>Configure</servlet-name>
       <jsp-file>/getcontextparam.jsp</jsp-file>
       <init-param>
     <param-name>conn</param-name>
     <param-value>connnn</param-value>
       </init-param>
     </servlet>
     <servlet-mapping>
       <servlet-name>Configure</servlet-name>
       <url-pattern>/configure</url-pattern>
     </servlet-mapping>
其中这里“<jsp-file>/getcontextparam.jsp</jsp-file>”是表明把某个JSP配置成Servlet。
在地址栏中访问时要输入http://localhost:8080/test/configure（url-pattern中内容）
在JSP中获取参数时使用config.getInitParameter("conn")即可。

### exception 对象
该实例代表其他页面的异常和错误,只有当页面是错误处理页面，即编译指令page的isErrorPage属性为true时,该对象才可以使用