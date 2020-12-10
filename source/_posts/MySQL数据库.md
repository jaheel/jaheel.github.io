---
title: MySQL数据库知识点汇总
tags:
  - MySQL
  - bash

categories: SQL
comments: false
---
# 1 数据库基础

## 1.1 数据库简介

数据管理的有效技术，是由一批数据构成的有序集合，数据保存在结构化的数据表里。

1. 数据库：用于存储数据的地方
2. 数据库管理系统：用于管理数据库的软件
3. 数据库应用程序：为了提高数据库系统的处理能力所使用的管理数据库库的软件补充



数据库管理系统(Database Management System, DBMS)

结构化查询语言(Structured Query Language)

## 1.2 存储结构

指数据库中的物理数据和逻辑数据的表示形式、物理数据和逻辑数据之间关系映射方式的描述。

物理数据和逻辑数据之间的转换通过数据库管理系统实现。

### 1.2.1 物理数据

数据在存储设备上的存储方式，实际存放在存储设备上的数据（物理记录）

有序存储、无序存储

单位：

> bit
>
> byte
>
> word：若干byte组成一个word（例如计算机字长8位、16位、32位等）
>
> block：内存储器、外存储器交换信息的最小单位（物理块、物理记录）（256字节、512字节、1024字节等）
>
> volume：一台输入输出设备所能装载的全部有用信息（卷）
>
> unordered：数据记录按照插入的顺序进行存储

### 1.2.2 逻辑数据

用户或程序员用于操作的数据形式，（逻辑记录）



1. 客观现实世界术语：

   * 实体(entity)

   * 实体集(entities)

     > 特性完全相同的同类实体的集合

   * 属性(attribute)

     > 实体的特性成为属性

   * 标识符(identifier)

     > 能够唯一地标识每个实体地属性或属性集

2. 关系型数据库管理系统：

   * 数据项(data item)：也称为字段(field)

     > 标记实体属性地可以命名的最小信息单位

   * 元组(tuple)

     >  也成为记录(record)，数据项的集合称为元组

   * 关系(relation)

     > 同一类元组所在的集合称为关系

   * 键码(key)

     > 能够唯一地标识关系中每个元组地数据项或数据项的组合称为关系的键码


<!--more-->

## 1.3 SQL语言

1. 数据定义语言(DDL)：DROP、CREATE、ALTER等语句
2. 数据操作语言(DML)：INSERT、UPDATE、DELETE等语句
3. 数据查询语言(DQL)：SELECT语句
4. 数据控制语句(DCL)：GRANT、REVOKE、COMMIT、ROLLBACK等语句

## 1.4 数据库访问接口

### 1.4.1 ODBC

全称：Open Database Connectivity，开放数据库互联

1. 访问不同SQL数据库提供了一个共同的接口
2. 使用SQL作为访问数据的标准
3. 一个应用程序可以通过共同的一组代码访问不同的SQL数据库管理系统

### 1.4.2 JDBC

全称：Java Data Base(Java数据库连接)

1. Java应用程序连接数据库的标准方法
2. 用于执行SQL语句的Java API

### 1.4.3 ADO.NET

微软在.NET框架下开发设计的一组用于和数据源进行交互额面向对象类库。

### 1.4.4 PDO

全称：PHP Data Object

为PHP访问数据库定义了一个轻量级的、一致性的接口，它提供了一个数据访问抽象层，这样，无论使用什么数据库，都可以通过一致的函数执行查询和获取数据。

# 2 MySQL数据库命令

## 2.1 数据库操作

### 2.1.1 创建数据库

```mysql
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];
```

### 2.1.2 查看数据库

```mysql
SHOW DATABASES [LIKE '数据库名'];
```

### 2.1.3 修改数据库

```mysql
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
```

### 2.1.4 删除数据库

```mysql
DROP DATABASE [ IF EXISTS ] <数据库名>
```

### 2.1.5 选择数据库

```mysql
USE <数据库名>
```

## 2.2 MySQL存储引擎

| 功能         | MylSAM | MEMORY | InnoDB | Archive |
| ------------ | ------ | ------ | ------ | ------- |
| 存储限制     | 256TB  | RAM    | 64TB   | None    |
| 支持事务     | No     | No     | Yes    | No      |
| 支持全文索引 | Yes    | No     | No     | No      |
| 支持树索引   | Yes    | Yes    | Yes    | No      |
| 支持哈希索引 | No     | Yes    | No     | No      |
| 支持数据缓存 | No     | N/A    | Yes    | No      |
| 支持外键     | No     | No     | Yes    | No      |

修改默认存储引擎：

```mysql
SET default_storage_engine=< 存储引擎名 >
```

## 2.3 数据类型

### 2.3.1 数值类型

整数类型：

| 类型名称      | 说明           | 存储需求 |
| ------------- | -------------- | -------- |
| TINYINT       | 很小的整数     | 1个字节  |
| SMALLINT      | 小的整数       | 2个宇节  |
| MEDIUMINT     | 中等大小的整数 | 3个字节  |
| INT (INTEGHR) | 普通大小的整数 | 4个字节  |
| BIGINT        | 大整数         | 8个字节  |

浮点数类型：FLOAT、DOUBLE

定点数类型：DECIMAL(DECIMAL 如果不指定精度，默认为（10，0）)

| 类型名称            | 说明               | 存储需求   |
| ------------------- | ------------------ | ---------- |
| FLOAT               | 单精度浮点数       | 4 个字节   |
| DOUBLE              | 双精度浮点数       | 8 个字节   |
| DECIMAL (M, D)，DEC | 压缩的“严格”定点数 | M+2 个字节 |

### 2.3.2 日期/时间类型

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |

### 2.3.3 字符串类型

| 类型名称   | 说明                                         | 存储需求                                                   |
| ---------- | -------------------------------------------- | ---------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                       | M 字节，1<=M<=255                                          |
| VARCHAR(M) | 变长非二进制字符串                           | L+1字节，在此，L< = M和 1<=M<=255                          |
| TINYTEXT   | 非常小的非二进制字符串                       | L+1字节，在此，L<2^8                                       |
| TEXT       | 小的非二进制字符串                           | L+2字节，在此，L<2^16                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                     | L+3字节，在此，L<2^24                                      |
| LONGTEXT   | 大的非二进制字符串                           | L+4字节，在此，L<2^32                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值             | 1或2个字节，取决于枚举值的数目 (最大值为65535)             |
| SET        | 一个设置，字符串对象可以有零个或 多个SET成员 | 1、2、3、4或8个字节，取决于集合 成员的数量（最多64个成员） |

PS:

1. VARCHAR(50) 定义了一个最大长度为 50 的字符串，如果插入的字符串只有 10 个字符，则实际存储的字符串为 10 个字符和一个字符串结束字符。VARCHAR 在值保存和检索时尾部的空格仍保留。



### 2.3.4 二进制类型

| 类型名称       | 说明                 | 存储需求               |
| -------------- | -------------------- | ---------------------- |
| BIT(M)         | 位字段类型           | 大约 (M+7)/8 字节      |
| BINARY(M)      | 固定长度二进制字符串 | M 字节                 |
| VARBINARY (M)  | 可变长度二进制字符串 | M+1 字节               |
| TINYBLOB (M)   | 非常小的BLOB         | L+1 字节，在此，L<2^8  |
| BLOB (M)       | 小 BLOB              | L+2 字节，在此，L<2^16 |
| MEDIUMBLOB (M) | 中等大小的BLOB       | L+3 字节，在此，L<2^24 |
| LONGBLOB (M)   | 非常大的BLOB         | L+4 字节，在此，L<2^32 |

## 2.4 数据表操作

### 2.4.1 创建数据表

```mysql
CREATE TABLE <表名> ([表定义选项])[表选项][分区选项];
```

查看表结构：

```mysql
DESCRIBE <表名>;
或
DESC <表名>;
```

显示创建表时的CREATE TABLE语句：

```mysql
SHOW CREATE TABLE <表名>\G；
```

### 2.4.2 修改数据表

```mysql
ALTER TABLE <表名> [修改选项]
```

修改选项语法格式：

```mysql
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名> }
```



添加字段：

```mysql
ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] [FIRST|AFTER 已存在的字段名]；
```



修改字段数据类型：

```mysql
ALTER TABLE <表名> MODIFY <字段名> <数据类型>
```

删除字段：

```mysql
ALTER TABLE <表名> DROP <字段名>；
```

修改字段名称：

```mysql
ALTER TABLE <表名> CHANGE <旧字段名> <新字段名> <新数据类型>；
```

修改表名：

```mysql
ALTER TABLE <旧表名> RENAME [TO] <新表名>；
```

### 2.4.3 删除数据表

```mysql
DROP TABLE [IF EXISTS] 表名1 [ ,表名2, 表名3 ...]
```

### 2.4.4 主键约束

定义列的同时指定主键：

```mysql
<字段名> <数据类型> PRIMARY KEY [默认值]
```

定义完所有列之后，指定主键的语法格式：

```mysql
[CONSTRAINT <约束名>] PRIMARY KEY [字段名]
```

设置复合主键：

```mysql
PRIMARY KEY [字段1，字段2，…,字段n]
```

修改表时添加主键约束：

```mysql
ALTER TABLE <数据表名> ADD PRIMARY KEY(<列名>);
```

### 2.4.5 外键约束

外键约束（FOREIGN KEY）用来在两个表的数据之间建立链接，它可以是一列或者多列。一个表可以有一个或多个外键。

外键对应的是参照完整性，一个表的外键可以为空值，若不为空值，则每一个外键的值必须等于另一个表中主键某个值。

外键是表的一个字段，不是本表的主键，但对应另一个表的主键。定义外键后，不允许删除另一个表中具有关联关系的行。

外键的主要作用是保持数据的一致性、完整性。



创建表时设置外键约束：

```mysql
[CONSTRAINT <外键名>] FOREIGN KEY 字段名 [，字段名2，…]
REFERENCES <主表名> 主键列1 [，主键列2，…]

例如：
FOREIGN KEY(deptId) REFERENCES tb_dept1(id)
```



修改表时添加外键约束：

```mysql
ALTER TABLE <数据表名> ADD CONSTRAINT <索引名>
FOREIGN KEY(<列名>) REFERENCES <主表名> (<列名>);

例如：
mysql> ALTER TABLE tb_emp2
    -> ADD CONSTRAINT fk_tb_dept1
    -> FOREIGN KEY(deptId)
    -> REFERENCES tb_dept1(id);
```



删除外键约束：

```mysql
ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;

例如：
mysql> ALTER TABLE tb_emp2
    -> DROP FOREIGN KEY fk_tb_dept1;
```

### 2.4.6 唯一约束

Unique Key

要求该列唯一，允许为空，但只能出现一个空值。

唯一约束可以确保一列或者几列不出现重复值。



创建表时设置唯一约束：

```mysql
<字段名> <数据类型> UNIQUE
```

PS：UNIQUE 和 PRIMARY KEY 的区别：一个表可以有多个字段声明为 UNIQUE，但只能有一个 PRIMARY KEY 声明；声明为 PRIMAY KEY 的列不允许有空值，但是声明为 UNIQUE 的字段允许空值的存在。



修改表时添加唯一约束：

```mysql
ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
```



删除唯一约束：

```mysql
ALTER TABLE <表名> DROP INDEX <唯一约束名>;
```

### 2.4.7 检查约束

选取设置检查约束的字段：

```mysql
CHECK <表达式>
```



创建表时设置检查约束：

```mysql
CHECK(<检查约束>)

CHECK(salary>0 AND salary<100)
```



修改表时添加检查约束：

```mysql
ALTER TABLE tb_emp7 ADD CONSTRAINT <检查约束名> CHECK(<检查约束>)
```



删除检查约束：

```mysql
ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;
```

### 2.4.8 默认值约束

创建表时设置默认值约束：

```mysql
<字段名> <数据类型> DEFAULT <默认值>;
```



修改表时添加默认值约束：

```mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
```



删除默认值约束：

```mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
```

### 2.4.8 非空约束

```mysql
创建时添加：
<字段名> <数据类型> NOT NULL;


修改时添加：
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名>
<字段名> <数据类型> NOT NULL;

删除：
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
```

## 2.5 增、删、改、查

### 2.5.1 查询数据表

```mysql
SELECT
{* | <字段列名>}
[
FROM <表 1>, <表 2>…
[WHERE <表达式>
[GROUP BY <group by definition>
[HAVING <expression> [{<operator> <expression>}…]]
[ORDER BY <order by definition>]
[LIMIT[<offset>,] <row count>]
]
```

其中，各条子句的含义如下：

- `{*|<字段列名>}`包含星号通配符的字段列表，表示查询的字段，其中字段列至少包含一个字段名称，如果要查询多个字段，多个字段之间要用逗号隔开，最后一个字段后不要加逗号。
- `FROM <表 1>，<表 2>…`，表 1 和表 2 表示查询数据的来源，可以是单个或多个。
- WHERE 子句是可选项，如果选择该项，将限定查询行必须满足的查询条件。
- `GROUP BY< 字段 >`，该子句告诉 MySQL 如何显示查询出来的数据，并按照指定的字段分组。
- `[ORDER BY< 字段 >]`，该子句告诉 MySQL 按什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC）。
- `[LIMIT[，]]`，该子句告诉 MySQL 每次显示查询出来的数据条数。



```mysql
SELECT DISTINCT <字段名> FROM <表名>;
//DISTINCT设置去重
```

### 2.5.2 设置别名

```mysql
<表名> [AS] <别名>
<列名> [AS] <列别名>
```

### 2.5.3 排序查询结果

```mysql
ORDER BY {<列名> | <表达式> | <位置>} [ASC|DESC]
```

### 2.5.4 条件查询

单一条件查询：

```mysql
mysql> SELECT name,height
    -> FROM tb_students_info
    -> WHERE height=170;
```

多条件查询：

```mysql
mysql> SELECT * FROM tb_students_info
    -> WHERE age>21 AND height>=175;
```

使用LIKE模糊查询：

```mysql
<表达式1> [NOT] LIKE <表达式2>
```

1. 百分号(%)

   > 百分号是 MySQL 中常用的一种通配符，在过滤条件中，百分号可以表示任何字符串，并且该字符串可以出现任意次。

2. 下划线(_)

   > 匹配单个字符

```mysql
mysql> SELECT name FROM tb_students_info
    -> WHERE name LIKE 'T%';
    
mysql> SELECT name FROM tb_students_info
    -> WHERE name LIKE '____y';
```

### 2.5.5 运算符

| 优先级由低到高排列 | 运算符                                                       |
| ------------------ | ------------------------------------------------------------ |
| 1                  | =(赋值运算）、:=                                             |
| 2                  | II、OR                                                       |
| 3                  | XOR                                                          |
| 4                  | &&、AND                                                      |
| 5                  | NOT                                                          |
| 6                  | BETWEEN、CASE、WHEN、THEN、ELSE                              |
| 7                  | =(比较运算）、<=>、>=、>、<=、<、<>、!=、 IS、LIKE、REGEXP、IN |
| 8                  | \|                                                           |
| 9                  | &                                                            |
| 10                 | <<、>>                                                       |
| 11                 | -(减号）、+                                                  |
| 12                 | *、/、%                                                      |
| 13                 | ^                                                            |
| 14                 | -(负号）、〜（位反转）                                       |
| 15                 | !                                                            |

### 2.5.6 内连接（INNER JOIN）

通过在查询中设置连接条件的方式，来移除查询结果集中某些数据行后的交叉连接。

```mysql
SELECT <列名1，列名2 …>
FROM <表名1> INNER JOIN <表名2> [ ON子句]
```

INNER关键字可省略

### 2.5.7 外连接（LEFT/RIGHT JOIN）

先将连接的表分为基表和参考表，再以基表为依据返回满足和不满足条件的记录。



1. 左外连接

   > 关键字：LEFT OUTER JOIN或者LEFT JOIN
   >
   > * 接收该关键字左表（基表）的所有行，并用这些行与该关键字右表（参考表）中的行进行匹配，即匹配左表中的每一行及右表中符合条件的行。
   > * 左外连接的结果集中，除了匹配的行之外，还包括左表中有但在右表中不匹配的行，对于这样的行，从右表中选择的列的值被设置为NULL，即左外连接的结果集中的NULL值表示右表中没有找到与左表符合的记录。

2. 右外连接

   > 关键字：RIGHT OUTER JOIN或者RIGHT JOIN
   >
   > 除了基表、参考表互换，其余与左外连接相同

### 2.5.8 子查询

1. IN子查询

   ```mysql
   <表达式> [NOT] IN <子查询>
   ```

2. 比较运算符子查询

   ```mysql
   <表达式> {= | < | > | >= | <= | <=> | < > | != }
   { ALL | SOME | ANY} <子查询>
   ```

3. EXIST子查询

   ```mysql
   EXIST <子查询>
   ```



实例：

```mysql
mysql> SELECT name FROM tb_students_info
    -> WHERE dept_id IN
    -> (SELECT dept_id
    -> FROM tb_departments
    -> WHERE dept_type= 'A' );
    
    
mysql> SELECT * FROM tb_students_info
    -> WHERE EXISTS
    -> (SELECT dept_name
    -> FROM tb_departments
    -> WHERE dept_id=7);
```

### 2.5.9 分组查询

```mysql
GROUP BY { <列名> | <表达式> | <位置> } [ASC | DESC]
```

### 2.5.10 HAVING：指定过滤条件

```mysql
HAVING <条件>
```



与WHERE的区别：

* WHERE 子句主要用于过滤数据行，而 HAVING 子句主要用于过滤分组，即 HAVING 子句基于分组的聚合值而不是特定行的值来过滤数据，主要用来过滤分组。
* WHERE 子句不可以包含聚合函数，HAVING 子句中的条件可以包含聚合函数。
* HAVING 子句是在数据分组后进行过滤，WHERE 子句会在数据分组前进行过滤。WHERE 子句排除的行不包含在分组中，可能会影响 HAVING 子句基于这些值过滤掉的分组。

### 2.5.11 正则表达式查询（REGEXP）

| 选项         | 说明                                  | 例子                                        | 匹配值示例                  |
| ------------ | ------------------------------------- | ------------------------------------------- | --------------------------- |
| ^            | 匹配文本的开始字符                    | '^b' 匹配以字母 b 开头 的字符串             | book、big、banana、 bike    |
| $            | 匹配文本的结束字符                    | 'st$’ 匹配以 st 结尾的字 符串               | test、resist、persist       |
| .            | 匹配任何单个字符                      | 'b.t’ 匹配任何 b 和 t 之间有一个字符        | bit、bat、but、bite         |
| *            | 匹配零个或多个在它前面的字 符         | 'f*n’ 匹配字符 n 前面有 任意个字符 f        | fn、fan、faan、abcn         |
| +            | 匹配前面的字符 1 次或多次             | 'ba+’ 匹配以 b 开头，后 面至少紧跟一个 a    | ba、bay、bare、battle       |
| <字符串>     | 匹配包含指定字符的文本                | 'fa’                                        | fan、afa、faad              |
| [字符集合]   | 匹配字符集合中的任何一个字 符         | '[xz]'匹配 x 或者 z                         | dizzy、zebra、x-ray、 extra |
| [^]          | 匹配不在括号中的任何字符              | '[^abc]’ 匹配任何不包 含 a、b 或 c 的字符串 | desk、fox、f8ke             |
| 字符串{n,}   | 匹配前面的字符串至少 n 次             | b{2} 匹配 2 个或更多 的 b                   | bbb、 bbbb、 bbbbbbb        |
| 字符串 {n,m} | 匹配前面的字符串至少 n 次， 至多 m 次 | b{2,4} 匹配最少 2 个， 最多 4 个 b          | bbb、 bbbb                  |

实例：

```mysql
mysql> SELECT * FROM tb_departments
    -> WHERE dept_name REGEXP '^C';
```

### 2.5.12 插入数据

1. INSERT...VALUES

   ```mysql
   INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
   VALUES (值1) [… , (值n) ];
   ```

   

2. INSERT...SET

   ```mysql
   INSERT INTO <表名>
   SET <列名1> = <值1>,
           <列名2> = <值2>,
           …
   ```

   

### 2.5.13 更改数据

```mysql
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ]
[ORDER BY 子句] [LIMIT 子句]
```

### 2.5.14 删除数据

```mysql
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
```

## 2.6 视图操作

### 2.6.1 创建视图

```mysql
CREATE VIEW <视图名> AS <SELECT语句>

//单表
CREATE VIEW view_students_info
AS SELECT * FROM tb_students_info;
//多表
mysql> CREATE VIEW v_students_info
    -> (s_id,s_name,d_id,s_age,s_sex,s_height,s_date)
    -> AS SELECT id,name,dept_id,age,sex,height,login_date
    -> FROM tb_students_info;
```



查询视图：

```mysql
DESCRIBE 视图名；
```

### 2.6.2 修改视图

```mysql
ALTER VIEW <视图名> AS <SELECT语句>
```

### 2.6.3 删除视图

```mysql
DROP VIEW <视图名1> [ , <视图名2> …]
```

## 2.7 存储过程

### 2.7.1 创建存储过程

```mysql
CREATE PROCEDURE <过程名> ( [过程参数[,…] ] ) <过程体>
[过程参数[,…] ] 格式
[ IN | OUT | INOUT ] <参数名> <类型>
```

不带参数：

```mysql
//创建
mysql> DELIMITER //
mysql> CREATE PROCEDURE ShowStuScore()
    -> BEGIN
    -> SELECT * FROM tb_students_score;
    -> END //
//执行    
mysql> DELIMITER ;
mysql> CALL ShowStuScore();
```



带参数：

```mysql
//创建
mysql> DELIMITER //
mysql> CREATE PROCEDURE GetScoreByStu
    -> (IN name VARCHAR(30))
    -> BEGIN
    -> SELECT student_score FROM tb_students_score
    -> WHERE student_name=name;
    -> END //
//执行
mysql> DELIMITER ;
mysql> CALL GetScoreByStu('Green');
```

### 2.7.2 修改存储过程

```mysql
ALTER PROCEDURE <过程名> [ <特征> … ]
```

### 2.7.3 删除存储过程

```mysql
DROP { PROCEDURE | FUNCTION } [ IF EXISTS ] <过程名>
```

## 2.8 触发器

MySQL 数据库中触发器是一个特殊的存储过程，不同的是执行存储过程要使用 CALL 语句来调用，而触发器的执行不需要使用 CALL 语句来调用，也不需要手工启动，只要一个预定义的事件发生就会被 MySQL自动调用。

引发触发器执行的事件一般如下：

- 增加一条学生记录时，会自动检查年龄是否符合范围要求。
- 每当删除一条学生信息时，自动删除其成绩表上的对应记录。
- 每当删除一条数据时，在数据库存档表中保留一个备份副本。


触发程序的优点如下：

- 触发程序的执行是自动的，当对触发程序相关表的数据做出相应的修改后立即执行。
- 触发程序可以通过数据库中相关的表层叠修改另外的表。
- 触发程序可以实施比 FOREIGN KEY 约束、CHECK 约束更为复杂的检查和操作。

1. INSERT触发器
2. UPDATE触发器
3. DELETE触发器

## 2.9 索引

类似哈希表

索引物理分类：

1. B-树索引

   > * 叶子节点：包含的条目直接指向表里的数据行。叶子节点之间彼此相连，一个叶子节点有一个指向下一个叶子节点的指针。
   > * 分支节点：包含的条目指向索引里其他的分支节点或者叶子节点。
   > * 根节点：一个B-树索引只有一个根节点，实际上就是位于树的最顶端的分支节点。

2. 哈希索引

   > MySQL 目前仅有 MEMORY 存储引擎和 HEAP 存储引擎支持这类索引。其中，MEMORY 存储引擎可以支持 B- 树索引和 HASH 索引，且将 HASH 当成默认索引。



索引逻辑分类：

1. 普通索引

   > 加快对数据的访问速度，通常使用关键字INDEX或KEY

2. 唯一性索引

   > 不允许索引列具有相同索引值的索引，避免数据重复

3. 主键索引

   > PRIMARY KEY

4. 空间索引

   > 地理空间数据类型GEOMETRY

5. 全文索引



### 2.9.1 创建索引

```mysql
1.CREATE INDEX语句
CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC])

2.CREATE TABLE或者 ALTER TABLE中直接添加
```



查看索引：

```mysql
SHOW INDEX FROM <表名> [ FROM <数据库名>]
```

### 2.9.2 删除索引

1. 使用DROP INDEX语句

   ```mysql
   DROP INDEX <索引名> ON <表名>
   ```

2. 使用ALTER TABLE语句

   - DROP PRIMARY KEY：表示删除表中的主键。一个表只有一个主键，主键也是一个索引。
   - DROP INDEX index_name：表示删除名称为 index_name 的索引。
   - DROP FOREIGN KEY fk_symbol：表示删除外键。

## 2.10 用户

避免用户恶意冒名使用root账号控制数据库，不同用户不同权限，确保数据的安全访问

### 2.10.1 创建用户

```mysql
CREATE USER <用户名> [ IDENTIFIED ] BY [ PASSWORD ] <口令>
```

 1) <用户名>

指定创建用户账号，格式为 'user_name'@'host_name'。这里`user_name`是用户名，`host_name`为主机名，即用户连接 MySQL 时所在主机的名字。若在创建的过程中，只给出了账户的用户名，而没指定主机名，则主机名默认为“%”，表示一组主机。

 2) PASSWORD

可选项，用于指定散列口令，即若使用明文设置口令，则需忽略`PASSWORD`关键字；若不想以明文设置口令，且知道 PASSWORD() 函数返回给密码的散列值，则可以在口令设置语句中指定此散列值，但需要加上关键字`PASSWORD`。

 3) IDENTIFIED BY子句

用于指定用户账号对应的口令，若该用户账号无口令，则可省略此子句。

 4) <口令>

指定用户账号的口令，在`IDENTIFIED BY`关键字或`PASSWOED`关键字之后。给定的口令值可以是只由字母和数字组成的明文，也可以是通过 PASSWORD() 函数得到的散列值。

使用 CREATE USER 语句应该注意以下几点：

- 如果使用 CREATE USER 语句时没有为用户指定口令，那么 MySQL 允许该用户可以不使用口令登录系统，然而从安全的角度而言，不推荐这种做法。
- 使用 CREATE USER 语句必须拥有 MySQL 中 MySQL 数据库的 INSERT 权限或全局 CREATE USER 权限。
- 使用 CREATE USER 语句创建一个用户账号后，会在系统自身的 MySQL 数据库的 user 表中添加一条新记录。若创建的账户已经存在，则语句执行时会出现错误。
- 新创建的用户拥有的权限很少。他们可以登录 MySQL，只允许进行不需要权限的操作，如使用 SHOW 语句查询所有存储引擎和字符集的列表等。

```mysql
mysql> CREATE USER 'james'@'localhost'
    -> IDENTIFIED BY 'tiger';
```

### 2.10.2 修改用户

修改用户名：

```mysql
RENAME USER <旧用户> TO <新用户>

mysql> RENAME USER james@'localhost'
    -> TO jack@'localhost';
```



修改用户口令：

```mysql
SET PASSWORD [ FOR <用户名> ] =
{
    PASSWORD('新明文口令')
    | OLD_PASSWORD('旧明文口令')
    | '加密口令值'
}

mysql> SET PASSWORD FOR 'jack'@'localhost'=
    -> PASSWORD('lion');
```

### 2.10.3 删除用户

```mysql
DROP USER <用户名1> [ , <用户名2> ]…
```

### 2.10.4 授予用户权限

```mysql
GRANT
<权限类型> [ ( <列名> ) ] [ , <权限类型> [ ( <列名> ) ] ]
ON <对象> <权限级别> TO <用户>
其中<用户>的格式：
<用户名> [ IDENTIFIED ] BY [ PASSWORD ] <口令>
[ WITH GRANT OPTION]
| MAX_QUERIES_PER_HOUR <次数>
| MAX_UPDATES_PER_HOUR <次数>
| MAX_CONNECTIONS_PER_HOUR <次数>
| MAX_USER_CONNECTIONS <次数>
```

### 2.10.5 删除用户权限

```mysql
//回收某些特定的权限
REVOKE <权限类型> [ ( <列名> ) ] [ , <权限类型> [ ( <列名> ) ] ]…
ON <对象类型> <权限名> FROM <用户1> [ , <用户2> ]…

//回收特定用户的所有权限
REVOKE ALL PRIVILEGES, GRANT OPTION
FROM user <用户1> [ , <用户2> ]…
```

## 2.11 事务

特性：

1. 原子性(Atomicity)

   > 要么全部执行，要么全都不执行

2. 一致性(Consistency)

3. 隔离性(Isolation)

   > 多个事务同时运行，互斥不干扰

4. 持久性(Durability)

   > 对数据的变动永久有效

### 2.11.1 开始事务

```mysql
BEGIN TRANSACTION <事务名称> |@<事务变量名称>
```

### 2.11.2 提交事务

```mysql
COMMIT TRANSACTION <事务名称> |@<事务变量名称>
```

### 2.11.3 撤销事务

```mysql
ROLLBACK [TRANSACTION]
[<事务名称>| @<事务变量名称> | <存储点名称>| @ <含有存储点名称的变量名>
```

## 2.12 备份(INTO OUTFILE)

SELECT INTO OUTFILE语句把表数据导出到一个文本文件中进行备份

```mysql
mysql> SELECT * FROM test_db.tb_students_info
    -> INTO OUTFILE 'C:/ProgramData/MySQL/MySQL Server 5.7/Uploads/file.txt'
    -> FIELDS TERMINATED BY '"'
    -> LINES TERMINATED BY '?';
```

## 2.13 数据库恢复(LOAD DATA)

以备份为基础，与备份相对应的系统维护和管理操作



数据库恢复机制关键问题：

1. 如何建立冗余数据
2. 如何利用这些冗余数据实施数据库恢复



建立冗余数据技术：

1. 数据转储

   > DBA定期将整个数据库复制到磁带或另一个磁盘上保存起来的过程（后备副本、后援副本）

2. 登录日志文件



可使用 LOAD DATA…INFILE 语句来恢复先前备份的数据。

```mysql
mysql> LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 5.7/
Uploads/file.txt'
    -> INTO TABLE test_db.tb_students_copy
    -> FIELDS TERMINATED BY ','
    -> OPTIONALLY ENCLOSED BY '"'
    -> LINES TERMINATED BY '?';
```

