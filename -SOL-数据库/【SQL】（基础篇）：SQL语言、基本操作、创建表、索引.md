
##### 【介绍】数据库与数据库管理系统
 
- 数据库：
【概述】：数据库提供 了一个存储空间用来存储各种数据，可以将数据库视为一个 **存储数据**的容器。

+ 数据库管理系统：

专门用于**创建和管理**数据库的一套 软件， 介于应用程序和操作系统之间，如 **MySQL、 Oracle、 SQL Server、 DB2**等。结构图如下所示。


![数据库系统架构图](https://upload-images.jianshu.io/upload_images/17476267-3ec106e60c0d94a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---

---
##### 关于SQL语言
 SQL<sup>（ Structured Query Language） </sup>是 一种数据库查询语言和程序设计语言，主要用于管理数据库中的数据，如存取数据、查询 数据、更新 数据等。
由以下四种语言构成：

- （1） 数据定义语言<sup>（ Data Definition Language， DDL）</sup>

【用于】数据库定义语言主要用于定义数据库、表等。

【例如】**CREATE、ALTER、DROP**

- （2） 数据操作语言<sup>（ Data Manipulation Language， DML）</sup>

【用于】数据操作语言 主要用于对数据库进行添加、修改和删除操作。

【例如】**INSERT、UPDATE、DELETE**

- （3）数据查询语言<sup>（ Data Query Language， DQL）</sup>

【用于】数据查询语言主要用于查询数据；

【例如】**SELECT**

- （4） 数据控制语言<sup>（ Data Control Language， DCL）</sup>

【用于】数据控制语言主要用于控制用的访问权限。

【例如】**GRANT、COMMIT、ROLLBACK**

---
【注释一下】SQL 对大小写不敏感！

【编程-好习惯】**分号是在数据库系统中分隔每条 SQL 语句的标准方法**，这样就可以在对服务器的相同请求中执行一条以上的语句。

如果使用的是 MS Access 和 SQL Server 2000，则不必在每条 SQL 语句之后使用分号，不过某些数据库软件要求必须使用分号。（最好还是写上）

---

- mysql -uroot -p ：连接到mysql

- \s  ：查看数据库信息

- set character_ set_ client = gbk：
设置数据库编码集（这种方式只能对当前窗口有效）；【永久修改】：在`my.ini`中配置。

---

#####数据库和表的基本操作

(1).创建数据库

- 【格式】：create database 数据库名称
- 【格式】：create database user;

(2).查看数据库

- 【格式】：show databases;

(3).查看某个数据库的创建信息

 - 【格式】：show create database 数据库名称
- 【格式】：show create database users;

(4).修改数据库

- 【格式】：`alter database 数据库名 default character set 编码方式 collate 编码方式_bin`

(5).删除数据库
- 【格式】：`drop database 数据库名称`

---

SQL 可以分为两个部分：数据操作语言<sup> (DML)</sup> 和 数据定义语言 <sup> (DDL)</sup> 。

 (结构化查询语言<sup> (SQL)</sup>是用于执行查询的语法。但是 SQL 语言也包含用于更新、插入和删除记录的语法。

查询和更新指令构成了 SQL 的 DML 部分：
``` sql
    SELECT - 从数据库表中获取数据
    UPDATE - 更新数据库表中的数据
    DELETE - 从数据库表中删除数据
    INSERT INTO - 向数据库表中插入数据
```
SQL 的数据定义语言 (DDL) 部分使我们有能力创建或删除表格。我们也可以定义索引（键），规定表之间的链接，以及施加表间的约束。

SQL 中最重要的 DDL 语句:
``` sql
    CREATE DATABASE - 创建新数据库
    ALTER DATABASE - 修改数据库
    CREATE TABLE - 创建新表
    ALTER TABLE - 变更（改变）数据库表
    DROP TABLE - 删除表
    CREATE INDEX - 创建索引（搜索键）
    DROP INDEX - 删除索引
```
实例：
``` sql
MariaDB [db_student_msg]> show tables;
+--------------------------+
| Tables_in_db_student_msg |
+--------------------------+
| tb_class                 |
| tb_score                 |
| tb_student               |
| tb_subject               |
+--------------------------+
4 rows in set (0.00 sec)

MariaDB [db_student_msg]> desc tb_student;
+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| stu_no    | char(8)     | NO   | PRI | NULL    |       |
| stu_name  | varchar(30) | NO   |     | NULL    |       |
| gender    | tinyint(1)  | NO   |     | NULL    |       |
| brith_day | datetime    | YES  |     | NULL    |       |
| class_no  | int(11)     | NO   | MUL | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
5 rows in set (0.02 sec)
MariaDB [db_student_msg]> desc tb_score;
+------------+---------+------+-----+---------+-------+
| Field      | Type    | Null | Key | Default | Extra |
+------------+---------+------+-----+---------+-------+
| stu_no     | char(8) | NO   | PRI | NULL    |       |
| subject_no | int(11) | NO   | PRI | NULL    |       |
| score      | double  | YES  |     | 0       |       |
+------------+---------+------+-----+---------+-------+
```
---

##### 数据类型

mysql 提供了多种数据类型，包括：**整数、浮点数、定点数、日期和时间、字符串和二进制**类型。（这里就不详细阐述了...）
|数据类型|存储范围|数据类型|存储范围|
|:--:|:--:|:--:|:--:|
|TINYNLOB|1~255字节|MEDIUMBLOB|0~16777215字节|
|BLOB|0~65535字节|LONGBLOB|0~4294967295字节|

---
#####创建数据表

```
 【格式】：CREATE TABLE 表 名 (
字段 名 1   数据 类型[ 完整性 约束 条件],
字段 名 2   数据 类型[ 完整性 约束 条件],
...
字段 名 n, 数据 类型[ 完整性 约束 条件]
)
```

- 查看数据表：show create table 表名；

- 查看表的字段信息：DESC 表名；

###### 修改数据表
- 1） 修改表名

【格式】：alter table 旧表名 rename [to] 新表名
- 2）修改字段名

【格式】：alter table 表名 change  旧字段名  新字段名  新字段类型
```
 ALTER TABLE grade CHANGE name username VARCHAR( 20);
```
 - （3）修改字段的数据类型

【格式】：alter table 表名 modify 字段名 数据类型；

- （4）添加字段

【格式】：alter  table 表名 add 新字段 数据类型[约束条件] [first|after 已存在的字段名]

- （5） 删除字段

【格式】：alter table 表名 drop  字段名

- （6）修改字段的排列位置

【格式】：alter  table  表名 modify 字段名1 数据类型 first | after   字段2

 - （7）删除数据表

【格式】：drop table 表名

---
##### 关于表的约束
为了防止向数据库中插入错误的数据，在mysql中定义了一些约束规则。
![表的约束](https://upload-images.jianshu.io/upload_images/17476267-6e09ad41f037256d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

(1).【主键】：通过primary key 来定义，唯一且非空；
- 单字段主键：
    -  【格式】：字段名 数据类型 primary key；
  `CREATE TABLE example01( id INT PRIMARY KEY, name VARCHAR( 20), grade FLOAT);`
- 多字段主键
由多个字段组合而成的**主键**。
  - 【格式】：PRIMARY KEY (字段 名 1,…, 字段 名 n)


(2).【 非空约束】

- 字段值不能为NULL；
- 【格式】：字段 名 数据 类型 NOT NULL；

(3).【唯一约束】

- 【作用】：保证字段的唯一性；
- 【格式】：字段 名 数据 类型 UNIQUE;

(4).【默认约束】

- 【提示】：插入数据时，如果字段没有被赋值，则给该字段一个默认值；
- 【格式】：字段 名 数据 类型 DEFAULT 默认值；

---
###### 关于表中字段的自动增加

- 【目的】：为了给新插入的字段一个唯一的ID
- 【格式】：字段名 数据类型 AUTO_ INCREMENT;
``` sql
例如：
CREATE TABLE example05( id INT PRIMARY KEY AUTO_ INCREMENT, stu_ id INT UNIQUE, grade FLOAT );
```

---
##### 索引
【解释】：索引就好像一本书的目录，用于快速检索数据。
索引分类：普通索引、唯一索引、全文索引。

- 普通索引
【解释】：由KEY 或 INDEX 定义。是基本的索引类型，可以用于任何数据类型 。其值是否唯一或为空由字段本身约束决定
- 唯一索引
【解释】：由Unique 定义的索引，该索引的字段值必须唯一。

- 全文索引
【解释】：只能创建在char、varchar、text字段类型上。只有MyISAM引擎支持。

【提高】：索引也可创建在多列上。

---
##### 创建索引
在创建表的时候，如果创建索引，结构如下：
``` sql
 CREATE TABLE 表 名(

字段名 数据类型[ 完整性 约束 条件],

字段名 数据类型[ 完整性 约束 条件], …

字段名 数据类型 [UNIQUE| FULLTEXT| SPATIAL] INDEX| KEY [别名] (字段 名 1 [(长度)]) [ASC|DESC]));

```
【解释一下】
  -  【别名】：索引的名称；
  -  【字段名1】：指定索引字段名称；
  -  【长度】：表示索引的长度；
  -  【ASC|DESC】： 索引的顺序；

###### 分类探讨创建索引
-  1）创建普通索引

【格式】：CREATE TABLE t1( id INT, name VARCHAR( 20), score FLOAT,name VARCHAR( 20), score FLOAT, INDEX (id) );

- 2）创建唯一索引

【格式】：CREATE TABLE t2( id INT NOT NULL, name VARCHAR( 20) NOT NULL, score FLOAT, UNIQUE INDEX unique_ id( id ASC) );

- 3）创建多列索引

【格式】：CREATE TABLE t5( id INT NOT NULL, name VARCHAR( 20) NOT NULL,score FLOAT, INDEX multi( id, name( 20)) );

---
###### 删除索引

- 直接操作

【结构】：ALTER TABLE 表名 DROP INDEX 索引；
- 使用 DROP INDEX 删除 索引

【结构】：DROP INDEX 索引 名 ON 表 名;

---
#####【重点】添加、更新、删除数据

###### 添加数据
  -  指定字段添加数据

【结构】：INSERT INTO 表 名( 字段 名 1, 字段 名 2,…) VALUES( 值 1, 值 2,…);

   - 不指定字段名的insert

【结构】：INSERT INTO 表 名 VALUES( 值 1, 值 2,…);

- 同时插入多条数据

【结构】：INSERT INTO 表 名[( 字段 名 1, 字段 名 2,…) ] VALUES( 值 1, 值 2,…),( 值 1, 值 2,…), … (值 1, 值 2,…);

###### 更新数据

【结构】：UPDATE 表 名 SET 字段 名 1= 值 1[, 字段 名 2 = 值 2,…] [WHERE 条件 表达式]

###### 删除数据

- 【结构】：DELETE FROM 表 名 [WHERE 条件 表达式]；

- 【结构】：TRUNCATE [TABLE] 表 名；



































