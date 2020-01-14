【插播】：[【SQL】(基础篇):](https://www.jianshu.com/p/b0d275039f72)https://www.jianshu.com/p/b0d275039f72
#### 关于单表查询
单表查询分为简单查询、条件查询以及高级查询三种，我将在下文中分别介绍一下。

---
##### 简单查询
简单查询主要介绍以下三种。

###### （1）SELECT 语句的使用
``` sql
SELECT
     [DISTINCT] *|{字段 名 1, 字段 名 2, 字段 名 3,…}
FROM 表 名
[WHERE 条件 表达式 1]
[GROUP BY 字段 名 [HAVING 条件 表达式 2]]
[ORDER BY 字段 名 [ASC| DESC]][LIMIT [OFFSET] 记录 数]
```
###### （2）查询所有字段

1、【结构】：
```SQL
SELECT 字段 名 1,… 
FROM 表 名;
```
2、【结构】：
```SQL
SELECT * 
FROM 表 名;
```
###### （3）查询指定字段

【结构】：
```SQL
SELECT 字段 名 1, 字段 名 2,… 
FROM 表 名;
```
---
![以下实例均基于此表数据](https://upload-images.jianshu.io/upload_images/17476267-4bd030846a006125.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
---

##### 按条件查询
条件查询主要分为以下八种。
```SQL
SELECT 字段 名 1, 字段 名 2,… 
FROM 表名 WHERE 条件 表达式
```
【实例】
``` sql
SELECT stu_name FROM tb_student WHERE stu_no='20162154';
SELECT * FROM tb_student WHERE stu_no='20162154';
SELECT * FROM tb_student WHERE gender = 0;
 SELECT * FROM tb_student WHERE gender = 0 And class_no=2;
 SELECT * FROM tb_student WHERE class_no=1 or (class_no = 2 AND gender = 0);
SELECT * FROM tb_student WHERE class_no=1 and stu_name !='李白';
SELECT * FROM tb_student WHERE class_no=1 and stu_name <>'李白';

SELECT * FROM tb_student WHERE stu_name = "李白"or stu_name = "杜甫";
```
---

###### （1）带关系运算符的的查询

  - 【结构】：
```SQL
select 字段1，字段2，....，字段n 
from 表名 where 条件表达式
```
  - 【结构】：
```SQL
SELECT id, name FROM student WHERE id= 4;
```
######  （2）带IN的关键字

  - 【结构】：
```SQL
SELECT *|字段 名 1, 字段 名 2,… 
FROM 表 名 
WHERE 字段 名 [NOT] IN (元素 1, 元素 2,…)
```
  - 【结构】：
```SQL
SELECT id, grade, name, gender 
FROM student 
WHERE id NOT IN( 1, 2, 3);
```
``` sql
SELECT
	*
FROM
	tb_student
WHERE
	stu_name IN (
		'李白',
		'杜甫',
		'孙悟空'
	);
SELECT
	*
FROM
	tb_student
WHERE
	stu_name NOT IN (
		'李白',
		'杜甫',
		'孙悟空'
	);
```
![结果展示](https://upload-images.jianshu.io/upload_images/17476267-e9b23a28df9e22b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
######  （3）带between...and 的关键字

  - 【结构】：
```SQL
SELECT *|{字段 名 1, 字段 名 2,…} 
FROM 表 名 WHERE 字段 名 [NOT] 
BETWEEN 值 1 AND 值 2
```
  - 【结构】：
```SQL
SELECT id, name 
FROM student WHERE id NOT 
BETWEEN 2 AND 5;
```
``` sql
SELECT
	*
FROM
	tb_student
WHERE
	stu_no BETWEEN "20162154"
AND "20162156";
```

【结果展示】
![带between...and](https://upload-images.jianshu.io/upload_images/17476267-485e013f7a8f4845.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

######  （4）空值查询
 在SQL中 NULL 与任何值（包括自己）做相等比较时，都是** false**。
【结构】：
```SQL
SELECT *|字段 名 1, 字段 名 2,… 
FROM 表 名 WHERE 字段 名 IS [NOT] NULL
```
```sql
UPDATE tb_student
SET birth_day = NULL
WHERE
	stu_name = "武则天";
-- 注释 SQL NULL 与任何值，包括自己
-- 做相等比较时，都是 false;
SELECT
	*
FROM
	tb_student
WHERE
	birth_day IS NULL;
```
---

######  （5）带distinct关键字的查询
【介绍】：DISTINCT 去重;
【结构】：
```SQL
SELECT DISTINCT 字段名 
FROM 表 名;
```
【结构】：
```SQL
SELECT DISTINCT 字段名1, 字段名2,… 
FROM 表 名;
```
```sql
-- DISTINCT 去重;
SELECT DISTINCT
	stu_name
FROM
	tb_student;
```
---

######  （6）带LIKE 关键字的查询
 【介绍】 like: 模糊查询，%：代表0个或多个字符；
【结构】：
```SQL
SELECT *|{字段 名 1, 字段 名 2,…}
 FROM 表 名 WHERE 字段名 
[NOT] LIKE '匹配 字符串';
```
【代码】：
```sql
-- like: 模糊查询，%：代表0个或多个字符；
SELECT * FROM
	tb_student
WHERE
	stu_name NOT LIKE '李%';
SELECT * FROM
	tb_student
WHERE
	stu_name LIKE '上_婉_';
```

【结果展示】
![结果展示](https://upload-images.jianshu.io/upload_images/17476267-4af189c74cfc48e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【NOT 关键字可选】
通配符有：

- 1、 % 用于匹配多个字符

- 2、_  用于匹配单个字符

- 3、要匹配 `% `或 _ 用 `"\" `来进行转义，**如果要更改默认的转义字符，可以使用 `escape 关键字`指定**。
例如：
``` SQL
 where product_name
 like '{_三' escape '{';
```

---

######  （7）带AND 关键字的多条件查询

【结构】：
```SQL
SELECT *|{字段 名 1, 字段 名 2,…} 
FROM 表 名 WHERE 条件 表达式 1 AND 条件 表达式 2 [… AND 条件 表达式 n];
```
```SQL
SELECT * FROM
	tb_student
WHERE
	class_no = 2
AND gender = 0;
```
```SQL
SELECT
	COUNT(*) AS 'p_num'
FROM
	tb_student t
WHERE
	t.class_no = 2
AND t.gender = 0;
```
######  （8）带or关键字的条件查询

【结构】：
```SQL
SELECT *|{字段 名 1, 字段 名 2,…} 
FROM 表 名 
WHERE 条件 表达式 1 
OR 条件 表达式 2 [… 
OR 条件 表达式 n];
```
---

##### 高级查询
高级查询是单表查询的重点查询方式，能够完成许多较为复杂的查询方式。主要有以下四种查询方式。
###### （1）聚合查询
|函数名称|作用|
|:-:|:-:|
|COUNT()|返会某列行数|
|SUM()|返会某列值总和|
|AVG()|返会某列值的平均值|
|MAX()|返会某列值的最大值|
|MIN()|返会某列值的最小值|
-  1、count 函数

【用途】：用来统计表中记录数。
【结构】：
```SQL
SELECT COUNT(*) FROM 表 名
```
- 2、sum()函数

【用途】：SUM() 是 求和 函数， 用于 求出 表中 某个 字段 所有 值 的 总和。
【结构】：
```SQL
SELECT SUM( 字段 名) FROM 表 名;
```
- 3、AVG() 函数

【用途】：AVG() 函数 用于 求出 某个 字段 所有 值 的 平均值。
【结构】：
```SQL
SELECT AVG( 字段 名) 
FROM student(表名);
```
- 4、MAX()函数

【用途】：MAX() 函数 是 求 最大值 的 函数， 用于 求出 某个 字段 的 最大值。
【结构】：
```SQL
SELECT MAX( grade) FROM student;
```
- 5.MIN()函数

【用途】：MIN() 函数 是 求 最小值 的 函数， 用于 求出 某个 字段 的 最小值。
【结构】：
```SQL
SELECT MIN( grade) FROM student;
```
```SQL
-- 分数分析 
SELECT MAX(score),subject_no FROM tb_score WHERE subject_no = 1;
SELECT MIN(score),subject_no FROM tb_score WHERE subject_no = 1;
SELECT avg(score),subject_no FROM tb_score WHERE subject_no = 1;
SELECT stu_no,SUM(score) FROM tb_score WHERE stu_no = '20162155';
```
---
###### （2）查询结果排序

```SQL
SELECT 字段名 1, 字段名 2,…
 FROM 表  ORDER BY 字段名 1 
[ASC | DESC], 字段 名 2
 [ASC | DESC]…
```
【提示】默认的话，是升序排列。

【实例如下】：
```SQL
SELECT * FROM
	tb_score
WHERE
	subject_no = 1
ORDER BY
	score DESC;

-- 分数相同，按学号高到低排序 
SELECT * FROM
	tb_score
WHERE
	subject_no = 1
ORDER BY
	score DESC,
	stu_no DESC;
```
【结果展示】：
![排序结果](https://upload-images.jianshu.io/upload_images/17476267-0c788b548d3e3803.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
---
######（3）分组查询
``` SQL
SELECT 字段 名 1, 字段 名 2,…
 FROM 表 名 
GROUP BY 字段 名 1, 字段 名 2,…
[ HAVING 条件 表达式];
```
【特别 强调】：
- 1、 `GROUP BY` 一般 和 聚合函数一起 使用，如果查询的字段出现在`GROUP BY `后，却没有包含在聚合函数中，该字段显示的是分组后的第一条记录的值。
- 2、GROUP BY 和 HAVING 关键字 一起 使用时
> HAVING 关键字 和 WHERE 关键字 的 作用 相同， 都用 于 设置 条件 表达式 对 查询 结果 进行 过滤， 两者 的 区别 在于， HAVING 关键 字后 可以 跟 聚合 函数， 而 WHERE 关键字不能。
使用limit限制查询结果的数量
```SQL
SELECT
	AVG(score) avg_score,
	subject_no
FROM
	tb_score
GROUP BY
	subject_no;
```
对比-分析
```SQL
SELECT
	AVG(score) avg_score,
	subject_no
FROM
	tb_score
WHERE
	score >= 60
GROUP BY
	subject_no
HAVING
	avg_score > 80;
```
【提示】关于HAVING和where  的区别：
-  having：用于过滤组。使用于group by 后面。
- where：用于过滤记录； 
---
######（4）使用LIMIT限制查询结果数量
【介绍】LIMIT: 用于分页；
```SQL
SELECT 字段 名 1, 字段 名 2,…
FROM 表 名 LIMIT [OFFSET,] 记录数
```
【备注】： OFFSET 若不设置，默认为0；
``` SQL
SELECT * FROM tb_student LIMIT 0, 3;
SELECT * FROM tb_student LIMIT 2, 3;
SELECT * FROM tb_student LIMIT 5, 3;
```
---
