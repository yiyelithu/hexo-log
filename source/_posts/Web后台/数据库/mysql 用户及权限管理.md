---
title: mysql 用户及权限管理
date: 2018-03-07 22:24:12
categories: server
tags: [mysql] 
---
### 权限控制
授权语法：
```sql
    GRANT privileges (columns) ON what TO user IDENTIFIED BY "password" WITH GRANT OPTION
    
    权限列表:
    ALTER: 修改表和索引。
    CREATE: 创建数据库和表。
    DELETE: 删除表中已有的记录。
    DROP: 抛弃(删除)数据库和表。
    INDEX: 创建或抛弃索引。
    INSERT: 向表中插入新行。
    REFERENCE: 未用。
    SELECT: 检索表中的记录。
    UPDATE: 修改现存表记录。
    FILE: 读或写服务器上的文件。
    PROCESS: 查看服务器中执行的线程信息或杀死线程。
    RELOAD: 重载授权表或清空日志、主机缓存或表缓存。
    SHUTDOWN: 关闭服务器。
    ALL: 所有权限，ALL PRIVILEGES同义词。
    USAGE: 特殊的 "无权限" 权限。
    用 户账户包括 "username" 和 "host" 两部分，后者表示该用户被允许从何地接入。tom@'%' 表示任何地址，默认可以省略。还可以是 "tom@192.168.1.%"、"tom@%.abc.com" 等。数据库格式为 db@table，可以是 "test.*" 或 "*.*"，前者表示 test 数据库的所有表，后者表示所有数据库的所有表。
    子句 "WITH GRANT OPTION" 表示该用户可以为其他用户分配权限。 
```
<!--more-->
实例：

```sql
  use mysql

  1. 新建用户, 并赋予所有数据库权限
    GRANT ALL PRIVILEGES ON *.* TO 'username'@'host' IDENTIFIED BY 'password' WITH GRANT OPTION;
  
    说明:
      1. username - 你将创建的用户名, host - 指定该用户在哪个主机上可以登陆,如果是本地用户可用localhost, 如果想让该用户可以从任意远程主机登陆,可以使用通配符%. password - 该用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器.
      2. 指定helloworld数据库: GRANT ALL PRIVILEGES ON helloword.* TO 'username'@'host' IDENTIFIED BY 'password' WITH GRANT OPTION;
  
  2. 指定该用户只能执行 select 和 update 命令
    GRANT SELECT, UPDATE ON *.* TO 'username'@'%' IDENTIFIED BY 'password';
  
  3. 另外每当调整权限后，通常需要执行以下语句刷新权限：
    FLUSH PRIVILEGES;

  4. grant 普通数据用户，查询、插入、更新、删除 数据库中所有表数据的权利。
    grant select on testdb.* to common_user@’%’
    grant insert on testdb.* to common_user@’%’
    grant update on testdb.* to common_user@’%’
    grant delete on testdb.* to common_user@’%’
    或者，用一条 MySQL 命令来替代：
    grant select, insert, update, delete on testdb.* to common_user@’%’
```

### 用户
```sql
  1. 删除刚才创建的用户：
    DROP USER username@localhost;
  
  2. 查看用户创建是否成功
    select user,host from user ;
    
    +-----------+-----------+
    | user      | host      |
    +-----------+-----------+
    | root      | %         |
    | select    | %         |
    | server    | %         |
    | shuiyang  | %         |
    | user      | %         |
    | mysql.sys | localhost |
    +-----------+-----------+
    
  3. 查看select用户的授权
   show grants for select;
   
   MySQL [mysql]>  show grants for `select`;
   +---------------------------------------------+
   | Grants for select@%                         |
   +---------------------------------------------+
   | GRANT SELECT, UPDATE ON *.* TO 'select'@'%' |
   +---------------------------------------------+
   1 row in set (0.00 sec)
   
  4. 设置与更改用户密码
  
  SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword')
  
  如果是当前登陆用户用
  
  SET PASSWORD = PASSWORD("newpassword");
  
```
  
  