# 第四章：数据表操作

## 1. 创建数据表

​		创建MySQL数据表需要以下信息： 

- 表名

- 表字段名

- 定义每个表字段

  ​	以下为创建MySQL数据表的SQL通用语法：

```mysql
CREATE TABLE table_name (column_name column_type);
```

​		以下例子中我们将在 RUNOOB 数据库中创建数据表runoob_tbl：

```mysql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

​		实例解析：

- 如果你不想字段为 **NULL** 可以设置字段的属性为 **NOT NULL**， 在操作数据库时如果输入该字段的数据为**NULL** ，就会报错。
- AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
-  PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。 
- ENGINE 设置存储引擎，CHARSET 设置编码。

### 1.1 通过命令提示符创建表

​		通过 `mysql>` 命令窗口可以很简单的创建MySQL数据表。你可以使用 SQL 语句 **CREATE TABLE** 来创建数据表。

​		以下为创建数据表 runoob_tbl 实例: 

```mysql
root@host# mysql -u root -p
Enter password:*******
mysql> use RUNOOB;
Database changed
mysql> CREATE TABLE runoob_tbl(
   -> runoob_id INT NOT NULL AUTO_INCREMENT,
   -> runoob_title VARCHAR(100) NOT NULL,
   -> runoob_author VARCHAR(40) NOT NULL,
   -> submission_date DATE,
   -> PRIMARY KEY ( runoob_id )
   -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.16 sec)
mysql>
```

> **注意：**MySQL命令终止符为分号 ; 。

> **注意：** -> 是换行符标识，不要复制。

### 1.2 使用PHP脚本创建数据表

​		你可以使用 PHP 的 mysqli_query() 函数来创建已存在数据库的数据表。 

​		该函数有两个参数，在执行成功时返回 TRUE，否则返回 FALSE。 

​		语法如下：

```
mysqli_query(connection,query,resultmode);
```

​		参数如下：

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| *connection* | 必需。规定要使用的 MySQL 连接。                              |
| *query*      | 必需，规定查询字符串。                                       |
| *resultmode* | 可选。一个常量。可以是下列值中的任意一个：MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个）MYSQLI_STORE_RESULT（默认） |

​		以下实例使用了PHP脚本来创建数据表：

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
$sql = "CREATE TABLE runoob_tbl( ".
        "runoob_id INT NOT NULL AUTO_INCREMENT, ".
        "runoob_title VARCHAR(100) NOT NULL, ".
        "runoob_author VARCHAR(40) NOT NULL, ".
        "submission_date DATE, ".
        "PRIMARY KEY ( runoob_id ))ENGINE=InnoDB DEFAULT CHARSET=utf8; ";
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
    die('数据表创建失败: ' . mysqli_error($conn));
}
echo "数据表创建成功\n";
mysqli_close($conn);
?>
```

执行成功后，就可以通过命令行查看表结构：

![img](../../images/DataBase/MysQL/83B74D96-5A71-40DF-86CB-10E1A434B973.jpg)

## 2. 删除数据表

​		MySQL中删除数据表是非常容易操作的， 但是你再进行删除表操作时要非常小心，因为执行删除命令后所有数据都会消失。

​		以下为删除MySQL数据表的通用语法：

```
DROP TABLE table_name ;
```

------

### 2.1 在命令提示窗口中删除数据表

​		在`mysql>`命令提示窗口中删除数据表SQL语句为 **DROP TABLE** ：

​		以下实例删除了数据表runoob_tbl: 

```mysql
root@host# mysql -u root -p
Enter password:*******
mysql> use RUNOOB;
Database changed
mysql> DROP TABLE runoob_tbl
Query OK, 0 rows affected (0.8 sec)
mysql>
```

------

### 2.2 使用PHP脚本删除数据表

​		PHP使用 mysqli_query 函数来删除 MySQL 数据表。 

​		该函数有两个参数，在执行成功时返回 TRUE，否则返回 FALSE。

​		语法如下：

```
mysqli_query(connection,query,resultmode);
```

​		参数如下：

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| *connection* | 必需。规定要使用的 MySQL 连接。                              |
| *query*      | 必需，规定查询字符串。                                       |
| *resultmode* | 可选。一个常量。可以是下列值中的任意一个：MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个）MYSQLI_STORE_RESULT（默认） |

​		以下实例使用了PHP脚本删除数据表 runoob_tbl: 

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
$sql = "DROP TABLE runoob_tbl";
mysqli_select_db( $conn, 'RUNOOB' );
$retval = mysqli_query( $conn, $sql );
if(! $retval )
{
  die('数据表删除失败: ' . mysqli_error($conn));
}
echo "数据表删除成功\n";
mysqli_close($conn);
?>
```

​		执行成功后，我们使用以下命令，就看不到 runoob_tbl 表了：

```
mysql> show tables;
Empty set (0.01 sec)
```