---
title: Git
date: 2017-10-22 10:45:12
categories: server
tags: [git] 
---
## 使用Git的前的初始配置

### 1. 配置提交时的用户名与邮件名称(注:只是标识本次commit是谁提交的)
1.1 通过命令的方式
```sql
$ git config --global user.name "songshuiyang"
$ git config --global user.email songshiuyang@foxmail.com

注: global 全局配置,在此电脑上的所有项目的git提交都会用这个用户名和邮件
```
1.2 通过修改配置文件的方式

```sql
文件路径: 用户目录/.gitconfig  文件
把name email改成(新增)自己的配置即可
[user]
	name = songshuiyang
	email = songshiuyang@foxmail.com
```
### 2. 配置 短命令
2.1 通过命令的方式
```sql
$ git config --global alias.st status
$ git config --global alias.ci commit
$ git congig --global alias.co checkout
$ git congig --global alias.br branch
```
2.2 通过修改配置文件的方式
```sql
[alias]
    co = checkout
    ci = commit
    st = status
    cm = commit -m
    br = branch
    bm = branch -m
    bd = branch -D
    cb = checkout -b
    df = diff
    ls = log --stat
    lp = log -p
    plo = pull origin
    plode = pull origin develop
    pho = push origin
```
### 3. 配置文件
>Git的三个配置文件
1. 版本库级别的配置文件,文件路径: `项目路径/.git/config`
2. 全局配置文件, 文件路径: `用户目录/.gitconfig` 
3. 系统级配置文件,文件路径: `安装目录/etc目录下`

优先级: 版本库级别的配置文件 >  全局配置文件  > 系统级配置文件
