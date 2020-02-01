
函数在SQL数据库中使用十分广泛，下面分别介绍了一些比较常用的函数用法，具体请看下表。


|函数名称|作用|
|:-:|:-:|
|ABS(x)|返会的绝对值|
|SQRT(x)|返会x的非负2次方根|
|MOD(x,y)|返会x被y除后的余数|
|CEILING(x)|返会不小于x的最小整数|
|FLOOR(x)|返会不大于x的最大整数|
|ROUND(x,y)|对x进行四舍五入，小数点后保留y位|
|TRUNCATE(x,y)|舍去x中的小数点y位后面的数|
|SIGN(x)|返会x的符号（正负1、0）|

|函数名称|作用|
|:-:|:-:|
|LENGTH（str）|返会字符串的长度|
|CONCAT（S`1`，...，S `n`）|返会一个或者多个字符串连接产生的新字符串|
|trim（str）|删除字符串两则的空格|
|replace（str，S`1`，S `2`）|使用字符串S2替代字符串str中的所有字符串S1|
|substring（str，n，len）|返会字符串str的子串，起始位置为n，长度为len|
|reverse（str）|返会字符串反转后的结果|
|lacate（S1，str）|返会字符串S1在字符串str中的起始位置|


比较懒...就不做表格了，直接上图

![函数功能（续）](https://upload-images.jianshu.io/upload_images/17476267-c3a0f9afe307da64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---

举几个实际中使用函数的语句
- （1）
```sql
SELECT CONCAT( id,'_', name,'_', grade,'_', gender) 
FROM student;
```
- （2）
```sql
SELECT if(total_price % 2 = 1,total_price, 0)
 FROM tb_order;
```
- （3）
```sql
SELECT CONCAT( id,'_', name,'_', grade,'_', gender) 
FROM student;
```
---
##### 关于表和字段的别名

- 为表取别名
```sql
SELECT * FROM 表 名 [AS] 别名;
```
- 为字段取别名
```sql
SELECT 字段 名 [AS] 别名[, 字段 名 [AS] 别名,…] 
FROM 表 名;
```
---
##### 多表操作（SQL中的重点操作）
(1) 外键
【概述】：外 键 是指 引用 另一个 表中 的 一列 或 多列， 被 引 用的 列 应该 具 有主 键 约束 或 唯一 性 约束。

【注意】：引入外键后，外键列的值只能是参照列的值，并且参照列中的值不能被删除。

- 为表添加外键约束
```sql
alter table 表 名 add constraint FK_ ID foreign 
key( 外 键 字段 名) REFERENCES 外表 表 名( 主 键 字段 名);
 ```
【补充】**show create table 表名**：可执行此操作来查看约束效果。

【例】
```sql
 create table 表名(
    字段描述，
   constraint fk_name foreign
key (外键字段名) references 外键表表名（主键字段）
)
constraint fk_name foreign key (cat_id)
 references tb_category（id）
```
【注意点】：
-  1、建立外键的表必须是**InnoDB型**。

- 2、定义外键名时**不能加引号**。

*添加外键约束参数*
```
 alter table 表 名 add constraint FK_ ID foreign key(外键字段名) 
REFERENCES 外表表名( 主键字段名); 
[ON DELETE {CASCADE | SET NULL | NO ACTION || RESTRICT}]
[ON UPDATE {CASCADE | SET NULL | NO ACTION | RESTRICT}]
```

---

- 关于几个**外键约束参数**的功能汇总表

|参数名称（小写）|作用描述|
|:-:|:-:|
|cascade|删除包含与已删除键值有参照关系的所有记录|
|set null|修改包含与已删除键值有参照关系的所有记录，并使用NULL值替换（不用于已标记的Not NULL字段）|
|no action|不进行任何操作|
|restrict|拒绝主表删除或者修改外检关联列。（在不定义ON DALETE 和ON UPDATE 的子句时，这是默认设置，也是比较安全的设置）|

- 删除外键约束
```sql
alter table 表名 drop foreign key 外键名;
```
---
##### 关于 - 操作关联表

主要分为三种情况：
-  1、多对一（员工与部门）外键；

- 2、多对多（课程与学生）关系表；

- 3、一对一（证件号码与个体）一般情况；

---

##### 连接查询
主要分为交叉连接、内连接以及外连接三种情况。
- 交叉连接
*【笛卡儿积】：两个表行数的乘积。*
**交叉连接返会的就是笛卡尔积。**
```SQL
 SELECT * FROM department(部门) CROSS JOIN employee(员工);
```
- 内连接
【解释】：在内连接查询 中， 只有满足 条件的记录才能出现在查询结果中。
```SQL
SELECT 查询 字段 FROM 表 1 
[INNER] JOIN 表 2 ON 表 1\. 关系字段 = 表 2\. 关系字段
```
- 外连接
【解释】： 返回查询结果中不仅包含符合条件的数据，而且还包括**左表**（ 左连接或左外连接）、**右表**（右连接或右外连接）或两个表（全外连接）中的所有数据。
```sql
SELECT 所 查字 段 FROM 表 1 LEFT| RIGHT [OUTER] JOIN 表 2 
ON 表 1\. 关系 字段= 表 2\. 关系 字段 WHERE 条件
  ```
关于左连接与右连接的区别
- LEFT JOIN(左连接)：返会包括左表中的所有记录和右表中符合连接的条件的记录；
- RIGHT JOIN(右连接)：返会包括右表中的所有记录和左表中符合连接的条件的记录；（没有数据的部分，**使用NULL来补充**）
---
##### 子查询
【解释】：指 一个查询语句嵌套在另 一个查询语句内部的查询。
- 带 IN 关键字的子查询
```SQL
SELECT * FROM department WHERE did 
 IN( SELECT did FROM employee WHERE age= 20);
```
- 带 ANY 关键字的子查询
```SQL
SELECT * FROM department 
WHERE did ＞ any( select did from employee);
```
- 带 ALL 关键字的子查询
```SQL
SELECT * FROM department 
WHERE did ＞ all( select did from employee);
```
-  带 比较运算符 的子查询
```SQL
SELECT * FROM department 
WHERE did=( select did from employee where name=' GGG');
```













