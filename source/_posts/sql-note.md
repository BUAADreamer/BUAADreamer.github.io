---
title: SQL学习笔记
date: 2021-01-29 23:35:52
tags: 数据库

---

最近结合廖雪峰教程学习了和sql,mysql相关的一些知识，对重要的部分做了一些梳理。
<!--more-->

## 英文教程

##### [How to Load the Sample Database into MySQL Server](https://www.mysqltutorial.org/how-to-load-sample-database-into-mysql-database-server.aspx)

##### https://www.mysqltutorial.org/

## SQL教程（廖雪峰）笔记

### 关系模型

关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的**二维表**，可以把它们看作很多**Excel表**。

### 查询数据

#### 脚本生成数据方式

1.打开MYSQL-command line 

2.输入source 将相应的sql文件对应的地址输入（或者直接把文件拖入命令行）

廖老师的sql文件：[init-test-data.sql](F:\学习\工程与应用科学\CS技术学习\SQL-learn\init-test-data.sql)

3.`SHOW DATABASE`可以查看已有数据库

#### 基本查询

使用SELECT查询的基本语句`SELECT * FROM <表名>`可以查询一个表的所有行和所有列的数据。

#### 条件查询

```mysql
SELECT * FROM <表名> WHERE <条件表达式>
SELECT * FROM students WHERE score >= 80;
```

| 条件                 | 表达式举例1       | 表达式举例2      | 说明                                              |
| :------------------- | :---------------- | :--------------- | :------------------------------------------------ |
| 使用=判断相等        | score = 80        | name = 'abc'     | 字符串需要用单引号括起来                          |
| 使用>判断大于        | score > 80        | name > 'abc'     | 字符串比较根据ASCII码，中文字符比较根据数据库设置 |
| 使用>=判断大于或相等 | score >= 80       | name >= 'abc'    |                                                   |
| 使用<判断小于        | score < 80        | name <= 'abc'    |                                                   |
| 使用<=判断小于或相等 | score <= 80       | name <= 'abc'    |                                                   |
| 使用<>判断不相等     | score <> 80       | name <> 'abc'    |                                                   |
| 使用LIKE判断相似     | name LIKE 'ab%'   | name LIKE '%bc%' | %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |
| BETWEEN a AND b      | BETWEEN 60 AND 90 |                  |                                                   |
| IN (a,b)             | 是否等于a或b      |                  |                                                   |

#### 投影查询

基本格式：`SELECT 列1, 列2, 列3 FROM students`

使用`SELECT 列1, 列2, 列3 FROM ...`时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是`SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`。

例如，以下`SELECT`语句将列名`score`重命名为`points`，而`id`和`name`列名保持不变：

`SELECT id, score points, name FROM students;`

#### 排序

查询结果集通常是按照`id`排序的，也就是根据主键排序。要根据其他条件排序——加上ORDER BY子句

**默认的排序规则**是`ASC`：“**升序**”，即从小到大。ASC可以省略，即`ORDER BY score ASC和ORDER BY score`效果一样。

加上`DESC`表示“**倒序**”

如果score列有相同的数据，要进一步排序，可以继续添加列名。例如，使用`ORDER BY score DESC, gender`表示先按score列倒序，如果有相同分数的，再按gender列排序：

如果有`WHERE`子句，那么`ORDER BY`子句要放到WHERE子句后面。

#### 分页查询

使用`LIMIT <M> OFFSET <N>`可以对结果集进行分页，每次查询返回结果集的一部分；

offset—从第几条记录开始查

limit—最多显示多少条数据

分页查询需要先确定每页的数量和当前页数，然后确定LIMIT和OFFSET的值。

```mysql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```

#### 聚合查询

对于统计总数、平均数这类计算，SQL提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。

`SELECT COUNT(*) FROM students;`

`COUNT(*)`表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是`COUNT(*)`。通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果：

`SELECT COUNT(*) num FROM students;`

聚合查询同样可以使用WHERE条件

除了COUNT()函数外，SQL还提供了如下聚合函数：

| 函数 | 说明                                   |
| :--- | :------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

要特别注意：如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`：

每页3条记录，如何通过聚合查询获得总页数？——`SELECT CEILING(COUNT(*) / 3) FROM students;`

##### 分组

按class_id分组: `SELECT COUNT(*) num FROM students GROUP BY class_id;`

`SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;`

请使用一条SELECT查询查出每个班级男生和女生的平均分：`SELECT class_id,gender,AVG(score) FROM students GROUP BY class_id,gender;`

#### 多表查询

`SELECT * FROM <表1> <表2>`

注意，多表查询时，要使用`表名.列名`这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。但是，用`表名.列名`这种方式列举两个表的所有列实在是很麻烦，所以SQL还允许给表设置一个别名，让我们在投影查询中引用起来稍微简洁一点：

`SELECT s.id sid, s.name, s.gender, s.score, c.id cid, c.name cname FROM students s, classes c;`

注意到`FROM`子句给表设置别名的语法是`FROM <表名1> <别名1>, <表名2> <别名2>`。这样我们用别名`s`和`c`分别表示`students`表和`classes`表。

使用多表查询可以获取M x N行记录；

多表查询的结果集可能非常巨大，要小心使用。

#### 连接查询

注意**INNER JOIN**查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

有**RIGHT OUTER JOIN，就有LEFT OUTER JOIN，以及FULL OUTER JOIN**。它们的区别是：

- INNER JOIN只返回同时存在于两张表的行数据，由于students表的class_id包含1，2，3，classes表的id包含1，2，3，4，所以，INNER JOIN根据条件s.class_id = c.id返回的结果集仅包含1，2，3。

- RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以NULL填充剩下的字段。

- LEFT OUTER JOIN则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的class_name是NULL：

#### 查询数据方式小结

代码形式总结知识点

```MYSQL
SELECT col1_name,col2_name,(COUNT(*),(AVG(col3_name)) average) #基本查询方式 聚合查询——聚合函数 投影查询
FROM excel1_name replace1_name (,excel2_name replace2_name) #多表查询 同表名字替换
INNER(/FULL OUTER/RIGHT OUTER/LEFT OUTER)  JOIN classes c #连接查询
(GROUP BY colx_name) #聚合查询——分组查询
WHERE (EXPRESSION) #条件查询
ORDER BY coly_name (ASC/DESC) #排序
LIMIT m OFFSET n; #分页查询
```

### 修改数据

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete。

而对于增、删、改，对应的SQL语句分别是：

- INSERT：插入新记录；
- UPDATE：更新已有记录；
- DELETE：删除已有记录。

#### INSERT

`INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);`

注意到我们并没有列出`id`字段，也没有列出`id`字段对应的值，这是因为`id`字段是一个自增主键，它的值可以由数据库自己推算出来。此外，如果一个字段有默认值，那么在`INSERT`语句中也可以不出现。

还可以一次性添加多条记录，只需要在`VALUES`子句中指定多个记录值，每个记录是由`(...)`包含的一组值

```mysql
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
SELECT * FROM students;
```

#### UPDATE

`UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;`

可以一次更新多条记录

#### DELETE

`DELETE FROM <表名> WHERE ...;`

不带`WHERE`条件的`DELETE`语句会删除整个表的数据

### MySQL

查找MYSQL安装目录方法：进入mysql命令行输入：`show variables like "%char%";`

我的：C:\Program Files\MySQL\MySQL Server 8.0\bin

**加环境变量**：我的计算机——属性——高级系统设置——环境变量——在用户变量和系统变量的path处加地址

打开命令提示符，输入命令`mysql -u root -p`，提示输入口令。填入MySQL的`root`口令，如果正确，就连上了MySQL Server，同时提示符变为`mysql>`：

输入`exit`断开与MySQL Server的连接并返回到命令提示符。

MySQL Client的可执行程序是mysql，MySQL Server的可执行程序是mysqld。

MySQL Client和MySQL Server的关系如下：

```ascii
┌──────────────┐  SQL   ┌──────────────┐
│ MySQL Client │───────>│ MySQL Server │
└──────────────┘  TCP   └──────────────┘
```

在MySQL Client中输入的SQL语句通过TCP连接发送到MySQL Server。默认端口号是3306，即如果发送到本机MySQL Server，地址就是`127.0.0.1:3306`。

也可以只安装MySQL Client，然后连接到远程MySQL Server。假设远程MySQL Server的IP地址是`10.0.1.99`，那么就使用`-h`指定IP或域名：

```cmd
mysql -h 10.0.1.99 -u root -p
```

##### 小结

命令行程序`mysql`实际上是MySQL客户端，真正的MySQL服务器程序是`mysqld`，在后台运行。

#### 管理MySQL

要管理MySQL，可以使用可视化图形界面MySQL Workbench。

MySQL Workbench可以用可视化的方式查询、创建和修改数据库表，但是，归根到底，MySQL Workbench是一个图形客户端，它对MySQL的操作仍然是发送SQL语句并执行。因此，本质上，MySQL Workbench和MySQL Client命令行都是客户端，和MySQL交互，唯一的接口就是SQL。

因此，MySQL提供了大量的SQL语句用于管理。虽然可以使用MySQL Workbench图形界面来直接管理MySQL，但是，很多时候，通过SSH远程连接时，只能使用SQL命令，所以，了解并掌握常用的SQL管理操作是必须的。

##### 1.数据库

在一个运行MySQL的服务器上，实际上可以创建多个数据库（Database）。要列出所有数据库，使用命令：`	SHOW DATABASES`

其中，`information_schema`、`mysql`、`performance_schema`和`sys`是系统库，不要去改动它们。其他的是用户创建的数据库。

要**创建**一个新数据库，使用命令：

```MYSQL
CREATE DATABASE test;
```


要**删除**一个数据库，使用命令：

```MYSQL
DROP DATABASE test;
```


注意：删除一个数据库将导致该数据库的所有表全部被删除。

对一个数据库进行操作时，要首先将其切换为当前数据库：`USE test;`

##### 2.表

列出当前数据库的所有表，使用命令：

```MYSQL
SHOW TABLES;
```

要查看一个表的结构，使用命令：

```MYSQL
DESC students;
```

还可以使用以下命令查看创建表的SQL语句：

```MYSQL
SHOW CREATE TABLE students;
```

创建表使用`CREATE TABLE`语句，而删除表使用`DROP TABLE`语句：

修改表就比较复杂。如果要给`students`表新增一列`birth`，使用：

```
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```

要修改`birth`列，例如把列名改为`birthday`，类型改为`VARCHAR(20)`：

```
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```

要删除列，使用：

```
ALTER TABLE students DROP COLUMN birthday;
```

##### 3.退出MySQL

使用`EXIT`命令退出MySQL

#### 实用SQL语句

在编写SQL时，灵活运用一些技巧，可以大大简化程序逻辑。

##### 1.插入或替换

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用`REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：

```
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前`id=1`的记录将被删除，然后再插入新记录。

##### 2.插入或更新

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。

##### 3.插入或忽略

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用`INSERT IGNORE INTO ...`语句：

```
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。

##### 4.快照

如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合`CREATE TABLE`和`SELECT`：

```
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

新创建的表结构和`SELECT`使用的表结构完全一致。

##### 5.写入查询结果集

如果查询结果集需要写入到表中，可以结合`INSERT`和`SELECT`，将`SELECT`语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表`statistics`，记录各班的平均成绩：

```
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
```

然后，我们就可以用一条语句写入各班的平均成绩：

```
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

确保`INSERT`语句的列和`SELECT`语句的列能一一对应，就可以在`statistics`表中直接保存查询的结果：

```
> SELECT * FROM statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
```

##### 6.强制使用指定索引

在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用`FORCE INDEX`强制查询使用指定的索引。例如：

```
> SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
```

指定索引的前提是索引`idx_class_id`必须存在。

## Python3 MySQL 数据库连接 - PyMySQL 驱动

`$ pip3 install PyMySQL   `



