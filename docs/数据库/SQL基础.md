# SQL基础

>   参考文档: [SQL 教程](https://www.w3school.com.cn/sql/index.asp)

## SQL简介

**SQL 是用于访问和处理数据库的标准的计算机语言。**

### 什么是 SQL

*   SQL 指结构化查询语言
*   SQL 使我们有能力访问数据库
*   SQL 是一种 ANSI 的标准计算机语言

注：ANSI，美国国家标准化组织

### DBMS - 数据库管理系统（Database Management System）

数据库管理系统是一种可以访问数据库中数据的计算机程序。

DBMS 使我们有能力在数据库中提取、修改或者存贮信息。

不同的 DBMS 提供不同的函数供查询、提交以及修改数据。

### RDBMS- 关系数据库管理系统（Relational Database Management System）

RDBMS 是 SQL 的基础，同样也是所有现代数据库系统的基础，比如 MS SQL Server, IBM DB2, Oracle, MySQL 以及 Microsoft Access。

RDBMS 中的数据存储在被称为表（tables）的数据库对象中。

表是相关的数据项的集合，它由列和行组成。

### DML 和 DDL

可以把 SQL 分为两个部分：数据操作语言 (DML) 和 数据定义语言 (DDL)。

SQL (结构化查询语言)是用于执行查询的语法。但是 SQL 语言也包含用于更新、插入和删除记录的语法。

查询和更新指令构成了 SQL 的 DML 部分：

*   *SELECT* - 从数据库表中获取数据
*   *UPDATE* - 更新数据库表中的数据
*   *DELETE* - 从数据库表中删除数据
*   *INSERT INTO* - 向数据库表中插入数据

SQL 的数据定义语言 (DDL) 部分使我们有能力创建或删除表格。我们也可以定义索引（键），规定表之间的链接，以及施加表间的约束。

SQL 中最重要的 DDL 语句:

*   *CREATE DATABASE* - 创建新数据库
*   *ALTER DATABASE* - 修改数据库
*   *CREATE TABLE* - 创建新表
*   *ALTER TABLE* - 变更（改变）数据库表
*   *DROP TABLE* - 删除表
*   *CREATE INDEX* - 创建索引（搜索键）
*   *DROP INDEX* - 删除索引

### 注意事项

#### 大小写问题

一定要记住，SQL 对大小写不敏感！

#### SQL 语句后面的分号

某些数据库系统要求在每条 SQL 命令的末端使用分号。

分号是在数据库系统中分隔每条 SQL 语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的语句。如果使用的是 MS Access 和 SQL Server 2000，则不必在每条 SQL 语句之后使用分号，不过某些数据库软件要求必须使用分号。

## 基础语法

### SELECT

SELECT 语句用于从表中选取数据。

结果被存储在一个结果表中（称为结果集）。

```sql
SELECT 列名称 FROM 表名称
```

以及：

```sql
SELECT * FROM 表名称
```

**注释：**SQL 语句对大小写不敏感。SELECT 等效于 select。

### SELECT DISTINCT

在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

```sql
SELECT DISTINCT 列名称 FROM 表名称
```

### WHERE

如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句。

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

下面的运算符可在 WHERE 子句中使用：

| 操作符  | 描述         |
| :------ | :----------- |
| =       | 等于         |
| <>      | 不等于       |
| >       | 大于         |
| <       | 小于         |
| >=      | 大于等于     |
| <=      | 小于等于     |
| BETWEEN | 在某个范围内 |
| LIKE    | 搜索某种模式 |

**注释：**在某些版本的 SQL 中，操作符 <> 可以写为 !=。

**引号的使用**

SQL 使用单引号来环绕*文本值*（大部分数据库系统也接受双引号）。如果是*数值*，请不要使用引号。

**AND 和 OR 运算符**

AND 和 OR 运算符用于基于一个以上的条件对记录进行过滤。

AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。

如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

### GROUP BY

GROUP BY 语句用于结合合计函数，根据一个或多个列对结果集进行分组。

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
```

**GROUP BY 一个以上的列:**

我们也可以对一个以上的列应用 GROUP BY 语句，就像这样：

```sql
SELECT Customer,OrderDate,SUM(OrderPrice) FROM Orders
GROUP BY Customer,OrderDate
```

### HAVING

在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator valu
```

### ORDER BY

ORDER BY 语句用于根据指定的列对结果集进行排序。

ORDER BY 语句默认按照升序对记录进行排序。

如果希望按照降序对记录进行排序，可以使用 DESC 关键字。

**注意：**使用多列进行排序时，左边的列的排序优先级高于右侧，

示例：

```sql
SELECT exp1, exp2 FROM table1 ORDER BY exp1, exp2 DESC
```

先对exp1进行升序排序，若exp1相同，则按照exp2进行降序排序；若exp1没有重复，则不会对exp2排序。

### INSERT INTO

INSERT INTO 语句用于向表格中插入新的行。

```sql
INSERT INTO 表名称 VALUES (值1, 值2,....)
```

我们也可以指定所要插入数据的列：

```sql
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

### UPDATE

Update 语句用于修改表中的数据。

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

### DELETE

DELETE 语句用于删除表中的行。

```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```

**删除所有行**

可以在不删除表的情况下删除所有的行。这意味着**表的结构、属性和索引都是完整的**：

```sql
DELETE FROM table_name
```

或者：

```sql
DELETE * FROM table_name
```

### CREATE DATABASE

CREATE DATABASE 用于创建数据库。

```sql
CREATE DATABASE database_name
```

### CREATE TABLE

CREATE TABLE 语句用于创建数据库中的表。

```sql
CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
)
```

数据类型（data_type）规定了列可容纳何种数据类型。下面的表格包含了SQL中最常用的数据类型：

| 数据类型                                                     | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| integer(size) <br/>int(size) <br/>smallint(size)<br/>tinyint(size) | 仅容纳整数。在括号内规定数字的最大位数。                     |
| decimal(size,d)<br/>numeric(size,d)                          | 容纳带有小数的数字。"size" 规定数字的最大位数。"d" 规定小数点右侧的最大位数。 |
| char(size)                                                   | 容纳固定长度的字符串（可容纳字母、数字以及特殊字符）。在括号中规定字符串的长度。 |
| varchar(size)                                                | 容纳可变长度的字符串（可容纳字母、数字以及特殊的字符）。在括号中规定字符串的最大长度。 |
| date(yyyymmdd)                                               | 容纳日期。                                                   |

### ALTER TABLE 

ALTER TABLE 语句用于在已有的表中添加、修改或删除列。

如需在表中添加列，请使用下列语法:

```sql
ALTER TABLE table_name
ADD column_name datatype
```

要删除表中的列，请使用下列语法：

```sql
ALTER TABLE table_name 
DROP COLUMN column_name
```

**注释：**某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。

要改变表中列的数据类型，请使用下列语法：

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

### CREATE INDEX

**CREATE INDEX 语句用于在表中创建索引。**

**在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。**

您可以在表中创建索引，以便更加快速高效地查询数据。

用户无法看到索引，它们只能被用来加速搜索/查询。

**注释：**更新一个包含索引的表需要比更新一个没有索引的表更多的时间，这是由于索引本身也需要更新。因此，理想的做法是**仅仅在常常被搜索的列（以及表）上面创建索引**。

**CREATE INDEX 语法:**

在表上创建一个简单的索引。允许使用重复的值：

```sql
CREATE INDEX index_name
ON table_name (column_name [DESC])
```

**注释：**"column_name" 规定需要索引的列。

**SQL CREATE UNIQUE INDEX 语法:**

在表上创建一个唯一的索引。**唯一的索引意味着两个行不能拥有相同的索引值**。

```sql
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

### CREATE VIEW

什么是视图？

在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。

视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。

**注释：**数据库的设计和结构不会受到视图中的函数、where 或 join 语句的影响。

SQL CREATE VIEW 语法:

```sql
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

**注释：**视图总是显示最近的数据。每当用户查询视图时，数据库引擎通过使用 SQL 语句来重建数据。

## 操作符

### TOP (LIMIT)

TOP 子句用于规定要返回的记录的数目。

对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。

**注释：**并非所有的数据库系统都支持 TOP 子句。

MySQL 语法：

```sql
SELECT column_name(s)
FROM table_name
LIMIT numbe
```

### LIKE 

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern
```

**通配符:**

在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

| 通配符                     | 描述                       |
| :------------------------- | :------------------------- |
| %                          | 替代一个或多个字符         |
| _                          | 仅替代一个字符             |
| [charlist]                 | 字符列中的任何单一字符     |
| [^charlist]或者[!charlist] | 不在字符列中的任何单一字符 |

### IN 

IN 操作符允许我们在 WHERE 子句中规定多个值。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```

### BETWEEN 

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

**重要事项：**不同的数据库对 BETWEEN...AND 操作符的处理方式是有差异的。所以，请检查你的数据库是如何处理 BETWEEN....AND 操作符的！

### SQL Alias

**通过使用 SQL，可以为列名称和表名称指定别名（Alias）。**

表的 SQL Alias 语法

```sql
SELECT column_name(s)
FROM table_name
AS alias_name
```

列的 SQL Alias 语法

```sql
SELECT column_name 
AS alias_name
FROM table_name
```

### JOIN

**SQL join 用于根据两个或多个表中的列之间的关系，从这些表中查询数据。**

**Join 和 Key**

有时为了得到完整的结果，我们需要从两个或更多的表中获取结果。我们就需要执行 join。

数据库中的表可通过键将彼此联系起来。主键（Primary Key）是一个列，在这个列中的每一行的值都是唯一的。在表中，每个主键的值都是唯一的。这样做的目的是在不重复每个表中的所有数据的情况下，把表间的数据交叉捆绑在一起。

我们可以通过引用两个表的方式，从两个表中获取数据，我们也可以使用关键词 JOIN 来从两个表中获取数据。

**不同的 SQL JOIN**

*   JOIN: 如果表中有至少一个匹配，则返回行
*   LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行
*   RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行
*   FULL JOIN: 只要其中一个表中存在匹配，就返回行

#### INNER JOIN 

在表中存在至少一个匹配时，INNER JOIN 关键字返回行。

```sql
SELECT column_name(s)
FROM table_name1
INNER JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**INNER JOIN 与 JOIN 是相同的。

#### LEFT JOIN 

LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。

```sql
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， LEFT JOIN 称为 LEFT OUTER JOIN。

#### RIGHT JOIN

RIGHT JOIN 关键字会右表 (table_name2) 那里返回所有的行，即使在左表 (table_name1) 中没有匹配的行。

```sql
SELECT column_name(s)
FROM table_name1
RIGHT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， RIGHT JOIN 称为 RIGHT OUTER JOIN。

#### FULL JOIN

只要其中某个表存在匹配，FULL JOIN 关键字就会返回行。

```sql
SELECT column_name(s)
FROM table_name1
FULL JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

**注释：**在某些数据库中， FULL JOIN 称为 FULL OUTER JOIN。

### UNION

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，**UNION 内部的 SELECT 语句必须拥有相同数量的列，也必须拥有相似的数据类型，同时每条 SELECT 语句中的列的顺序必须相同**。

SQL UNION 语法:

```sql
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

SQL UNION ALL 语法:

```sql
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```

另外，**UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名**。

### SELECT INTO

SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。

SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。

您可以把所有的列插入新表：

```sql
SELECT *
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

或者只把希望的列插入新表：

```sql
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

### SQL DROP

**通过使用 DROP 语句，可以轻松地删除索引、表和数据库。**

**SQL DROP INDEX 语句:**

我们可以使用 DROP INDEX 命令删除表格中的索引。

用于 MySQL 的语法:

```sql
ALTER TABLE table_name DROP INDEX index_name
```

**SQL DROP DATABASE 语句:**

DROP DATABASE 语句用于删除数据库：

```sql
DROP DATABASE 数据库名称
```

### TRUNCATE

如果我们仅仅需要除去表内的数据，但并不删除表本身，那么我们该如何做呢？

请使用 TRUNCATE TABLE 命令（仅仅删除表格中的数据）：

```sql
TRUNCATE TABLE 表名称
```

### NULL

如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。这意味着该字段将以 NULL 值保存。

NULL 值的处理方式与其他值不同。

NULL 用作未知的或不适用的值的占位符。

**注释：**无法比较 NULL 和 0；**它们是不等价的**。

那么我们如何测试 NULL 值呢？

无法使用比较运算符来测试 NULL 值，比如 =, <, 或者 <>。

我们必须使用 IS NULL 和 IS NOT NULL 操作符。

## 约束 (Constraints)

### 约束简介

约束用于限制加入表的数据的类型。

可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。

### NOT NULL

NOT NULL 约束强制列不接受 NULL 值。

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

### UNIQUE

UNIQUE 约束唯一标识数据库表中的每条记录。

UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。

PRIMARY KEY 拥有自动定义的 UNIQUE 约束。

请注意，**每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束**。

### PRIMARY KEY

PRIMARY KEY 约束唯一标识数据库表中的每条记录。

主键必须包含唯一的值。

主键列不能包含 NULL 值。

每个表都应该有一个主键，并且每个表只能有一个主键。

### FOREIGN KEY

一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。

FOREIGN KEY 约束用于预防破坏表之间连接的动作。

FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

### CHECK

CHECK 约束用于限制列中的值的范围。

如果对单个列定义 CHECK 约束，那么该列只允许特定的值。

如果对一个表定义 CHECK 约束，那么此约束会在特定的列中对值进行限制。

### DEFAULT

DEFAULT 约束用于向列中插入默认值。

如果没有规定其他的值，那么会将默认值添加到所有的新记录。

### AUTO INCREMENT

**Auto-increment 会在新记录插入表中时生成一个唯一的数字。**

我们通常希望在每次插入新记录时，自动地创建主键字段的值。

我们可以在表中创建一个 auto-increment 字段。

## 函数

### 函数简介

内建 SQL 函数的语法是：

```sql
SELECT function(列) FROM 表
```

在 SQL 中，基本的函数类型和种类有若干种。函数的基本类型是：

*   Aggregate 函数

    Aggregate 函数的操作面向一系列的值，并返回一个单一的值。

    **注释：**如果在 SELECT 语句的项目列表中的众多其它表达式中使用 SELECT 语句，则这个 SELECT 必须使用 GROUP BY 语句

*   Scalar 函数

    Scalar 函数的操作面向某个单一的值，并返回基于输入值的一个单一的值。

### AVG

AVG 函数返回数值列的平均值。NULL 值不包括在计算中。

```sql
SELECT AVG(column_name) FROM table_name
```

### COUNT

**SQL COUNT(column_name) 语法:**

COUNT(column_name) 函数返回指定列的值的数目（NULL 不计入）：

```sql
SELECT COUNT(column_name) FROM table_name
```

**SQL COUNT(*) 语法:**

COUNT(*) 函数返回表中的记录数：

```sql
SELECT COUNT(*) FROM table_name
```

**SQL COUNT(DISTINCT column_name) 语法:**

COUNT(DISTINCT column_name) 函数返回指定列的不同值的数目：

```sql
SELECT COUNT(DISTINCT column_name) FROM table_name
```

**注释：**COUNT(DISTINCT) 适用于 ORACLE 和 Microsoft SQL Server，但是无法用于 Microsoft Access。

### FIRST

FIRST() 函数返回指定的字段中第一个记录的值。

**提示：**可使用 ORDER BY 语句对记录进行排序。

```sql
SELECT FIRST(column_name) FROM table_name
```

### LAST

LAST() 函数返回指定的字段中最后一个记录的值。

**提示：**可使用 ORDER BY 语句对记录进行排序。

```sql
SELECT LAST(column_name) FROM table_name
```

### MAX

MAX 函数返回一列中的最大值。NULL 值不包括在计算中。

```sql
SELECT MAX(column_name) FROM table_name
```

**注释：**MIN 和 MAX 也可用于文本列，以获得按字母顺序排列的最高或最低值。

### MIN

MIN 函数返回一列中的最小值。NULL 值不包括在计算中。

```sql
SELECT MIN(column_name) FROM table_name
```

**注释：**MIN 和 MAX 也可用于文本列，以获得按字母顺序排列的最高或最低值。

### SUM

SUM 函数返回数值列的总数（总额）。

```sql
SELECT SUM(column_name) FROM table_name
```

**合计函数 (比如 SUM) 常常需要添加 GROUP BY 语句。**

### UCASE

UCASE 函数把字段的值转换为大写。

```sql
SELECT UCASE(column_name) FROM table_name
```

### LCASE

LCASE 函数把字段的值转换为小写。

```sql
SELECT LCASE(column_name) FROM table_name
```

### MID

MID 函数用于从文本字段中提取字符。

```sql
SELECT MID(column_name,start[,length]) FROM table_names
```

| 参数        | 描述                                                        |
| :---------- | :---------------------------------------------------------- |
| column_name | 必需。要提取字符的字段。                                    |
| start       | 必需。规定开始位置（起始值是 1）。                          |
| length      | 可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。 |

### LEN

LEN 函数返回文本字段中值的长度。

```sql
SELECT LEN(column_name) FROM table_name
```

### ROUND

ROUND 函数用于把数值字段舍入为指定的小数位数。

```sql
SELECT ROUND(column_name,decimals) FROM table_name
```

| 参数        | 描述                         |
| :---------- | :--------------------------- |
| column_name | 必需。要舍入的字段。         |
| decimals    | 必需。规定要返回的小数位数。 |

### NOW

NOW 函数返回当前的日期和时间。

**提示：**如果您在使用 Sql Server 数据库，请使用 getdate() 函数来获得当前的日期时间。

```sql
SELECT NOW() FROM table_name
```

### FORMAT

FORMAT 函数用于对字段的显示进行格式化。

```sql
SELECT FORMAT(column_name,format) FROM table_name
```

| 参数        | 描述                   |
| :---------- | :--------------------- |
| column_name | 必需。要格式化的字段。 |
| format      | 必需。规定格式。       |

