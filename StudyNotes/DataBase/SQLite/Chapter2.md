# 第二章：命令

​		本章将向您讲解 SQLite 编程人员所使用的简单却有用的命令。这些命令被称为 SQLite 的点命令，这些命令的不同之处在于它们不以分号 ; 结束。

​		让我们在命令提示符下键入一个简单的 **sqlite3** 命令，在 SQLite 命令提示符下，您可以使用各种 SQLite 命令。

```
$ sqlite3
SQLite version 3.3.6
Enter ".help" for instructions
sqlite>
```

​		如需获取可用的点命令的清单，可以在任何时候输入 ".help"。例如：

```
sqlite>.help
```

​		上面的命令会显示各种重要的 SQLite 点命令的列表，如下所示：

| 命令                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| .backup ?DB? FILE     | 备份 DB 数据库（默认是 "main"）到 FILE 文件。                |
| .bail ON\|OFF         | 发生错误后停止。默认为 OFF。                                 |
| .databases            | 列出数据库的名称及其所依附的文件。                           |
| .dump ?TABLE?         | 以 SQL 文本格式转储数据库。如果指定了 TABLE 表，则只转储匹配 LIKE 模式的 TABLE 表。 |
| .echo ON\|OFF         | 开启或关闭 echo 命令。                                       |
| .exit                 | 退出 SQLite 提示符。                                         |
| .explain ON\|OFF      | 开启或关闭适合于 EXPLAIN 的输出模式。如果没有带参数，则为 EXPLAIN on，及开启 EXPLAIN。 |
| .header(s) ON\|OFF    | 开启或关闭头部显示。                                         |
| .help                 | 显示消息。                                                   |
| .import FILE TABLE    | 导入来自 FILE 文件的数据到 TABLE 表中。                      |
| .indices ?TABLE?      | 显示所有索引的名称。如果指定了 TABLE 表，则只显示匹配 LIKE 模式的 TABLE 表的索引。 |
| .load FILE ?ENTRY?    | 加载一个扩展库。                                             |
| .log FILE\|off        | 开启或关闭日志。FILE 文件可以是 stderr（标准错误）/stdout（标准输出）。 |
| .mode MODE            | 设置输出模式，MODE 可以是下列之一： **csv** 逗号分隔的值**column** 左对齐的列**html** HTML 的 <table> 代码**insert** TABLE 表的 SQL 插入（insert）语句**line** 每行一个值**list** 由 .separator 字符串分隔的值**tabs** 由 Tab 分隔的值**tcl** TCL 列表元素 |
| .nullvalue STRING     | 在 NULL 值的地方输出 STRING 字符串。                         |
| .output FILENAME      | 发送输出到 FILENAME 文件。                                   |
| .output stdout        | 发送输出到屏幕。                                             |
| .print STRING...      | 逐字地输出 STRING 字符串。                                   |
| .prompt MAIN CONTINUE | 替换标准提示符。                                             |
| .quit                 | 退出 SQLite 提示符。                                         |
| .read FILENAME        | 执行 FILENAME 文件中的 SQL。                                 |
| .schema ?TABLE?       | 显示 CREATE 语句。如果指定了 TABLE 表，则只显示匹配 LIKE 模式的 TABLE 表。 |
| .separator STRING     | 改变输出模式和 .import 所使用的分隔符。                      |
| .show                 | 显示各种设置的当前值。                                       |
| .stats ON\|OFF        | 开启或关闭统计。                                             |
| .tables ?PATTERN?     | 列出匹配 LIKE 模式的表的名称。                               |
| .timeout MS           | 尝试打开锁定的表 MS 毫秒。                                   |
| .width NUM NUM        | 为 "column" 模式设置列宽度。                                 |
| .timer ON\|OFF        | 开启或关闭 CPU 定时器。                                      |

​		让我们尝试使用 **.show** 命令，来查看 SQLite 命令提示符的默认设置。

```
sqlite>.show
     echo: off
  explain: off
  headers: off
     mode: column
nullvalue: ""
   output: stdout
separator: "|"
    width:
sqlite>
```

> 确保 sqlite> 提示符与点命令之间没有空格，否则将无法正常工作。

## 1. 格式化输出

​		您可以使用下列的点命令来格式化输出为本教程下面所列出的格式：

```
sqlite>.header on
sqlite>.mode column
sqlite>.timer on
sqlite>
```

​		上面设置将产生如下格式的输出：

```
ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------
1           Paul        32          California  20000.0
2           Allen       25          Texas       15000.0
3           Teddy       23          Norway      20000.0
4           Mark        25          Rich-Mond   65000.0
5           David       27          Texas       85000.0
6           Kim         22          South-Hall  45000.0
7           James       24          Houston     10000.0
CPU Time: user 0.000000 sys 0.000000
```

## 2. sqlite_master 表格

​		主表中保存数据库表的关键信息，并把它命名为 **sqlite_master**。如要查看表概要，可按如下操作：

```
sqlite>.schema sqlite_master
```

​		这将产生如下结果：

```
CREATE TABLE sqlite_master (
  type text,
  name text,
  tbl_name text,
  rootpage integer,
  sql text
);
```