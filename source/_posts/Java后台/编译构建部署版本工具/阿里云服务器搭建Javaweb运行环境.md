---
title: 阿里云服务器搭建Javaweb运行环境
date: 2018-01-20 15:07:12
categories: server
tags: [java] 
---
### 一：前言
借助阿里云的云翼计划的梯子买了个 阿里的ESC云服务器，学生专享优惠10块钱/月，原价一百多一个月，超级划算，当然服务器配置对于我们这些学生捣鼓捣鼓还是满足的。

配置：

|  配置 | 参数    | 
| -----   | -----:   | 
| CPU     | Intel Xeon E5-2682 v4 1核   |
| 内存     | 2G 最新一代DDR4 内存        |
| 带宽    | 1M带宽 VPC专有网络, I/O 优化 |
| 系统盘   | 40G系统盘高效云盘           |

系统：CentOS 7.3 64位(可选ubuntu, windows service)


ESC: 云服务器 ECS（Elastic Compute Service）是一种弹性可伸缩的计算服务，助您降低 IT 成本，提升运维效率，使您更专注于核心业务创新

### 二：搭建步骤
#### 2.1 购买 ESC云服务器
购买链接:https://www.aliyun.com/product/ecs?spm=5176.8499797.765261.239.9Uf4pK, 当然如果是学生的话可以使用上面的云翼计划(https://promotion.aliyun.com/ntms/campus2017.html?spm=5176.8789780.765430.4.246c0fa5bHX2oK)优惠的方式购买，当时购买还送了(CDN流量包)和(OSS资源包) , 良心企业!!! 

#### 2.2 查看系统参数，及配置参数
下单完成之后在 控制台-> 云服务器 ECS -> 实例 可看到系统自动为我们创建的 服务器实例, 里面提供了一些系统参数，还展示了系统的一些运行状态参数。

我们需要的参数

1.公网ip

>访问实例需要用

2.远程连接密码  

> 这个在第一次使用浏览器远程连接主机的时候，阿里云会提供，记住只出现一次，可以用笔记本记录下来，以后每次用浏览器远程控制访问主机的时候需要提供

3.登入系统的密码 

在实例信息面板中有一个重置密码的功能，第一次需要自己设置，这个是主机系统的登入密码。 一开始用浏览器远程连接主机的时候，进入到了命令行界面, 要求输入密码的时候一直输入的是远程连接密码，导致一直登不进，查了一下资料发现系统登入密码需要自己创建, 登入用户 `root`

4.安全组配置

安全组配置是阿里云在系统做了一次网关过滤，外网访问主机，主机访问外网都需要配置这个参数，否则访问不到, 安全组配置分为入口和出口

入口配置:

- 把一些常用端口打开:`80 22(ssh, sftp) 23(telnet)` , 使用xshell和ftp都是使用的是22端口
- 添加 `全部 ICMP` 协议类型, 端口范围为`-1/-1`, 没有这条规则则ping 不通主机


#### 2.3 连接主机进行配置
有了上面的配置就可以通过远程连接主机了, 我是使用`xshell` 进行远程连接, 使用`fileZilla`进行传输文件
##### 2.3.1 配置 Java环境
参考: https://www.cnblogs.com/sxdcgaq8080/p/7492426.html

注: 如果出现`export =' not a valid identifier`
```sql
原因就是你修改的 /etc/profile 文件里
你加过空格

我的代码如下：
export JAVA_HOME = /usr/java/jdk1.7.0_75
export PATH = $JAVA_HOME/bin:$PATH
export CLASSPATH = .:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

修改为如下：
export JAVA_HOME=/usr/java/jdk1.7.0_75
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
将等号两边的空格去掉就可以了 ，大家要注意
```

##### 2.3.2 配置 Tomcat环境
参考： http://www.linuxidc.com/Linux/2015-09/123118.htm

注: 
1.在ECS上启动tomcat后，第一次访问时间特别长
```sql
2017-04-25 10:16:04 INFO com.world.socket.ServerSocketListener  
25-Apr-2017 10:18:48.171 INFO [localhost-startStop-1] org.apache.catalina.util.SessionIdGeneratorBase.createSecureRandom CreaecureRandom instance for session ID generation using [SHA1PRNG] took [163,521] milliseconds. 
 
 
这个session ID引起的 

解决办法：在JVM环境中解决 
打开$JAVA_PATH/jre/lib/security/java.security这个文件，找到下面的内容：securerandom.source=file:/dev/urandom 
替换成securerandom.source=file:/dev/./urandom
```
2.Centos打开、关闭、结束tomcat，及查看tomcat运行日志
```sql
启动：一般是执行sh tomcat/bin/startup.sh 
停止：一般是执行sh tomcat/bin/shutdown.sh脚本命令 
查看：执行ps -ef |grep tomcat 输出如下 *** 5144 。。。等等.Bootstrap start 说明tomcat已经正常启动， 5144 就为进程号 pid = 5144 
杀死：kill -9 5144


------------------------linux下实时查看tomcat运行日志-------------------------
1、先切换到：cd tomcat/logs
2、tail -f catalina.out
3、这样运行时就可以实时查看运行日志了
Ctrl+c 是退出tail命令。
```

##### 2.3.3 配置 Mysql环境
参考： http://www.linuxidc.com/Linux/2016-09/134992.htm

#### 2.4 投放项目文件
使用`fileZilla`进行传输文件
```sql
Tomcat中部署web项目的三种方式：
1.部署解包的webapp目录
2.打包的war文件
3.Manager Web应用程序
```