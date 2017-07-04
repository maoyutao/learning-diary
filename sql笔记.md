#基础概括
可以把 SQL  (结构化查询语言)分为两个部分：数据操作语言 (DML) 和 数据定义语言 (DDL)。

查询和更新指令构成了 SQL 的 DML 部分：

SELECT - 从数据库表中获取数据

INSERT INTO - 向数据库表中插入数据

UPDATE - 更新数据库表中的数据

DELETE - 从数据库表中删除数据


SQL 的数据定义语言 (DDL) 部分使我们有能力创建或删除表格。我们也可以定义索引（键），规定表之间的链接，以及施加表间的约束。

SQL 中最重要的 DDL 语句:

CREATE DATABASE - 创建新数据库

ALTER DATABASE - 修改数据库

CREATE TABLE - 创建新表

ALTER TABLE - 变更（改变）数据库表

DROP TABLE - 删除表

CREATE INDEX - 创建索引（搜索键）

DROP INDEX - 删除索引


链接数据库
mysql -h localhost -u username -p password

显示所有数据库
show databases

选定数据库
use database_name

查看所有表
show tables

查看表中内容
select * from table_name

#具体语句
###创建：

`CREATE DATABASE database_name`

```
CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
)
```
![数据类型]
(https://ws4.sinaimg.cn/large/006tNc79ly1fgzk4px01uj30lo0citb0.jpg)
###删除：

`DROP TABLE 表名称`

`DROP DATABASE 数据库名称`

`TRUNCATE TABLE 表名称`(仅仅删除表格中的数据)

###改动：

```
ALTER TABLE table_name
ADD column_name datatype
```
```
ALTER TABLE table_name 
DROP COLUMN column_name
```
```
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```
(修改列的数据类型）
***
下文举例用"persons"表：

| Id	|LastName	| FirstName | Address|	City|
|------|:--------:|:----------:|:-------|:----:|
| 1	    |Adams|	John	|Oxford Street|	London|
| 2  	|Bush	|George|	Fifth Avenue	|New York|
|	3  |Carter	|Thomas|	Changan Street|Beijing|
|	4  |mike	|Thomas|	tsinghua Street|Beijing|

***
###SQL SELECT 语句
* **普通**

 `SELECT 列名称 FROM 表名称`

 eg.

 `SELECT LastName,FirstName FROM persons`

 |LastName	| FirstName | 
|:--------:|:----------:|
|Adams|	John	|
|Bush	|George|
|Carter	|Thomas|
|mike	|Thomas|
* **选取所有的列***
 
 `SELECT * FROM Persons`
* **选取不重复的 关键词 distinct**

 `SELECT DISTINCT 列名称 FROM 表名称`
 
 eg.
 
 `SELECT DISTINCT Firstname FROM persons`
 
 | FirstName | 
|:----------:|
|	John	|
|George|
|Thomas|

* **有条件地选取哪几行 关键词where**

 `SELECT 列名称 FROM 表名称 WHERE 列 运算符 值`
 
 操作符：= <> > < >= <= BETWEEN LIKE (注意：等于只有一个等于）
 
 eg.
 
 `SELECT * FROM Persons WHERE City='Beijing'`
 
 | Id	|LastName	| FirstName | Address|	City|
|------|:--------:|:----------:|:-------|:----:|
|	3  |Carter	|Thomas|	Changan Street|Beijing|
|	4  |mike	|Thomas|	tsinghua Street|Beijing|
**注意**：我们在例子中的条件值周围使用的是单引号。
SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。如果是数值，不要使用引号（html里属性数值也要引号）。

 多个条件 ：AND 和 OR 运算符
 
 `SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'`
 `SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'`

* **排序 关键词ORDER BY**

  `SELECT LastName,FirstName FROM persons ORDER BY city,id`
   (字母顺序，默认升序，一样的看第二个指标）
   
  降序 DESC：
  `SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC`
  
  
###SQL INSERT语句
用于向表格中插入新的行 插在最后一行

  `INSERT INTO 表名称 VALUES (值1, 值2,....)`
  
  或
  
  `INSERT INTO 表名称(列1, 列2,...) VALUES (值1, 值2,....)`
  
###SQL UPDATE语句
用于修改表中的数据 where限制哪行

`UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值`(一句可改多值）

```
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing'
WHERE LastName = 'Wilson'
```

###SQL DELETE 语句
用于删除表中的行

`DELETE FROM 表名称 WHERE 列名称 = 值`

删除所有行:

`DELETE FROM table_name`

或

`DELETE * FROM table_name`

#高级教程
1. 返回规定数目的记录 limit

 ```
SELECT column_name(s)
FROM table_name
LIMIT number
```

2. where 相关

* like   and not like
 
 LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。
 `
 SELECT * FROM Persons
WHERE City LIKE 'N%'`
 `SELECT * FROM Persons
WHERE City LIKE '%g'`
`SELECT * FROM Persons
WHERE City LIKE '%lon%'`
`SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'`

 **通配符**<br/>
 % 替代一个或多个字符<br/>
 _ 代替一个字符<br/>
 [13nd] 13nd中的一个字符<br/>
 [!13nd]或[^13nd] 不是13nd的一个字符<br/>
 
 ```
SELECT * FROM Persons
WHERE City LIKE '[ALN]%'
```
(找ALN开头的）
* IN 操作符 
 允许我们在 WHERE 子句中规定多个值
 
 ```
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```
上面[]的字符串版
 
* BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

 ```
 SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
```

 ```
SELECT * FROM Persons
WHERE LastName
NOT BETWEEN 'Adams' AND 'Carter'
```

 重要事项：不同的数据库对 BETWEEN...AND 操作符的处理方式是有差异的。某些数据库会列出介于 "Adams" 和 "Carter" 之间的人，但不包括 "Adams" 和 "Carter" ；某些数据库会列出介于 "Adams" 和 "Carter" 之间并包括 "Adams" 和 "Carter" 的人；而另一些数据库会列出介于 "Adams" 和 "Carter" 之间的人，包括 "Adams" ，但不包括 "Carter" 。

3.Alias（别名）

* 表的 SQL Alias 语法（写起来方便）

```
SELECT po.OrderID, p.LastName, p.FirstName
FROM Persons AS p, Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
```

* 列的 SQL Alias 语法（改变了结果集）

```
SELECT LastName AS Family, FirstName AS Name
FROM Persons
```
展示的结果 列就是family和name

4.JOIN ON

* *inner join ／join*（最少的 相等才显示)<br/>
 **普通形式**
 
 ```
 SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons, Orders
WHERE Persons.Id_P = Orders.Id_P 
```
 **join形式**
 
 ```
 SELECT Persons.LastName, Persons.FirstName, Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
```
* *left join*

 ```
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```
LEFT JOIN 关键字会从左表 (table_name1) 那里返回所有的行，即使在右表 (table_name2) 中没有匹配的行。 （就显示null）

* *right join*<br/>
RIGHT JOIN 关键字会右表 (table_name2) 那里返回所有的行，即使在左表 (table_name1) 中没有匹配的行。
* *full join*<br/>
 FULL JOIN 关键字会从左表 (Persons) 和右表 (Orders) 那里返回所有的行。如果 "Persons" 中的行在表 "Orders" 中没有匹配，或者如果 "Orders" 中的行在表 "Persons" 中没有匹配，这些行同样会列出。

5  UNION （只列出不等的） UNION ALL （重复的也会列出）
 
```
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```
```
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```
请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
**join 是把列拼起来  union 是重叠列**

6 SELECT INTO 语句<br/>
SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表。<br/>
SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。
(还可存到别的数据库中的表 用in）

```
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```

```
SELECT *
INTO Persons IN 'Backup.mdb'
FROM Persons
```
(还可以结合where等 还可从一个以上的表取列）

```
SELECT Persons.LastName,Orders.OrderNo
INTO Persons_Order_Backup
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P
```

7 约束 (Constraints)

约束用于限制加入表的数据的类型。
可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。

* NOT NULL 约束

 ```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
约束强制字段始终包含值

* UNIQUE 约束

 ```
 CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (Id_P)
)
```
如果需要命名 UNIQUE 约束，以及为多个列定义 UNIQUE 约束，请使用下面的 SQL 语法：

 ```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
)
```
当表已被创建时

 ```
ALTER TABLE Persons
ADD UNIQUE (Id_P)
```
```
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
```
撤销 UNIQUE 约束

 ```
ALTER TABLE Persons
DROP INDEX uc_PersonID
```
*  PRIMARY KEY 约束

 PRIMARY KEY 约束唯一标识数据库表中的每条记录。<br/>
主键必须包含唯一的值。<br/>
主键列不能包含 NULL 值。<br/>
每个表都应该有一个主键，并且每个表只能有一个主键。

 ```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (Id_P)
)
```
```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
)
```
```
ALTER TABLE Persons
ADD PRIMARY KEY (Id_P)
```
```
ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (Id_P)
```
如果使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）

 ```
ALTER TABLE Persons
DROP PRIMARY KEY
```
* FOREIGN KEY 约束

 一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY
 FOREIGN KEY 约束用于预防破坏表之间连接的动作。<br/>
FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。
 
 ```
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
)
```
```
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
)
```
```
ALTER TABLE Orders
ADD FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
```
```
ALTER TABLE Orders
ADD CONSTRAINT fk_PerOrders
FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
```
```
ALTER TABLE Orders
DROP FOREIGN KEY fk_PerOrders
```
* CHECK 约束

 CHECK 约束用于限制列中的值的范围。

 ```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (Id_P>0)
)
```
```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
)
```
```
ALTER TABLE Persons
ADD CHECK (Id_P>0)
```
```
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
```
```
ALTER TABLE Persons
DROP CHECK chk_Person
```
* DEFAULT 约束
 DEFAULT 约束用于向列中插入默认值。
 
 ```
 CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes'
)
```
通过使用类似 GETDATE() 这样的函数，DEFAULT 约束也可以用于插入系统值：

 ```
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
OrderDate date DEFAULT GETDATE()
)
```
```
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES'
```
```
ALTER TABLE Persons
ALTER City DROP DEFAULT
```

8 CREATE INDEX 语句用于在表中创建索引

```
CREATE INDEX PersonIndex
ON Person (LastName, FirstName)
```

9 AUTO INCREMENT 字段
我们通常希望在每次插入新记录时，自动地创建主键字段的值。
我们可以在表中创建一个 auto-increment 字段。

```
CREATE TABLE Persons
(
P_Id int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (P_Id)
)
```
默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。
要让 AUTO_INCREMENT 序列以其他的值起始，请使用下列 SQL 语法：

```
ALTER TABLE Persons AUTO_INCREMENT=100
```
故要在 "Persons" 表中插入新记录，我们不必为 "P_Id" 列规定值（会自动添加一个唯一的值）

10 视图

视图是基于 SQL 语句的结果集的可视化的表。
视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。
简单的说，视图是按照你的sql语句生成的一个虚拟的东西，本身并不占数据库空间
譬如有这个表
create table table1 (id int,name varchar(10))
然后有这么一个视图
create view view1 as select id from table1
当你表里的数据增加或者删除的时候，你视图里的内容也随着变化
总之你不能对视图进行update或者insert into操作
说白了，就是视图的变化随着表的变化而变化
除非重新create or replace view1 才能把这个视图中的东西更改掉

```
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```
```
CREATE VIEW [Current Product List] AS
SELECT ProductID,ProductName
FROM Products
WHERE Discontinued=No
```
```
SELECT * FROM [Current Product List]
WHERE CategoryName='Beverages'
```
```
SQL DROP VIEW Syntax
DROP VIEW view_name
```
11 NULL

不能用=NULL 只有is null和is not null

```
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL
```
```
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NOT NULL
```
```
SELECT ProductName,UnitPrice*(UnitsInStock+IFNULL(UnitsOnOrder,0))
FROM Products
```
MYSQL IFNULL(expr1,expr2)          
如果expr1不是NULL，IFNULL()返回expr1，否则它返回expr2。

```
SELECT ProductName,UnitPrice*(UnitsInStock+COALESCE(UnitsOnOrder,0))
FROM Products
```

##注意事项
* SQL 对大小写不敏感 


* 在使用mysql执行update的时候，如果不是用主键当where语句，会报如下错误，使用主键用于where语句中正常。
异常内容：Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, toggle the option in Preferences -> SQL Queries and reconnect.这是因为MySql运行在safe-updates模式下，该模式会导致非主键条件下无法执行update或者delete命令，执行命令SET SQL_SAFE_UPDATES = 0(k看左边  是下划线不是斜体）;如果想要提高数据库安全等级，可以在恢复回原有的设置，执行命令：SET SQL_SAFE_UPDATES = 1;
执行成功后，以delete命令为例，非主键情况下又报错了，说明安全等级修改成功