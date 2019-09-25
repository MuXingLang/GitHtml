# MySQL安装

## 1.阿里云ECS安装mysql

### 1.1 XShell远程连接

```shell
Xshell 6 (Build 0149)
Copyright (c) 2002 NetSarang Computer, Inc. All rights reserved.

Type `help' to learn how to use Xshell prompt.
[D:\~]$ 

Connecting to 47.105.58.53:22022...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

WARNING! The remote SSH server rejected X11 forwarding request.
Last failed login: Sun Sep 22 11:16:25 CST 2019 from 219.218.22.247 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Fri Sep 20 09:07:07 2019 from 219.155.5.218

Welcome to Alibaba Cloud Elastic Compute Service !
```

​		以上为正常登入的情况

### 1.2 检查是否已安装

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# rpm -qa | grep mysql
```

​		以上即未安装mysql

### 1. 3下载Yum Repository

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
--2019-09-24 09:19:11--  http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
Resolving dev.mysql.com (dev.mysql.com)... 137.254.60.11
Connecting to dev.mysql.com (dev.mysql.com)|137.254.60.11|:80... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm [following]
--2019-09-24 09:19:11--  https://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
Connecting to dev.mysql.com (dev.mysql.com)|137.254.60.11|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://repo.mysql.com//mysql57-community-release-el7-10.noarch.rpm [following]
--2019-09-24 09:19:12--  https://repo.mysql.com//mysql57-community-release-el7-10.noarch.rpm
Resolving repo.mysql.com (repo.mysql.com)... 104.127.195.16
Connecting to repo.mysql.com (repo.mysql.com)|104.127.195.16|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 25548 (25K) [application/x-redhat-package-manager]
Saving to: ‘mysql57-community-release-el7-10.noarch.rpm’

100%[==========================================================================================>] 25,548      --.-K/s   in 0.06s   

2019-09-24 09:19:13 (451 KB/s) - ‘mysql57-community-release-el7-10.noarch.rpm’ saved [25548/25548]

-c: No such file or directory
No URLs found in -c.
FINISHED --2019-09-24 09:19:13--
Total wall clock time: 2.1s
Downloaded: 1 files, 25K in 0.06s (451 KB/s)
```

​		以上操作通过`wget`从MysSQL官网下载了文件`mysql57-community-release-el7-10.noarch.rpm`

### 1.4 安装Yum Repository

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# yum -y install mysql57-community-release-el7-10.noarch.rpm
Loaded plugins: fastestmirror
Examining mysql57-community-release-el7-10.noarch.rpm: mysql57-community-release-el7-10.noarch
Marking mysql57-community-release-el7-10.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package mysql57-community-release.noarch 0:el7-10 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================================================
 Package                              Arch              Version           Repository                                           Size
====================================================================================================================================
Installing:
 mysql57-community-release            noarch            el7-10            /mysql57-community-release-el7-10.noarch             30 k

Transaction Summary
====================================================================================================================================
Install  1 Package

Total size: 30 k
Installed size: 30 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql57-community-release-el7-10.noarch                                                                          1/1 
  Verifying  : mysql57-community-release-el7-10.noarch                                                                          1/1 

Installed:
  mysql57-community-release.noarch 0:el7-10                                                                                         

Complete!
```

​		以上操作即将该yum源安装到系统中

### 1.5 安装MySQL

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# yum -y install mysql-community-server
Loaded plugins: fastestmirror
Determining fastest mirrors
base                                                                                                         | 3.6 kB  00:00:00     
epel                                                                                                         | 5.4 kB  00:00:00     
extras                                                                                                       | 2.9 kB  00:00:00     
mysql-connectors-community                                                                                   | 2.5 kB  00:00:00     
mysql-tools-community                                                                                        | 2.5 kB  00:00:00     
mysql57-community                                                                                            | 2.5 kB  00:00:00     
updates                                                                                                      | 2.9 kB  00:00:00     
(1/9): extras/7/x86_64/primary_db                                                                            | 152 kB  00:00:00     
(2/9): base/7/x86_64/group_gz                                                                                | 165 kB  00:00:00     
(3/9): epel/x86_64/updateinfo                                                                                | 1.0 MB  00:00:00     
(4/9): base/7/x86_64/primary_db                                                                              | 6.0 MB  00:00:00     
(5/9): epel/x86_64/primary_db                                                                                | 6.8 MB  00:00:00     
(6/9): updates/7/x86_64/primary_db                                                                           | 1.1 MB  00:00:00     
(7/9): mysql-connectors-community/x86_64/primary_db                                                          |  44 kB  00:00:00     
(8/9): mysql-tools-community/x86_64/primary_db                                                               |  61 kB  00:00:00     
(9/9): mysql57-community/x86_64/primary_db                                                                   | 184 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package mysql-community-server.x86_64 0:5.7.27-1.el7 will be installed
--> Processing Dependency: mysql-community-common(x86-64) = 5.7.27-1.el7 for package: mysql-community-server-5.7.27-1.el7.x86_64
--> Processing Dependency: mysql-community-client(x86-64) >= 5.7.9 for package: mysql-community-server-5.7.27-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.4)(64bit) for package: mysql-community-server-5.7.27-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.1)(64bit) for package: mysql-community-server-5.7.27-1.el7.x86_64
--> Processing Dependency: libaio.so.1()(64bit) for package: mysql-community-server-5.7.27-1.el7.x86_64
--> Running transaction check
---> Package libaio.x86_64 0:0.3.109-13.el7 will be installed
---> Package mysql-community-client.x86_64 0:5.7.27-1.el7 will be installed
--> Processing Dependency: mysql-community-libs(x86-64) >= 5.7.9 for package: mysql-community-client-5.7.27-1.el7.x86_64
---> Package mysql-community-common.x86_64 0:5.7.27-1.el7 will be installed
--> Running transaction check
---> Package mariadb-libs.x86_64 1:5.5.60-1.el7_5 will be obsoleted
--> Processing Dependency: libmysqlclient.so.18()(64bit) for package: 2:postfix-2.10.1-7.el7.x86_64
--> Processing Dependency: libmysqlclient.so.18(libmysqlclient_18)(64bit) for package: 2:postfix-2.10.1-7.el7.x86_64
---> Package mysql-community-libs.x86_64 0:5.7.27-1.el7 will be obsoleting
--> Running transaction check
---> Package mysql-community-libs-compat.x86_64 0:5.7.27-1.el7 will be obsoleting
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================================================
 Package                                   Arch                 Version                       Repository                       Size
====================================================================================================================================
Installing:
 mysql-community-libs                      x86_64               5.7.27-1.el7                  mysql57-community               2.2 M
     replacing  mariadb-libs.x86_64 1:5.5.60-1.el7_5
 mysql-community-libs-compat               x86_64               5.7.27-1.el7                  mysql57-community               2.0 M
     replacing  mariadb-libs.x86_64 1:5.5.60-1.el7_5
 mysql-community-server                    x86_64               5.7.27-1.el7                  mysql57-community               165 M
Installing for dependencies:
 libaio                                    x86_64               0.3.109-13.el7                base                             24 k
 mysql-community-client                    x86_64               5.7.27-1.el7                  mysql57-community                24 M
 mysql-community-common                    x86_64               5.7.27-1.el7                  mysql57-community               275 k

Transaction Summary
====================================================================================================================================
Install  3 Packages (+3 Dependent packages)

Total download size: 194 M
Downloading packages:
(1/6): libaio-0.3.109-13.el7.x86_64.rpm                                                                      |  24 kB  00:00:00     
warning: /var/cache/yum/x86_64/7/mysql57-community/packages/mysql-community-common-5.7.27-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
Public key for mysql-community-common-5.7.27-1.el7.x86_64.rpm is not installed
(2/6): mysql-community-common-5.7.27-1.el7.x86_64.rpm                                                        | 275 kB  00:00:00     
(3/6): mysql-community-libs-5.7.27-1.el7.x86_64.rpm                                                          | 2.2 MB  00:00:00     
(4/6): mysql-community-libs-compat-5.7.27-1.el7.x86_64.rpm                                                   | 2.0 MB  00:00:00     
(5/6): mysql-community-client-5.7.27-1.el7.x86_64.rpm                                                        |  24 MB  00:00:02     
(6/6): mysql-community-server-5.7.27-1.el7.x86_64.rpm                                                        | 165 MB  00:00:14     
------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                12 MB/s | 194 MB  00:00:16     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Importing GPG key 0x5072E1F5:
 Userid     : "MySQL Release Engineering <mysql-build@oss.oracle.com>"
 Fingerprint: a4a9 4068 76fc bd3c 4567 70c8 8c71 8d3b 5072 e1f5
 Package    : mysql57-community-release-el7-10.noarch (installed)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql-community-common-5.7.27-1.el7.x86_64                                                                       1/7 
  Installing : mysql-community-libs-5.7.27-1.el7.x86_64                                                                         2/7 
  Installing : mysql-community-client-5.7.27-1.el7.x86_64                                                                       3/7 
  Installing : libaio-0.3.109-13.el7.x86_64                                                                                     4/7 
  Installing : mysql-community-server-5.7.27-1.el7.x86_64                                                                       5/7 
  Installing : mysql-community-libs-compat-5.7.27-1.el7.x86_64                                                                  6/7 
  Erasing    : 1:mariadb-libs-5.5.60-1.el7_5.x86_64                                                                             7/7 
  Verifying  : mysql-community-libs-compat-5.7.27-1.el7.x86_64                                                                  1/7 
  Verifying  : mysql-community-common-5.7.27-1.el7.x86_64                                                                       2/7 
  Verifying  : mysql-community-libs-5.7.27-1.el7.x86_64                                                                         3/7 
  Verifying  : mysql-community-server-5.7.27-1.el7.x86_64                                                                       4/7 
  Verifying  : mysql-community-client-5.7.27-1.el7.x86_64                                                                       5/7 
  Verifying  : libaio-0.3.109-13.el7.x86_64                                                                                     6/7 
  Verifying  : 1:mariadb-libs-5.5.60-1.el7_5.x86_64                                                                             7/7 

Installed:
  mysql-community-libs.x86_64 0:5.7.27-1.el7                     mysql-community-libs-compat.x86_64 0:5.7.27-1.el7                  
  mysql-community-server.x86_64 0:5.7.27-1.el7                  

Dependency Installed:
  libaio.x86_64 0:0.3.109-13.el7    mysql-community-client.x86_64 0:5.7.27-1.el7    mysql-community-common.x86_64 0:5.7.27-1.el7   

Replaced:
  mariadb-libs.x86_64 1:5.5.60-1.el7_5                                                                                              

Complete!
```

​		以上即通过yum方式安装MySQL

### 1.6 启动MySQL

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# systemctl start mysqld.service
```

​		通过以上命令启动MySQL服务

### 1.7 查看MySQL运行状态

```bash
[root@iZm5e8c13rno7bir9cx8sqZ ~]# systemctl status mysqld.service
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2019-09-24 09:23:12 CST; 17s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 5344 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 5271 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 5347 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─5347 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

Sep 24 09:23:06 iZm5e8c13rno7bir9cx8sqZ systemd[1]: Starting MySQL Server...
Sep 24 09:23:12 iZm5e8c13rno7bir9cx8sqZ systemd[1]: Started MySQL Server.
```

​		如上所示，MySQL服务正在运行

### 1.8 查看密码

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# grep "password" /var/log/mysqld.log 
2019-09-24T01:23:08.030808Z 1 [Note] A temporary password is generated for root@localhost: LpsmHd:Pw8N7
```

​		通过以上命令可查看MySQL的初始密码

### 1.9 登录MySQL

```shell
[root@iZm5e8c13rno7bir9cx8sqZ ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.27

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

​		通过用户名密码可以通过`mysql -u root -p`命令登录MySQL

### 1.10 查看所有数据库；

```shell
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

​		如上所示，登陆之后，MySQL拒绝了我查看所有数据库的命令

​		网查解决方式如下，：

​		MySQL版本5.7.6版本以前用户可以使用如下命令：

```
mysql> SET PASSWORD = PASSWORD('Xiaoming250'); 
```

​		MySQL版本5.7.6版本开始的用户可以使用如下命令：

```
mysql> ALTER USER USER() IDENTIFIED BY 'Xiaoming250';
```

> 版权声明：本文为CSDN博主「不忘初_心」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/muziljx/article/details/81541896

### 1.11 修改密码

```shell
mysql> ALTER USER USER() IDENIFIED BY '123456';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'IDENIFIED BY '123456'' at line 1
mysql> set PASSWORD = PASSWORD('123456');
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> set PASSWORD = PASSWORD('1Qw121saa$12@1213');
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

​		如上所示

### 1.12 再次查询所有数据库

```shell
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

### 1.13 退出MySQL

```shell
mysql> exit;
Bye
[root@iZm5e8c13rno7bir9cx8sqZ ~]# 
```

### 1.14 说明

> 参考文档：Linux之yum安装MySQL - 简书  
>
> 在线地址：https://www.jianshu.com/p/136003ffce41

