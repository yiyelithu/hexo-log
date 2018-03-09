---
title: Docker笔记
date: 2018-03-08 14:30:44
categories: 服务器
tags: [java,server]
---
#### 什么是Docker
Docker 是一种“轻量级”容器技术，它几乎动摇了传统虚拟化技术的地位，现在国内外已经有越来越多的公司开始逐步使用 Docker 来替换现有的虚拟化平台了。作为一名 Java 程序员，我们是时候一起把 Docker 学起来了！

1.传统虚拟化技术的体系架构：

![logo](/images/server/docker/virtual mechine.png) 

可见，我们在宿主机的操作系统上，可安装了多个虚拟机，而在每个虚拟机中，通过虚拟化技术，实现了一个虚拟操作系统，随后，就可以在该虚拟操作系统上，安装自己所需的应用程序了。这一切看似非常简单，但其中的技术细节是相当高深莫测的，大神级人物都不一定说得清楚。
<!--more-->

凡是使用过虚拟机的同学，应该都知道，启动虚拟机就像启动一台计算机，初始化过程是相当慢的，我们需要等很久，才能看到登录界面。一旦虚拟机启动以后，就可以与宿主机建立网络连接，确保虚拟机与宿主机之间是互联互通的。不同的虚拟机之间却是相互隔离的，也就是说，彼此并不知道对方的存在，但每个虚拟机占用的都是宿主机的硬件与网络资源。

2.Docker 技术的体系架构

![logo](/images/server/docker/virtual mechine.png) 

可见，在宿主机的操作系统上，有一个 Docker 服务在运行（或者称为“Docker 引擎”），在此服务上，我们可开启多个 Docker 容器，而每个 Docker 容器中可运行自己所需的应用程序，Docker 容器之间也是相互隔离的，同样地，都是占用的宿主机的硬件与网络资源。、

Docker 容器相对于虚拟机而言，除了在技术实现上完全不一样以外，启动速度较虚拟机而言有本质的飞跃，启动一个容器只在眨眼瞬间。不管是虚拟机还是 Docker 容器，它们都是为了隔离应用程序的运行环境，节省我们的硬件资源，为我们开发人员提供福利。


3.Docker 的 Logo:

![logo](/images/server/docker/docker logo.png) 

很明显，这是一只鲸鱼，它托着许多集装箱。我们可以把宿主机可当做这只鲸鱼，把相互隔离的容器可看成集装箱，每个集装箱中都包含自己的应用程序。这 Logo 简直的太形象了！

4.Docker的应用场景
```sql
1. Web 应用的自动化打包和发布。

2. 自动化测试和持续集成、发布。

3. 在服务型环境中部署和调整数据库或其他的后台应用。
```
5.Docker 的优点
```sql
1、简化程序：
Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的	任务，在Docker容器的处理下，只需要数秒就能完成。

2、避免选择恐惧症：
如果你有选择恐惧症，还是资深患者。Docker 帮你	打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。

3、节省开支：
一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。
```
#### Docker 术语

   |     术语 | 说明    | 
    | --------   | --------------------:   | 
    |  Docker 镜像(Images)       | Docker 镜像是用于创建 Docker 容器的模板。 |
    |  Docker 容器(Container)        | 容器是独立运行的一个或一组应用。     |
    |  Docker 客户端(Client)     | Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。 |
    |  Docker 主机(Host)      |一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。     |
    |  Docker 仓库(Registry) |      Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。|
    |  Docker Machine       | Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。      |


#### 使用Docker前先配置 Docker国内镜像或者使用registry-mirrors配置加速

由于国内访问直接访问Docker hub网速比较慢，拉取镜像的时间就会比较长。一般我们会使用镜像加速或者直接从国内的一些平台镜像仓库上拉取。 

```sql
方法一： 网易镜像中心：https://c.163.com/hub#/m/home/ 

拉取镜像的命令是： docker pull 镜像名字 所以我们可以按照给出的镜像名字或者命令直接拉取。

eg: docker pull hub.c.163.com/library/tomcat:latest


方法二： daocloud镜像市场：https://hub.daocloud.io/

如果说还是想从dockerhub上拉取，那么使用加速器修改docker的registry-mirrors。这里使用的是DaoCloud的加速器。 

首先在http://www.daocloud.io/进行注册登录。然后点击加速器，得到如下脚本

    curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://24524c4f.m.daocloud.io Copy
    该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同。更多详情请访问文档。
 
也可以自己手动修改 /etc/docker/daemon.json

{
 "registry-mirrors": ["http://ef017c13.m.daocloud.io"],
 "live-restore": true
}

最后重启docker service docker restart
```
#### 安装 Docker
1.前提条件
```sql
使用 yum 安装（CentOS 7下）

Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
通过 uname -r 命令查看你当前的内核版本

```
2.安装
```sql
yum -y install docker
```
3.启动服务
```sql
service docker start
```
4.测试运行 hello-world
```sql
docker run hello-world
```

#### Docker中使用CentOS7镜像
1.启动容器服务
```genericsql
systemctl start docker.service 
```
2.下载CentOS7 镜像
```genericsql
[root@JD docker]# docker pull centos:7.3.1611
Trying to pull repository docker.io/library/centos ... 
7.3.1611: Pulling from docker.io/library/centos

版本: https://hub.docker.com/_/centos/ 可以在这个网站上选择自己想要的版本
  latest, centos7, 7 (docker/Dockerfile)
  centos6, 6 (docker/Dockerfile)
  centos7.4.1708, 7.4.1708 (docker/Dockerfile)
  centos7.3.1611, 7.3.1611 (docker/Dockerfile)
  centos7.2.1511, 7.2.1511 (docker/Dockerfile)
  centos7.1.1503, 7.1.1503 (docker/Dockerfile)
  centos7.0.1406, 7.0.1406 (docker/Dockerfile)
  centos6.9, 6.9 (docker/Dockerfile)
  centos6.8, 6.8 (docker/Dockerfile)
  centos6.7, 6.7 (docker/Dockerfile)
  centos6.6, 6.6 (docker/Dockerfile)
```
3.下载成功之后查看本地所有的镜像，得到centos的 IMAGE ID: 66ee80d59a68
```sql
[root@JD ~]# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
docker.io/tomcat        latest              108db0e7c85e        2 weeks ago         557.4 MB
docker.io/hello-world   latest              f2a91732366c        3 months ago        1.848 kB
docker.io/centos        7.3.1611            66ee80d59a68        4 months ago        191.8 MB
```

4.启动docker中的CentOS7
```sql
docker run -ti 66ee /bin/bash
#6866 是 IMAGE ID 前四位数字-能区分出是哪个image即可

root@b4ad1d1c87da /]# 
#登录成功，接下来就可以为所欲为啦。

命令笔记
  容器是在镜像的基础上来运行的，一旦容器启动了，我们就可以登录到容器中，安装自己所需的软件或应用程序。既然镜像已经下载到本地，那么如何才能启动容器呢
  
  docker run -i -t -v /root/software/:/mnt/software/ 25c5298b1a36 /bin/bash

  docker run <相关参数> <镜像 ID> <初始命令>

    -i：表示以“交互模式”运行容器
    -t：表示容器启动后会进入其命令行
    -v：表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>
    假设我们的所有安装程序都放在了宿主机的/root/software/目录下，现在需要将其挂载到容器的/mnt/software/目录下。
    
  初始命令表示一旦容器启动，需要运行的命令，此时使用“/bin/bash”，表示什么也不做，只需进入命令行即可。



```
5.检查CentOS7系统
```sql
root@b4ad1d1c87da  /]# uname -a
Linux b4ad1d1c87da 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
[root@b4ad1d1c87da /]# cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```
6.退出
```sql
  ctrl+d 退出容器且关闭, 
  docker ps 查看无,
  ctrl+p+q 退出容器但不关闭, 
  docker ps
```
 
7.再进入CentOS7
```sql
[root@wxtest1607 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
b4ad1d1c87da        6866                "/bin/bash"         12 seconds ago      Up 9 seconds                            mad_swanson
                  drunk_hypatia
得到 CONTAINER ID         
[root@wxtest1607 ~]# docker exec -ti b4ad /bin/bash  
[root@b4ad1d1c87da /]#

```
8.安装tomcat 
```sql
 1. yum -y install tomcat
 
 注：
        在docker中通过systemctl 启动服务的时候总是报Failed to get D-Bus connection: Operation not permitted 这样的错误提示。
     解决方法：
        解决办法就是在docker run 的时候运行/usr/sbin/init 。比如：
        docker run -ti 66ee /usr/sbin/init
 2. 在Centos使用yum安装后，Tomcat相关的目录都已采用符号链接到/usr/share/tomcat6目录，包含webapps等，这很方便我们配置管理
```

转载：http://www.runoob.com/docker/docker-tutorial.html

转载：http://developer.51cto.com/art/201702/529956.htm

转载：http://www.jb51.net/article/112921.htm

转载：https://www.jianshu.com/p/0aa535e681f5