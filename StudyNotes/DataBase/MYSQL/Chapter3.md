# 第三章：数据库操作

## 1. 创建数据库

​		我们可以在登陆 MySQL 服务后，使用 **create** 命令创建数据库，语法如下:

```
CREATE DATABASE 数据库名;
```

 		以下命令简单的演示了创建数据库的过程，数据名为 **RUNOOB**:

```powershell
[root@host]# mysql -u root -p   
Enter password:******  # 登录后进入终端

mysql> create DATABASE RUNOOB;
```

###  1.1 使用 mysqladmin 创建数据库

​		 使用普通用户，你可能需要特定的权限来创建或者删除 MySQL 数据库。

 		所以我们这边使用root用户登录，root用户拥有最高权限，可以使用 mysql **mysqladmin** 命令来创建数据库。

 		以下命令简单的演示了创建数据库的过程，数据名为 RUNOOB:

```
[root@host]# mysqladmin -u root -p create RUNOOB
Enter password:******
```

​		以上命令执行成功后会创建 MySQL 数据库 RUNOOB。

###  1.2 使用 PHP脚本 创建数据库 

 		PHP 使用 mysqli_query 函数来创建或者删除 MySQL 数据库。 

​		 该函数有两个参数，在执行成功时返回 TRUE，否则返回 FALSE。

​		语法如下：

```
mysqli_query(connection,query,resultmode);
```

 		参数如下：

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| *connection* | 必需。规定要使用的 MySQL 连接。                              |
| *query*      | 必需，规定查询字符串。                                       |
| *resultmode* | 可选。一个常量。可以是下列值中的任意一个： 	 MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个） 		MYSQLI_STORE_RESULT（默认） |

 以下实例演示了使用PHP来创建一个数据库：

 

```php
<?php
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
  die('连接错误: ' . mysqli_error($conn));
}
echo '连接成功<br />';
$sql = 'CREATE DATABASE RUNOOB';
$retval = mysqli_query($conn,$sql );
if(! $retval )
{
    die('创建数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 创建成功\n";
mysqli_close($conn);
?>
```

​		执行成功后，返回如下结果：



![img](..\..\images\DataBase\MySQL\84D4F5A6-445C-4AF7-8626-990BA176EC8D.jpg)

 

​		如果数据库已存在，执行后，返回如下结果：

 

![img](..\..\images\DataBase\MySQL\2FA2CCA3-B04E-4E99-BF4B-D36CBFBDB1A7.jpg)

### 1.3 推荐方法

​		使用root登录MySQL后，可以使用以下命令

```mysql
CREATE DATABASE IF NOT EXISTS RUNOOB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

​		创建数据库，该命令的作用：

- 1. 如果数据库不存在则创建，存在则不创建。 
- 2. 创建RUNOOB数据库，并设定编码集为utf8

## 2. 删除数据库

 		使用普通用户登陆 MySQL 服务器，你可能需要特定的权限来创建或者删除 MySQL 数据库，所以我们这边使用 root 用户登录，root 用户拥有最高权限。 

​		 在删除数据库过程中，务必要十分谨慎，因为在执行删除命令后，所有数据将会消失。

### 2.1 drop 命令删除数据库

​		drop 命令格式：

```
drop database <数据库名>;
```

​		例如删除名为 RUNOOB 的数据库：

```
mysql> drop database RUNOOB;
```

### 2.2 使用 mysqladmin 删除数据库

 		你也可以使用 mysql **mysqladmin** 命令在终端来执行删除命令。 

 		 以下实例删除数据库 RUNOOB(该数据库在前一章节已创建)：

```
[root@host]# mysqladmin -u root -p drop RUNOOB
Enter password:******
```

​		执行以上删除数据库命令后，会出现一个提示框，来确认是否真的删除数据库： 

```
Dropping the database is potentially a very bad thing to do.
Any data stored in the database will be destroyed.

Do you really want to drop the 'RUNOOB' database [y/N] y
Database "RUNOOB" dropped
```

### 2.3 使用PHP脚本删除数据库

​		PHP使用 mysqli_query 函数来创建或者删除 MySQL 数据库。 

​		 该函数有两个参数，在执行成功时返回 TRUE，否则返回 FALSE。 

​		语法如下：

```
mysqli_query(connection,query,resultmode);
```

​		 参数如下：

| 参数         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| *connection* | 必需。规定要使用的 MySQL 连接。                              |
| *query*      | 必需，规定查询字符串。                                       |
| *resultmode* | 可选。一个常量。可以是下列值中的任意一个： 	 MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个） 		MYSQLI_STORE_RESULT（默认） |

 		以下实例演示了使用PHP mysqli_query函数来删除数据库：

```php
<?php
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功<br />';
$sql = 'DROP DATABASE RUNOOB';
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('删除数据库失败: ' . mysqli_error($conn));
}
echo "数据库 RUNOOB 删除成功\n";
mysqli_close($conn);
?>
```

执行成功后，数结果为：

![img](../../images/DataBase/MySQL/8831EF0F-E958-45BE-A39C-35AC5B14C246.jpg)

> **注意：** 在使用PHP脚本删除数据库时，不会出现确认是否删除信息，会直接删除指定数据库，所以你在删除数据库时要特别小心。

## 3. 选择数据库

​		 在你连接到 MySQL 数据库后，可能有多个可以操作的数据库，所以你需要选择你要操作的数据库。 

### 3.1 从命令提示窗口中选择MySQL数据库

​		 在 `mysql>` 提示窗口中可以很简单的选择特定的数据库。你可以使用SQL命令来选择指定的数据库。

 		以下实例选取了数据库 RUNOOB:

```
[root@host]# mysql -u root -p
Enter password:******
mysql> use RUNOOB;
Database changed
mysql>
```

​		执行以上命令后，你就已经成功选择了 RUNOOB 数据库，在后续的操作中都会在 RUNOOB 数据库中执行。 

> >**注意:**所有的数据库名，表名，表字段都是区分大小写的。所以你在使用SQL命令时需要输入正确的名称。

### 3.2 使用PHP脚本选择MySQL数据库

 		PHP 提供了函数 mysqli_select_db  来选取一个数据库。函数在执行成功后返回 TRUE ，否则返回 FALSE 。  

​		语法如下：

```
mysqli_select_db(connection,dbname);
```

 		参数如下：

| 参数         | 描述                            |
| :----------- | :------------------------------ |
| *connection* | 必需。规定要使用的 MySQL 连接。 |
| *dbname*     | 必需，规定要使用的默认数据库。  |

​		以下实例展示了如何使用 mysqli_select_db 函数来选取一个数据库：

```php
<?php
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('连接失败: ' . mysqli_error($conn));
}
echo '连接成功';
mysqli_select_db($conn, 'RUNOOB' );
mysqli_close($conn);
?>
```

