# 第三章：语法

​		SQLite 是遵循一套独特的称为语法的规则和准则。本教程列出了所有基本的 SQLite 语法，向您提供了一个 SQLite 快速入门。

## 1. 大小写敏感性

​		有个重要的点值得注意，SQLite 是**不区分大小写**的，但也有一些命令是大小写敏感的，比如 **GLOB** 和 **glob** 在 SQLite 的语句中有不同的含义。

## 2. 注释

​		SQLite 注释是附加的注释，可以在 SQLite 代码中添加注释以增加其可读性，他们可以出现在任何空白处，包括在表达式内和其他 SQL 语句的中间，但它们不能嵌套。

​		SQL 注释以两个连续的 "-" 字符（ASCII 0x2d）开始，并扩展至下一个换行符（ASCII 0x0a）或直到输入结束，以先到者为准。

​		您也可以使用 C 风格的注释，以 "/*" 开始，并扩展至下一个 "*/" 字符对或直到输入结束，以先到者为准。SQLite的注释可以跨越多行。

```
sqlite>.help -- 这是一个简单的注释
```

## 3. SQLite 语句

所有的 SQLite 语句可以以任何关键字开始，如 SELECT、INSERT、UPDATE、DELETE、ALTER、DROP 等，所有的语句以分号（;）结束。

### 3.1 ANALYZE：

```
ANALYZE;
or
ANALYZE database_name;
or
ANALYZE database_name.table_name;
```

### 3.2 AND/OR：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  CONDITION-1 {AND|OR} CONDITION-2;
```

### 3.3 ALTER TABLE：

```
ALTER TABLE table_name ADD COLUMN column_def...;
```

### 3.4 ALTER TABLE （Rename）：

```
ALTER TABLE table_name RENAME TO new_table_name;
```

### 3.5 ATTACH DATABASE ：

```
ATTACH DATABASE 'DatabaseName' As 'Alias-Name';
```

### 3.6 BEGIN TRANSACTION：

```
BEGIN;
or
BEGIN EXCLUSIVE TRANSACTION;
```

### 3.7 BETWEEN：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name BETWEEN val-1 AND val-2;
```

### 3.8 COMMIT：

```
COMMIT;
```

### 3.9 CREATE INDEX：

```
CREATE INDEX index_name
ON table_name ( column_name COLLATE NOCASE );
```

### 3.10 CREATE UNIQUE INDEX：

```
CREATE UNIQUE INDEX index_name
ON table_name ( column1, column2,...columnN);
```

### 3.11 CREATE TABLE：

```
CREATE TABLE table_name(
   column1 datatype,
   column2 datatype,
   column3 datatype,
   .....
   columnN datatype,
   PRIMARY KEY( one or more columns )
);
```

### 3.12 CREATE TRIGGER：

```
CREATE TRIGGER database_name.trigger_name 
BEFORE INSERT ON table_name FOR EACH ROW
BEGIN 
   stmt1; 
   stmt2;
   ....
END;
```

### 3.13 CREATE VIEW ：

```
CREATE VIEW database_name.view_name  AS
SELECT statement....;
```

### 3.14 CREATE VIRTUAL TABLE：

```
CREATE VIRTUAL TABLE database_name.table_name USING weblog( access.log );
or
CREATE VIRTUAL TABLE database_name.table_name USING fts3( );
```

### 3.15 COMMIT TRANSACTION：

```
COMMIT;
```

### 3.16 COUNT：

```
SELECT COUNT(column_name)
FROM   table_name
WHERE  CONDITION;
```

### 3.17 DELETE：

```
DELETE FROM table_name
WHERE  {CONDITION};
```

### 3.18 DETACH DATABASE：

```
DETACH DATABASE 'Alias-Name';
```

### 3.19 DISTINCT：

```
SELECT DISTINCT column1, column2....columnN
FROM   table_name;
```

### 3.20 DROP INDEX：

```
DROP INDEX database_name.index_name;
```

### 3.21 DROP TABLE：

```
DROP TABLE database_name.table_name;
```

### 3.22 DROP VIEW：

```
DROP VIEW view_name;
```

### 3.23 DROP TRIGGER：

```
DROP TRIGGER trigger_name
```

### 3.24 EXISTS：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name EXISTS (SELECT * FROM   table_name );
```

### 3.25 EXPLAIN：

```
EXPLAIN INSERT statement...;
or 
EXPLAIN QUERY PLAN SELECT statement...;
```

### 3.26 GLOB：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name GLOB { PATTERN };
```

### 3.27 GROUP BY：

```
SELECT SUM(column_name)
FROM   table_name
WHERE  CONDITION
GROUP BY column_name;
```

### 3.28 HAVING：

```
SELECT SUM(column_name)
FROM   table_name
WHERE  CONDITION
GROUP BY column_name
HAVING (arithematic function condition);
```

### 3.29 INSERT INTO：

```
INSERT INTO table_name( column1, column2....columnN)
VALUES ( value1, value2....valueN);
```

### 3.30 IN：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name IN (val-1, val-2,...val-N);
```

### 3.31 Like：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name LIKE { PATTERN };
```

### 3.32 NOT IN：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  column_name NOT IN (val-1, val-2,...val-N);
```

### 3.33 ORDER BY：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  CONDITION
ORDER BY column_name {ASC|DESC};
```

### 3.34 PRAGMA：

```
PRAGMA pragma_name;

For example:

PRAGMA page_size;
PRAGMA cache_size = 1024;
PRAGMA table_info(table_name);
```

### 3.35 RELEASE SAVEPOINT：

```
RELEASE savepoint_name;
```

### 3.36 REINDEX：

```
REINDEX collation_name;
REINDEX database_name.index_name;
REINDEX database_name.table_name;
```

### 3.37 ROLLBACK：

```
ROLLBACK;
or
ROLLBACK TO SAVEPOINT savepoint_name;
```

### 3.38 SAVEPOINT：

```
SAVEPOINT savepoint_name;
```

### 3.39 SELECT：

```
SELECT column1, column2....columnN
FROM   table_name;
```

### 3.40 UPDATE：

```
UPDATE table_name
SET column1 = value1, column2 = value2....columnN=valueN
[ WHERE  CONDITION ];
```

### 3.41 VACUUM：

```
VACUUM;
```

### 3.42 WHERE：

```
SELECT column1, column2....columnN
FROM   table_name
WHERE  CONDITION;
```