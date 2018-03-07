---
title: Centos7安装Mysql5.7.md
date: 2018-03-53 20:30:44
categories: 服务器
tags: [java,server]
---
#### 一：配置YUM源
>官网地址 在MySQL官网中下载YUM源rpm安装包：http://dev.mysql.com/downloads/repo/yum/

1.下载mysql源安装包
```sql
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
 命令笔记:
```sql
wget:
用来从指定的URL下载文件。wget非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性，如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕。如果是服务器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用

-a<日志文件>：在指定的日志文件中记录资料的执行过程；
-A<后缀名>：指定要下载文件的后缀名，多个后缀名之间使用逗号进行分隔；
-b：进行后台的方式运行wget；
-B<连接地址>：设置参考的连接地址的基地地址；
-c：继续执行上次终端的任务；
-C<标志>：设置服务器数据块功能标志on为激活，off为关闭，默认值为on；
-d：调试模式运行指令；
-D<域名列表>：设置顺着的域名列表，域名之间用“，”分隔；
-e<指令>：作为文件“.wgetrc”中的一部分执行指定的指令；
-h：显示指令帮助信息；
-i<文件>：从指定文件获取要下载的URL地址；
-l<目录列表>：设置顺着的目录列表，多个目录用“，”分隔；
-L：仅顺着关联的连接；
-r：递归下载方式；
-nc：文件存在时，下载文件不覆盖原有文件；
-nv：下载时只显示更新和出错信息，不显示指令的详细执行过程；
-q：不显示指令执行过程；
-nh：不查询主机名称；
-v：显示详细执行过程；
-V：显示版本信息；
--passive-ftp：使用被动模式PASV连接FTP服务器；
--follow-ftp：从HTML文件中下载FTP连接文件

下载并以不同的文件名保存:
wget -O wordpress.zip http://www.linuxde.net/download.aspx?id=1080

```
<!--more-->

2.安装mysql源
```sql
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```

 命令笔记:
```sql
yum命令

是在Fedora和RedHat以及SUSE中基于rpm的软件包管理器，它可以使系统管理人员交互和自动化地更细与管理RPM软件包，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

选项:
-h：显示帮助信息；
-y：对所有的提问都回答“yes”；
-c：指定配置文件；
-q：安静模式；
-v：详细模式；
-d：设置调试等级（0-10）；
-e：设置错误等级（0-10）；
-R：设置yum处理一个命令的最大等待时间；
-C：完全从缓存中运行，而不去下载或者更新任何头文件。

参数：
install：安装rpm软件包；
update：更新rpm软件包；
check-update：检查是否有可用的更新rpm软件包；
remove：删除指定的rpm软件包；
list：显示软件包的信息；
search：检查软件包的信息；
info：显示指定的rpm软件包的描述信息和概要信息；
clean：清理yum过期的缓存；
shell：进入yum的shell提示符；
resolvedep：显示rpm软件包的依赖关系；
localinstall：安装本地的rpm软件包；
localupdate：显示本地rpm软件包进行更新；
deplist：显示rpm软件包的所有依赖关系。


实例
部分常用的命令包括：

自动搜索最快镜像插件：yum install yum-fastestmirror
安装yum图形窗口插件：yum install yumex
查看可能批量安装的列表：yum grouplist
```
3.检查mysql源是否安装成功
```sql
yum repolist enabled | grep "mysql.*-community.*"
```
![logo](/images/server/linux/mysql_install_success.png) 

看到上图所示表示mysql源安装成功。

可以修改vim /etc/yum.repos.d/mysql-community.repo源，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可 

#### 二 安装MySQL
```sql
 yum install mysql-community-server
```
命令笔记:
```sql
安装

yum install              #全部安装
yum install package1     #安装指定的安装包package1
yum groupinsall group1   #安装程序组group1

更新和升级

yum update               #全部更新
yum update package1      #更新指定程序包package1
yum check-update         #检查可更新的程序
yum upgrade package1     #升级指定程序包package1
yum groupupdate group1   #升级程序组group1


查找和显示

yum info package1      #显示安装包信息package1
yum list               #显示所有已经安装和可以安装的程序包
yum list package1      #显示指定程序包安装情况package1
yum groupinfo group1   #显示程序组group1信息yum search string 根据关键字string查找安装包

删除程序

yum remove &#124; erase package1   #删除程序包package1
yum groupremove group1             #删除程序组group1
yum deplist package1               #查看程序package1依赖情况
```

#### 三：启动MySQL服务
1.启动
```sql
 systemctl start mysqld
```
命令笔记：
```sql
systemctl

是系统服务管理器指令，它实际上将 service 和 chkconfig 这两个命令组合到一起。

任务	                旧指令	                        新指令
使某服务自动启动	    chkconfig --level 3 httpd on	systemctl enable httpd.service
使某服务不自动启动	chkconfig --level 3 httpd off	systemctl disable httpd.service
检查服务状态	        service httpd status	        systemctl status httpd.service （服务详细信息） systemctl is-active httpd.service （仅显示是否 Active)
显示所有已启动的服务	chkconfig --list	            systemctl list-units --type=service
启动某服务	        service httpd start	            systemctl start httpd.service
停止某服务	        service httpd stop	            systemctl stop httpd.service
重启某服务	        service httpd restart	        systemctl restart httpd.service

```
2.查看状态
```sql
查看MySQL的启动状态
systemctl status mysqld

输出：
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2018-03-07 21:14:55 CST; 18min ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 17338 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 17320 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 17343 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─17343 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

Mar 07 21:14:54 VM_0_8_centos systemd[1]: Starting MySQL Server...
Mar 07 21:14:55 VM_0_8_centos systemd[1]: Started MySQL Server.

```
3.开机启动
```sql
systemctl enable mysqld
systemctl daemon-reload
```
4.修改root本地登录密码
```sql
mysql安装完成之后，在/var/log/mysqld.log文件中给root生成了一个默认密码。通过下面的方式找到root默认密码，然后登录mysql进行修改：

1. 执行： 
      grep 'temporary password' /var/log/mysqld.log
   输出： 
      2018-03-07T13:01:08.963552Z 1 [Note] A temporary password is generated for root@localhost: zktt1wKFD.HN

   得到临时密码: zktt1wKFD.HN

2. 登录mysql: 
      mysql -uroot -p
   输入临时密码进入mysql命令行
3. 修改密码策略
      mysql5.7默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示ERROR 1819 (HY000): Your password does not satisfy the current policy requirements错误

   步骤1：不需要密码策略，添加/etc/my.cnf件中添加如下配置禁用即可：
   validate_password = off
   步骤2：重新启动mysql服务使配置生效：
   systemctl restart mysqld    
```
#### 四：开启远程连接
```sql
登入mysql
  mysql -uroot -p
  
使用mysql数据库
  use mysql;
  
开启远程连接（root 用户名，% 所有人都可以访问 ，password 密码）
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
  
  也可以添加一个新用户: 
  GRANT ALL PRIVILEGES ON *.* TO 'shuiyang'@'%' IDENTIFIED BY 'password!' WITH GRANT OPTION;
  
  FLUSH PRIVILEGES; 
  
重起mysql服务
  service mysqld restart
如果执行完以上步骤，还是不能远程连接，那么我们需要查看服务器的防火墙是否开启
  service iptables status
如果防火墙开启，请关闭
  service iptables stop
```
#### 五：配置默认编码为utf8
```sql
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下所示：

[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'

重新启动mysql服务，查看数据库默认编码如下所示：

mysql> show variables like '%character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```

备注：
```sql
默认配置文件路径： 
配置文件：/etc/my.cnf 
日志文件：/var/log//var/log/mysqld.log 
服务启动脚本：/usr/lib/systemd/system/mysqld.service 
socket文件：/var/run/mysqld/mysqld.pid
```

转载：https://www.linuxidc.com/Linux/2016-09/135288.htm
转载:http://blog.csdn.net/sun614345456/article/details/53672150