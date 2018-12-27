## 结构化查询语言SQL介绍和基本操作

[结构化查询语言SQL介绍和基本操作](#结构化查询语言sql介绍和基本操作)
- [了解SQL](#了解sql)
- [DQL语言](#dql语言)
	- [检索数据](#检索数据)
	- [排序检索数据](#排序检索数据)
	- [过滤数据](#过滤数据)
	- [数据过滤](#数据过滤)
	- [用通配符进行过滤](#用通配符进行过滤)
	- [用正则表达式进行搜索](#用正则表达式进行搜索)
	- [创建计算字段](#创建计算字段)
	- [使用数据处理函数](#使用数据处理函数)
	- [汇总数据](#汇总数据)
	- [分组数据](#分组数据)
	- [使用子查询](#使用子查询)
	- [联结表](#联结表)
	- [创建高级联结](#创建高级联结)
	- [组合查询](#组合查询)
	- [全文本搜索](#全文本搜索)
- [DDL语言](#ddl语言)
	- [创建和操纵表](#创建和操纵表)
- [DML语言](#dml语言)
	- [插入数据](#插入数据)
	- [更新和删除数据](#更新和删除数据)
- [使用视图](#使用视图)
- [使用存储过程](#使用存储过程)
- [使用游标](#使用游标)
- [使用触发器](#使用触发器)
- [管理事务处理](#管理事务处理)
- [全球化和本地化](#全球化和本地化)
- [DCL语言](#dcl语言)
	- [安全管理](#安全管理)
- [其他](#其他)
	- [MySQL语句的语法](#mysql语句的语法)
	- [MySQL数据类型](#mysql数据类型)
- [实战项目](#实战项目)
- [总结](#总结)

---

> 本章将通过丰富的实例对 SQL 语言的基础进行详细介绍,MySQL,使得读者不但能够学习到
标准 SQL 的使用,又能够学习到 MySQL 中一些扩展 SQL 的使用方法。该教案根据《MySQL必知必会》撰写。

### 了解SQL

#### 什么是SQL

> SQL 简介

当面对一个陌生的数据库时,通常需要一种方式与它进行交互,以完成用户所需要的各种工
作,这个时候,就要用到 SQL 语言了。

SQL 是 Structure Query Language(结构化查询语言)的缩写,它是使用关系模型的数据库应
用语言,由 IBM 在 20 世纪 70 年代开发出来,作为 IBM 关系数据库原型 System R 的原型关
系语言,实现了关系数据库中的信息检索。

20 世纪 80 年代初,美国国家标准局(ANSI)开始着手制定 SQL 标准,最早的 ANSI 标准于
1986 年完成,就被叫作 SQL-86。标准的出台使 SQL 作为标准关系数据库语言的地位得到了
加强。SQL 标准目前已几经修改更趋完善。

正是由于 SQL 语言的标准化,所以大多数关系型数据库系统都支持 SQL 语言,它已经发展
成为多种平台进行交互操作的底层会话语言。

这里用了(My)SQL 这样的标题,目的是在介绍标准 SQL 的同时,也将一些 MySQL 在标准 SQL
上的扩展一同介绍给大家。希望读者看完本节后,能够对标准 SQL 的基本语法和 MySQL 的
部分扩展语法有所了解。

> SQL 分类

SQL 语句主要可以划分为以下 4 个类别。
- DDL(Data Definition Languages)语句:数据定义语言,这些语句定义了不同的数据段、
数据库、表、列、索引等数据库对象的定义。常用的语句关键字主要包括 create、drop、alter
等。
- DML(Data Manipulation Language)语句:数据操纵语句,用于添加、删除、更新和查
询数据库记录,并检查数据完整性,常用的语句关键字主要包括 insert、delete、udpate 和
select 等。
- DCL(Data Control Language)语句:数据控制语句,用于控制不同数据段直接的许可和
访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的
语句关键字包括 grant、revoke 等。
- DQL(Data Query Language)语句:数据查询语句，用于从一个或多个表中检索
信息。

> MySQL中的语法格式

1. SQL语句是由简单的英语单词构成的。这些单词称为关键字,每个SQL语句都是由一个或多个关键字构成的。
2. 结束SQL语句 多条SQL语句必须以分号(;)分隔。MySQL如同多数DBMS一样,不需要在单条SQL语句后加分号。但特定的DBMS可能必须在单条SQL语句后加上分号。当然,如果
愿意可以总是加上分号。事实上,即使不一定需要,但加上分号肯定没有坏处。如果你使用的是 mysql命令行,必须加上分号来结束 SQL 语句。
3. SQL语句和大小写 请注意,SQL语句不区分大小写,因此SELECT 与 select 是相同的。同样,写成 Select 也没有关系。许多SQL开发人员喜欢对所有SQL关键字使用大写,而对所有列和表名使用小写,这样做使代码更易于阅读和调试。
4. 使用空格 在处理SQL语句时,其中所有空格都被忽略。SQL语句可以在一行上给出,也可以分成许多行。多数SQL开发人员认为将SQL语句分成多行更容易阅读和调试。
---

> 学习样例表获取方法

学习样例mysql_scripts.zip存放在uplooking教室共享路径为http://classroom.example.com/content/MYSQL/04-othersmysql_scripts.zip 。
可以在连接互联网的情况下，访问http://www.forta.com/books/0672327120/

```shell
[root@mastera0 ~]# yum install -y wget vim net-tools unzip
[root@mastera0 ~]# wget http://classroom.example.com/content/MYSQL/04-othersmysql_scripts.zip
[root@mastera0 ~]# ls
anaconda-ks.cfg  mysql_scripts.zip
[root@mastera0 ~]# unzip mysql_scripts.zip
[root@mastera0 ~]# ls
anaconda-ks.cfg  create.sql  mysql_scripts.zip  populate.sql
[root@mastera0 ~]# mysql -uroot -puplooking
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 5.5.44-MariaDB MariaDB Server

Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.00 sec)

MariaDB [(none)]> exit
Bye
# 导入create.sql到test库，将会创建一些表
[root@mastera0 ~]# mysql -uroot -puplooking test < create.sql
[root@mastera0 ~]# mysql -uroot -puplooking -e "show tables from test";
+----------------+
| Tables_in_test |
+----------------+
| customers      |
| orderitems     |
| orders         |
| productnotes   |
| products       |
| vendors        |
+----------------+
# 导入populate.sql到test库，将会向表中新增数据
[root@mastera0 ~]# mysql -uroot -puplooking test < populate.sql
[root@mastera0 ~]# mysql -uroot -puplooking -e "select * from test.vendors";
+---------+----------------+-----------------+-------------+------------+----------+--------------+
| vend_id | vend_name      | vend_address    | vend_city   | vend_state | vend_zip | vend_country |
+---------+----------------+-----------------+-------------+------------+----------+--------------+
|    1001 | Anvils R Us    | 123 Main Street | Southfield  | MI         | 48075    | USA          |
|    1002 | LT Supplies    | 500 Park Street | Anytown     | OH         | 44333    | USA          |
|    1003 | ACME           | 555 High Street | Los Angeles | CA         | 90046    | USA          |
|    1004 | Furball Inc.   | 1000 5th Avenue | New York    | NY         | 11111    | USA          |
|    1005 | Jet Set        | 42 Galaxy Road  | London      | NULL       | N16 6PS  | England      |
|    1006 | Jouets Et Ours | 1 Rue Amusement | Paris       | NULL       | 45678    | France       |
+---------+----------------+-----------------+-------------+------------+----------+--------------+

[root@mastera0 ~]# mysql -uroot -puplooking
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 15
Server version: 5.5.44-MariaDB MariaDB Server

Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
# 查询所有的数据库
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.00 sec)
# 使用test库
MariaDB [(none)]> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
# 查询test库中所有表
MariaDB [test]> show tables;
+----------------+
| Tables_in_test |
+----------------+
| customers      |
| orderitems     |
| orders         |
| productnotes   |
| products       |
| vendors        |
+----------------+
6 rows in set (0.00 sec)
# 查询test库中products表的结构，  describe是show columns from的别名
# DESCRIBE tbl_name
# SHOW [FULL] COLUMNS FROM tbl_name [FROM db_name] [like_or_where]

MariaDB [test]> desc products;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| prod_id    | char(10)     | NO   | PRI | NULL    |       |
| vend_id    | int(11)      | NO   | MUL | NULL    |       |
| prod_name  | char(255)    | NO   |     | NULL    |       |
| prod_price | decimal(8,2) | NO   |     | NULL    |       |
| prod_desc  | text         | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

MariaDB [test]> show columns from test.products;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| prod_id    | char(10)     | NO   | PRI | NULL    |       |
| vend_id    | int(11)      | NO   | MUL | NULL    |       |
| prod_name  | char(255)    | NO   |     | NULL    |       |
| prod_price | decimal(8,2) | NO   |     | NULL    |       |
| prod_desc  | text         | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

```

> 本教案中所有章节中使用的数据库表都是关系表，关于每个表及关系的描述,如下所示：

样例表为一个想象的随身物品推销商使用的订单录入系统,这些随身物品可能是你喜欢的卡通人物需要的(是的,卡通
人物,没人规定学习MySQL必须沉闷地学)。这些表用来完成以下几个任务:

* 管理供应商;
* 管理产品目录;
* 管理顾客列表;
* 录入顾客订单。

要完成这几个任务需要作为关系数据库设计成分的紧密联系的6个表。

**vendors 表** 存储销售产品的供应商。每个供应商在这个表中有一个记录,供应商ID( vend_id )列用来匹配产品和供应商。

|vendors 表的列|说明|
|:--|:--|
|vend_id| 唯一的供应商ID|
|vend_name| 供应商名|
|vend_address| 供应商的地址|
|vend_city| 供应商的城市|
|vend_state| 供应商的州|
|vend_zip| 供应商的邮政编码|
|vend_country| 供应商的国家|

**products 表** 包含产品目录,每行一个产品。每个产品有唯一的ID( prod_id 列),通过 vend_id (供应商的唯一ID)关联到它的供应商。

|products 表的列|说明|
|:--|:--|
|prod_id| 唯一的产品ID|
|vend_id| 产品供应商ID(关联到vendors表中的vend_id)|
|prod_name| 产品名|
|prod_price| 产品价格
|prod_desc |产品描述|

**customers 表** 存储所有顾客的信息。每个顾客有唯一的ID( cust_id列)。

|customers 表的列|说明|
|:--|:--|
|cust_id |唯一的顾客ID|
|cust_name| 顾客名|
|cust_address| 顾客的地址|
|cust_city |顾客的城市|
|cust_state |顾客的州|
|cust_zip |顾客的邮政编码|
|cust_country| 顾客的国家|
|cust_contact |顾客的联系名|
|cust_email |顾客的联系email地址|

**orderitems 表** 存储每个订单中的实际物品,每个订单的每个物品占
一行。对 orders 中的每一行, orderitems 中有一行或多行。每个订单物
品由订单号加订单物品(第一个物品、第二个物品等)唯一标识。订单
物品通过 order_num 列(关联到 orders 中订单的唯一ID)与它们相应的订
单相关联。此外,每个订单项包含订单物品的产品ID(它关联物品到
products 表)。

|orderitems 表的列|说明|
|:--|:--|
|order_num| 订单号(关联到orders表的order_num)|
|order_item| 订单物品号(在某个订单中的顺序)|
|prod_id| 产品ID(关联到products表的prod_id)|
|quantity| 物品数量|
|item_price| 物品价格|

**productnotes 表**存储与特定产品有关的注释。并非所有产品都有相
关的注释,而有的产品可能有许多相关的注释。

|productnotes 表的列|说明|
|:--|:--|
|note_id| 唯一注释ID|
|prod_id| 产品ID(对应于products表中的prod_id)|
|note_date| 增加注释的日期|
|note_text| 注释文本|

---

### DQL语言

DQL(Data Query Language)语句:数据查询语句，用于从一个或多个表中检索
信息。本章介绍如何使用 SELECT 语句从表中检索一个或多个数据列。

#### 检索数据

* 数据插入到数据库中后,就可以用 SELECT 命令进行各种各样的查询,使得输出的结果符合我们的要求。由于 SELECT 的语法很复杂,所有这里只介绍最基本的语法
* 大概,最经常使用的SQL语句就是 SELECT 语句了。它的用途是从一个或多个表中检索信息。
* 为了使用 SELECT 检索表数据,必须至少给出两条信息——想选择什么,以及从什么地方选择。

我们将从简单的SQL SELECT 语句开始介绍,此语句如下所示:

> 检索单个列
>>利 用 SELECT 语 句 从 products 表 中 检 索 一 个 名 为prod_name 的列。所需的列名在 SELECT 关键字之后给出, FROM关键字指出从其中检索数据的表名。


```shell
MariaDB [test]> select prod_name from products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)

```

**未排序数据** 如果读者自己试验这个查询,可能会发现显示输
出的数据顺序与这里的不同。出现这种情况很正常。如果没有
明确排序查询结果(下一章介绍)
,则返回的数据的顺序没有
特殊意义。返回数据的顺序可能是数据被添加到表中的顺序,
也可能不是。只要返回相同数目的行,就是正常的。

**使用空格** 在处理SQL语句时,其中所有空格都被忽略。SQL
语句可以在一行上给出,也可以分成许多行。多数SQL开发人
员认为将SQL语句分成多行更容易阅读和调试。

> 检索多个列
>> 使用 SELECT 语句从表 products中选择数据,指定3个列名prod_id,prod_name,prod_price,列名之间用逗号分隔。

```shell
MariaDB [test]> select prod_id,prod_name,prod_price from products;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| ANV01   | .5 ton anvil   |       5.99 |
| ANV02   | 1 ton anvil    |       9.99 |
| ANV03   | 2 ton anvil    |      14.99 |
| DTNTR   | Detonator      |      13.00 |
| FB      | Bird seed      |      10.00 |
| FC      | Carrots        |       2.50 |
| FU1     | Fuses          |       3.42 |
| JP1000  | JetPack 1000   |      35.00 |
| JP2000  | JetPack 2000   |      55.00 |
| OL1     | Oil can        |       8.99 |
| SAFE    | Safe           |      50.00 |
| SLING   | Sling          |       4.49 |
| TNT1    | TNT (1 stick)  |       2.50 |
| TNT2    | TNT (5 sticks) |      10.00 |
+---------+----------------+------------+
14 rows in set (0.00 sec)

```

**当心逗号** 在选择多个列时,一定要在列名之间加上逗号,但
最后一个列名后不加。如果在最后一个列名后加了逗号,将出
现错误。

**数据表示** 从上述输出可以看到,SQL语句一般返回原始的、
无格式的数据。数据的格式化是一个表示问题,而不是一个
检索问题。因此,表示(对齐和显示上面的价格值,用货币
符号和逗号表示其金额)一般在显示该数据的应用程序中规
定。一般很少使用实际检索出的原始数据(没有应用程序提供的格式)。


> 检索所有列
>> 除了指定所需的列外(如上所述,一个或多个列), SELECT 语句还可以检索所有的列而不必逐个列出它们。这可以通过在实际列名的位置使
用星号( * )通配符来达到,使用 SELECT 语句从表 products中选择所有数据。

```shell
MariaDB [test]> select * from products;
+---------+---------+----------------+------------+----------------------------------------------------------------+
| prod_id | vend_id | prod_name      | prod_price | prod_desc                                                      |
+---------+---------+----------------+------------+----------------------------------------------------------------+
| ANV01   |    1001 | .5 ton anvil   |       5.99 | .5 ton anvil, black, complete with handy hook                  |
| ANV02   |    1001 | 1 ton anvil    |       9.99 | 1 ton anvil, black, complete with handy hook and carrying case |
| ANV03   |    1001 | 2 ton anvil    |      14.99 | 2 ton anvil, black, complete with handy hook and carrying case |
| DTNTR   |    1003 | Detonator      |      13.00 | Detonator (plunger powered), fuses not included                |
| FB      |    1003 | Bird seed      |      10.00 | Large bag (suitable for road runners)                          |
| FC      |    1003 | Carrots        |       2.50 | Carrots (rabbit hunting season only)                           |
| FU1     |    1002 | Fuses          |       3.42 | 1 dozen, extra long                                            |
| JP1000  |    1005 | JetPack 1000   |      35.00 | JetPack 1000, intended for single use                          |
| JP2000  |    1005 | JetPack 2000   |      55.00 | JetPack 2000, multi-use                                        |
| OL1     |    1002 | Oil can        |       8.99 | Oil can, red                                                   |
| SAFE    |    1003 | Safe           |      50.00 | Safe with combination lock                                     |
| SLING   |    1003 | Sling          |       4.49 | Sling, one size fits all                                       |
| TNT1    |    1003 | TNT (1 stick)  |       2.50 | TNT, red, single stick                                         |
| TNT2    |    1003 | TNT (5 sticks) |      10.00 | TNT, red, pack of 10 sticks                                    |
+---------+---------+----------------+------------+----------------------------------------------------------------+
14 rows in set (0.00 sec)

```

**使用通配符** 一般,除非你确实需要表中的每个列,否则最
好别使用 * 通配符。虽然使用通配符可能会使你自己省事,不
用明确列出所需列,但检索不需要的列通常会降低检索和应
用程序的性能。

**检索未知列** 使用通配符有一个大优点。由于不明确指定列名(因为星号检索每个列),所以能检索出名字未知的列。


> 检索不同的行
>> 正如所见, SELECT 返回所有匹配的行。但是,如果你不想要每个值
每次都出现,怎么办?例如,假如你想得出 products 表中产品的所有供
应商ID:

```shell
MariaDB [test]> select vend_id from products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1001 |
|    1001 |
|    1002 |
|    1002 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1003 |
|    1005 |
|    1005 |
+---------+
14 rows in set (0.00 sec)
```

SELECT 语句返回14行(即使表中只有4个供应商),因为 products 表
中列出了14个产品。那么,如何检索出有不同值的列表呢?

**DISTINCT 关键字** 顾名思义,此关键字指示MySQL只返回不同的值。

`SELECT DISTINCT vend_id` 告诉MySQL只返回不同(唯一)的`vend_id `行,因此只返回4行,如下面的输出所示。如果使用`DISTINCT` 关键字,它必须直接放在列名的前面。

```shell
MariaDB [test]> select distinct vend_id from products;
+---------+
| vend_id |
+---------+
|    1001 |
|    1002 |
|    1003 |
|    1005 |
+---------+
4 rows in set (0.01 sec)
```

**不能部分使用 DISTINCT** `DISTINCT` 关键字应用于所有列而不仅是前置它的列。如果给出 `SELECT DISTINCT vend_id,prod_price` ,除非指定的两个列都不同,否则所有行都将被检索出来。

> 限制结果
>> SELECT 语句返回所有匹配的行,它们可能是指定表中的每个行。为了返回第一行或前几行,可使用 `LIMIT` 子句。下面举一个例子:用 SELECT 语句检索单个列返回不多于5行。

```shell
MariaDB [test]> select prod_name from products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products limit 5;
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
| Detonator    |
| Bird seed    |
+--------------+
5 rows in set (0.00 sec)
```

LIMIT 5, 5 指示MySQL返回从行5开始的5行。第一个数为开始
位置,第二个数为要检索的行数。


```shell
MariaDB [test]> select prod_name from products limit 5,5;
+--------------+
| prod_name    |
+--------------+
| Carrots      |
| Fuses        |
| JetPack 1000 |
| JetPack 2000 |
| Oil can      |
+--------------+
5 rows in set (0.00 sec)
```

所以,带一个值的 LIMIT 总是从第一行开始,给出的数为返回的行数。
带两个值的 LIMIT 可以指定从行号为第一个值的位置开始。

**行 0**  检索出来的第一行为行0而不是行1。因此,`LIMIT 1, 1`
将检索出第二行而不是第一行

```shell
MariaDB [test]> select prod_name from products limit 1,1;
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
+-------------+
1 row in set (0.00 sec)

MariaDB [test]> select prod_name from products limit 0,1;
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
+--------------+
1 row in set (0.00 sec)

MariaDB [test]> select prod_name from products limit 1;
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
+--------------+
1 row in set (0.00 sec)
```

**在行数不够时** LIMIT 中指定要检索的行数为检索的最大行
数。如果没有足够的行(例如,给出 `LIMIT 10, 5 `,但只有13
行),MySQL将只返回它能返回的那么多行。

```shell
MariaDB [test]> select prod_name from products limit 5,10;
+----------------+
| prod_name      |
+----------------+
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
9 rows in set (0.00 sec)
```

**MySQL 5的 LIMIT 语法** `LIMIT 3, 4` 的含义是从行4开始的3
行还是从行3开始的4行?如前所述,它的意思是从行3开始的4
行,这容易把人搞糊涂。
由于这个原因, MySQL 5支持 LIMIT 的另一种替代语法。 `LIMIT
4 OFFSET 3` 意为从行3开始取4行,就像 `LIMIT 3, 4` 一样。

```shell
MariaDB [test]> select prod_name from products limit 3,4;
+-----------+
| prod_name |
+-----------+
| Detonator |
| Bird seed |
| Carrots   |
| Fuses     |
+-----------+
4 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products limit 4 offset 3;
+-----------+
| prod_name |
+-----------+
| Detonator |
| Bird seed |
| Carrots   |
| Fuses     |
+-----------+
4 rows in set (0.00 sec)
```

> 使用完全限定的表名
>> 迄今为止使用的SQL例子只通过列名引用列。也可能会使用完全限定
的名字来引用列(同时使用表名和列字)
>> 请看以下例子:通过完全限定的表名和列名查询products表的prod_name列的所有值

```shell
MariaDB [test]> select products.prod_name from products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```

这条SQL语句在功能上等于本章最开始使用的那一条语句,但这里指定了一个完全限定的列名。

表名也可以是完全限定的,如下所示:

```shell
MariaDB [test]> select products.prod_name from test.products;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Detonator      |
| Bird seed      |
| Carrots        |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```

这条语句在功能上也等于刚使用的那条语句。
正如以后章节所介绍的那样,有一些情形需要完全限定名。现在,需要注意这个语法,以便在遇到时知道它的作用。

> 小结
>> 本章学习了如何使用SQL的 SELECT 语句来检索单个表列、多个表列
以及所有表列。下一章将讲授如何排序检索出来的数据。

---

#### 排序检索数据

本章将讲授如何使用 SELECT 语句的 ORDER BY 子句,根据需要排序检
索出的数据。

> 排序数据
>> 利 用 SELECT 语 句 从 products 表 中 检 索 一 个 名 为prod_name 的列。
对 prod_name 列以字母顺序排序。

```shell
MariaDB [test]> select prod_name from products order by prod_name;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Bird seed      |
| Carrots        |
| Detonator      |
| Fuses          |
| JetPack 1000   |
| JetPack 2000   |
| Oil can        |
| Safe           |
| Sling          |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
14 rows in set (0.00 sec)
```

**如果不排序** 数据一般将以它在底层表中出现的顺序显示。这可以是数据最初
添加到表中的顺序。但是,如果数据后来进行过更新或删除,则此顺
序将会受到MySQL重用回收存储空间的影响。因此,如果不明确控
制的话,不能(也不应该)依赖该排序顺序。关系数据库设计理论认
为,如果不明确规定排序顺序,则不应该假定检索出的数据的顺序有
意义。

**子句(clause)** SQL语句由子句构成,有些子句是必需的,而
有的是可选的。一个子句通常由一个关键字和所提供的数据组
成。子句的例子有 SELECT 语句的 FROM 子句,我们在前一章看到过这个子
句。

**通过非选择列进行排序** 通常, ORDER BY 子句中使用的列将
是为显示所选择的列。但是,实际上并不一定要这样,用非
检索的列排序数据是完全合法的。

```shell
MariaDB [test]> select prod_name from products order by vend_id;
+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| Fuses          |
| Oil can        |
| TNT (1 stick)  |
| Sling          |
| Safe           |
| TNT (5 sticks) |
| Carrots        |
| Bird seed      |
| Detonator      |
| JetPack 2000   |
| JetPack 1000   |
+----------------+
14 rows in set (0.00 sec)
```

经常需要按不止一个列进行数据排序。例如,如果要显示雇员清单,
可能希望按姓和名排序(首先按姓排序,然后在每个姓中再按名排序)。
如果多个雇员具有相同的姓,这样做很有用。

为了按多个列排序,只要指定列名,列名之间用逗号分开即可(就
像选择多个列时所做的那样)。

> 按多个列排序
>> 利用SELECT语句从products表中检索3个列(prod_id,prod_name,prod_price)，并按其中两个列对结果进行排序——首先按
价格prod_price,然后再按名称prod_name排序。

```shell
MariaDB [test]> select prod_id,prod_name,prod_price from products order by prod_price,prod_name;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| FC      | Carrots        |       2.50 |
| TNT1    | TNT (1 stick)  |       2.50 |
| FU1     | Fuses          |       3.42 |
| SLING   | Sling          |       4.49 |
| ANV01   | .5 ton anvil   |       5.99 |
| OL1     | Oil can        |       8.99 |
| ANV02   | 1 ton anvil    |       9.99 |
| FB      | Bird seed      |      10.00 |
| TNT2    | TNT (5 sticks) |      10.00 |
| DTNTR   | Detonator      |      13.00 |
| ANV03   | 2 ton anvil    |      14.99 |
| JP1000  | JetPack 1000   |      35.00 |
| SAFE    | Safe           |      50.00 |
| JP2000  | JetPack 2000   |      55.00 |
+---------+----------------+------------+
14 rows in set (0.00 sec)
```

重要的是理解在按多个列排序时,排序完全按所规定的顺序进行。
换句话说,对于上述例子中的输出,仅在多个行具有相同的 prod_price
值时才对产品按 prod_name 进行排序。如果 prod_price 列中所有的值都是
唯一的,则不会按 prod_name 排序。


数据排序不限于升序排序(从 A 到 Z )。这只是默认的排序顺序,还可
以使用 ORDER BY 子句以降序(从 Z 到 A )顺序排序。为了进行降序排序,
必须指定 DESC 关键字。

> 指定排序方向
>> 利用SELECT语句从products表中检索3个列(prod_id,prod_name,prod_price)，按价格prod_price以降序排序产品(最贵的排在最前面)

```shell
MariaDB [test]> select prod_id,prod_name,prod_price from products order by prod_price desc;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| JP2000  | JetPack 2000   |      55.00 |
| SAFE    | Safe           |      50.00 |
| JP1000  | JetPack 1000   |      35.00 |
| ANV03   | 2 ton anvil    |      14.99 |
| DTNTR   | Detonator      |      13.00 |
| TNT2    | TNT (5 sticks) |      10.00 |
| FB      | Bird seed      |      10.00 |
| ANV02   | 1 ton anvil    |       9.99 |
| OL1     | Oil can        |       8.99 |
| ANV01   | .5 ton anvil   |       5.99 |
| SLING   | Sling          |       4.49 |
| FU1     | Fuses          |       3.42 |
| FC      | Carrots        |       2.50 |
| TNT1    | TNT (1 stick)  |       2.50 |
+---------+----------------+------------+
14 rows in set (0.00 sec)
```

但是,如果打算用多个列排序怎么办?

>> 利用SELECT语句从products表中检索3个列(prod_id,prod_name,prod_price)，按价格prod_price以降序排序产品(最贵的排在最前面)，然后再对产品名排序prod_name

```shell
MariaDB [test]> select prod_id,prod_name,prod_price from products order by prod_price desc,prod_name;
+---------+----------------+------------+
| prod_id | prod_name      | prod_price |
+---------+----------------+------------+
| JP2000  | JetPack 2000   |      55.00 |
| SAFE    | Safe           |      50.00 |
| JP1000  | JetPack 1000   |      35.00 |
| ANV03   | 2 ton anvil    |      14.99 |
| DTNTR   | Detonator      |      13.00 |
| FB      | Bird seed      |      10.00 |
| TNT2    | TNT (5 sticks) |      10.00 |
| ANV02   | 1 ton anvil    |       9.99 |
| OL1     | Oil can        |       8.99 |
| ANV01   | .5 ton anvil   |       5.99 |
| SLING   | Sling          |       4.49 |
| FU1     | Fuses          |       3.42 |
| FC      | Carrots        |       2.50 |
| TNT1    | TNT (1 stick)  |       2.50 |
+---------+----------------+------------+
14 rows in set (0.00 sec)
```

**在多个列上降序排序** 如果想在多个列上进行降序排序,必须
对每个列指定 DESC 关键字。与 DESC 相反的关键字是 ASC ( ASCENDING )
,在升序排序时可以指定它。但实际上, ASC 没有多大用处,因为升序是默认的(如果既不指定 ASC 也
不指定 DESC ,则假定为 ASC )。

**区分大小写和排序顺序** 在对文本性的数据进行排序时,A与
a相同吗?a位于B之前还是位于Z之后?这些问题不是理论问
题,其答案取决于数据库如何设置。在字典(dictionary)排序顺序中, A被视为与a相同,这是MySQL
(和大多数数据库管理系统)的默认行为。但是,许多数据库
管理员能够在需要时改变这种行为(如果你的数据库包含大量
外语字符,可能必须这样做)。这里,关键的问题是,如果确实需要改变这种排序顺序,用简
单的 ORDER BY 子句做不到。你必须请求数据库管理员的帮助。

>> 利用SELECT语句从products表中检索出最昂贵物品价格prod_price

```shell
MariaDB [test]> select prod_price from products order by prod_price desc limit 1;
+------------+
| prod_price |
+------------+
|      55.00 |
+------------+
1 row in set (0.00 sec)

MariaDB [test]> select prod_price from products order by prod_price desc limit 0,1;
+------------+
| prod_price |
+------------+
|      55.00 |
+------------+
1 row in set (0.00 sec)

MariaDB [test]> select prod_price from products order by prod_price desc limit 1 offset 0;
+------------+
| prod_price |
+------------+
|      55.00 |
+------------+
1 row in set (0.00 sec)
```

**ORDER BY 子句的位置** 在给出 ORDER BY 子句时,应该保证它
位于 FROM 子句之后。如果使用 LIMIT ,它必须位于 ORDER BY
之后。使用子句的次序不对将产生错误消息。

```shell
MariaDB [test]> select prod_price from products limit 1 order by prod_price desc;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'order by prod_price desc' at line 1
```

> 小结
>> 本章学习了如何用 SELECT 语句的 ORDER BY 子句对检索出的数据进行
排序。这个子句必须位于 FROM 子句之后,必须是 SELECT 语句中的最后一条子句。可根据需要,利
用它在一个或多个列上对数据进行排序。

---

#### 过滤数据

本章将讲授如何使用 SELECT 语句的 WHERE 子句指定搜索条件。

> 使用 WHERE 子句

数据库表一般包含大量的数据,很少需要检索表中所有行。通常只
会根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要
指 定 搜 索 条 件 ( search criteria ) , 搜 索 条 件 也 称 为 过 滤 条 件 ( filter
condition)。

在 SELECT 语句中,数据根据 WHERE 子句中指定的搜索条件进行过滤。
WHERE 子句在表名( FROM 子句)之后给出。

>> 从 products 表中检索两个列(prod_name,prod_price),但不返回所有行,只返
回 prod_price 值为 2.50 的行。

```shell
MariaDB [test]> select prod_name,prod_price  from products where prod_price=2.50;
+---------------+------------+
| prod_name     | prod_price |
+---------------+------------+
| Carrots       |       2.50 |
| TNT (1 stick) |       2.50 |
+---------------+------------+
2 rows in set (0.00 sec)
```

**SQL过滤与应用过滤** 数据也可以在应用层过滤。为此目
的,SQL的 SELECT 语句为客户机应用检索出超过实际所需的
数据,然后客户机代码对返回数据进行循环,以提取出需要
的行。
通常,这种实现并不令人满意。因此,对数据库进行了优化,
以便快速有效地对数据进行过滤。让客户机应用(或开发语言)
处理数据库的工作将会极大地影响应用的性能,并且使所创建
的应用完全不具备可伸缩性。此外,如果在客户机上过滤数据,
服务器不得不通过网络发送多余的数据,这将导致网络带宽的
浪费。

**WHERE 子句的位置** 在同时使用 ORDER BY 和 WHERE 子句时,应
该让 ORDER BY 位于 WHERE 之后,否则将会产生错误。

```shell
MariaDB [test]> select prod_name,prod_price  from products where prod_name like "T%" order by prod_price desc;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| TNT (5 sticks) |      10.00 |
| TNT (1 stick)  |       2.50 |
+----------------+------------+
2 rows in set (0.01 sec)

MariaDB [test]> select prod_name,prod_price  from products order by prod_price desc where prod_name like "T%";
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'where prod_name like "T%"' at line 1
```

> WHERE 子句操作符

我们在关于相等的测试时看到了第一个 WHERE 子句,它确定一个列是否包含特定的值。MySQL支持下表列出的所有条件操作符。

|操作符|说明|
|:--|:-|
|=|等于|
|<>|不等于|
|!=|不等于|
|<|小于|
|<=|小于等于|
|>|大于|
|>=|大于等于|
|between|在指定的两个值之间|


1.检查单个值

>> 从 products 表中检索两个列(prod_name,prod_price),但不返回所有行,只返回 prod_name 的值为 Fuses 的一行。

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_name='fuses';
+-----------+------------+
| prod_name | prod_price |
+-----------+------------+
| Fuses     |       3.42 |
+-----------+------------+
1 row in set (0.00 sec)
```

检查 `WHERE prod_name='fuses'` 语句,它返回 prod_name 的值
为 Fuses 的一行。MySQL在执行匹配时默认不区分大小写,所以 fuses 与 Fuses 匹配。

>> 列出价格小于10美元的所有产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_price < 10;
+---------------+------------+
| prod_name     | prod_price |
+---------------+------------+
| .5 ton anvil  |       5.99 |
| 1 ton anvil   |       9.99 |
| Carrots       |       2.50 |
| Fuses         |       3.42 |
| Oil can       |       8.99 |
| Sling         |       4.49 |
| TNT (1 stick) |       2.50 |
+---------------+------------+
7 rows in set (0.00 sec)
```

>> 检索价格小于等于10美元的所有产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_price <= 10;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| 1 ton anvil    |       9.99 |
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Fuses          |       3.42 |
| Oil can        |       8.99 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
9 rows in set (0.00 sec)
```

>> 实践思考

```shell
 MariaDB [test]> select prod_name,prod_price from products where prod_name <= "b";
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
+--------------+------------+
3 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price from products where prod_name <= b;
ERROR 1054 (42S22): Unknown column 'b' in 'where clause'
MariaDB [test]> select prod_name,prod_price from products where prod_name < "F";
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
| Detonator    |      13.00 |
| Bird seed    |      10.00 |
| Carrots      |       2.50 |
+--------------+------------+
6 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price from products where prod_name < "f";
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
| Detonator    |      13.00 |
| Bird seed    |      10.00 |
| Carrots      |       2.50 |
+--------------+------------+
6 rows in set (0.00 sec)
```

**何时使用引号** 如果仔细观察上述 WHERE 子句中使用的条件,
会看到有的值括在单引号内(如前面使用的 'fuses' ),而有
的值未括起来。单引号用来限定字符串。如果将值与串类型的
列进行比较,则需要限定引号。用来与数值列进行比较的值不
用引号。

2.不匹配检查

>> 不是由供应商 1003 制造的所有产品

```shell
MariaDB [test]> select vend_id,prod_name from products where vend_id <> 1003;
+---------+--------------+
| vend_id | prod_name    |
+---------+--------------+
|    1001 | .5 ton anvil |
|    1001 | 1 ton anvil  |
|    1001 | 2 ton anvil  |
|    1002 | Fuses        |
|    1005 | JetPack 1000 |
|    1005 | JetPack 2000 |
|    1002 | Oil can      |
+---------+--------------+
7 rows in set (0.00 sec)

MariaDB [test]> select vend_id,prod_name from products where vend_id != 1003;
+---------+--------------+
| vend_id | prod_name    |
+---------+--------------+
|    1001 | .5 ton anvil |
|    1001 | 1 ton anvil  |
|    1001 | 2 ton anvil  |
|    1002 | Fuses        |
|    1005 | JetPack 1000 |
|    1005 | JetPack 2000 |
|    1002 | Oil can      |
+---------+--------------+
7 rows in set (0.00 sec)
```

3.范围值检查

为了检查某个范围的值,可使用 BETWEEN 操作符。其语法与其他 WHERE 子句的操作符稍有不同,因为它需要两个值,即范围的开始值和结束值。
例如, BETWEEN 操作符可用来检索价格在5美元和10美元之间或日期在指定的开始日期和结束日期之间的所有产品。

>> 检索价格在5美元和10美元之间的所有产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_price between 5 and 10;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| 1 ton anvil    |       9.99 |
| Bird seed      |      10.00 |
| Oil can        |       8.99 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
5 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price from products where prod_price between 5 and 10 order by prod_price;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| .5 ton anvil   |       5.99 |
| Oil can        |       8.99 |
| 1 ton anvil    |       9.99 |
| Bird seed      |      10.00 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
5 rows in set (0.00 sec)
```

4.空值检查

在创建表时,表设计人员可以指定其中的列是否可以不包含值。在
一个列不包含值时,称其为包含空值 NULL 。

**NULL 无值(no value)** 它与字段包含 0 、空字符串或仅仅包含空格不同。

>> 检查products表中prod_price列具有 NULL 值的行

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_price is null;
Empty set (0.00 sec)
```

>> 检查customers表中cust_email列具有 NULL 值的行

```shell
MariaDB [test]> select cust_name,cust_email from customers where cust_email is null;
+-------------+------------+
| cust_name   | cust_email |
+-------------+------------+
| Mouse House | NULL       |
| E Fudd      | NULL       |
+-------------+------------+
2 rows in set (0.00 sec)
```

>> 检查customers表中cust_email列不具有 NULL 值的行

```shell
MariaDB [test]> select cust_name,cust_email from customers where cust_email is not null;
+----------------+---------------------+
| cust_name      | cust_email          |
+----------------+---------------------+
| Coyote Inc.    | ylee@coyote.com     |
| Wascals        | rabbit@wascally.com |
| Yosemite Place | sam@yosemite.com    |
+----------------+---------------------+
3 rows in set (0.00 sec)
```

> 小结
>> 本章介绍了如何用 SELECT 语句的 WHERE 子句过滤返回的数据。我们学
习了如何对相等、不相等、大于、小于、值的范围以及 NULL 值等进行测
试。

---

#### 数据过滤

本章讲授如何组合 WHERE 子句以建立功能更强的更高级的搜索条件。
我们还将学习如何使用 NOT 和 IN 操作符。

前面章节中介绍的所有 WHERE 子句在过滤数据时使用的都是单一的条件。
为了进行更强的过滤控制,MySQL允许给出多个 WHERE 子句。这些子
句可以两种方式使用:以 AND 子句的方式或 OR 子句的方式使用。

**操作符(operator)** 用来联结或改变 WHERE 子句中的子句的关键字。也称为逻辑操作符(logical operator)。

> 组合 WHERE 子句

1. AND 操作符

`AND` 用在 WHERE 子句中的关键字,用来指示检索满足所有给定条件的行。

>> 检索products表由供应商 1003 制造且价格小于等于10美元的所有产品的名称和价格。

```shell
MariaDB [test]> select prod_name,prod_price from products where vend_id=1003 and prod_price <= 10;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
5 rows in set (0.00 sec)
```

2. OR 操作符

`OR` 操作符与 AND 操作符不同,它指示MySQL检索匹配任一条件的行。

>> 检索products表由供应商 1002或1003 制造的所有产品的名称和价格。

```shell
MariaDB [test]> select prod_name,prod_price from products where vend_id=1002 or vend_id=1003;
+----------------+------------+
| prod_name      | prod_price |
+----------------+------------+
| Detonator      |      13.00 |
| Bird seed      |      10.00 |
| Carrots        |       2.50 |
| Fuses          |       3.42 |
| Oil can        |       8.99 |
| Safe           |      50.00 |
| Sling          |       4.49 |
| TNT (1 stick)  |       2.50 |
| TNT (5 sticks) |      10.00 |
+----------------+------------+
9 rows in set (0.00 sec)
```

3. 计算次序

WHERE 可包含任意数目的 AND 和 OR 操作符。允许两者结合以进行复杂和高级的过滤。

>> 检索products表，列出价格为10美元(含)以上且由 1002 或 1003 制造的所有产品。

```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id=1002 or vend_id=1003 and prod_price >=10;
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Detonator      |      13.00 |    1003 |
| Bird seed      |      10.00 |    1003 |
| Fuses          |       3.42 |    1002 |
| Oil can        |       8.99 |    1002 |
| Safe           |      50.00 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
+----------------+------------+---------+
6 rows in set (0.00 sec)
```

请看上面的结果。返回的行中有两行价格小于10美元,显然,返回的行未按预期的进行过滤。
为什么会这样呢?原因在于计算的次序。SQL(像多数语言一样)在处理 OR 操作符前,优先处理 AND 操作符。
当SQL看到上述 WHERE 子句时,它理解为由供应商 1003 制造的任何价格为10美元(含)以上的产品,或者由供应商 1002 制造的任何产品,
而不管其价格如何。换句话说,由于 AND 在计算次序中优先级更高,操作符被错误地组合了。

此问题的解决方法是使用圆括号明确地分组相应的操作符。请看下面的 SELECT 语句及输出:

```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where (vend_id=1002 or vend_id=1003) and prod_price >=10;
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Detonator      |      13.00 |    1003 |
| Bird seed      |      10.00 |    1003 |
| Safe           |      50.00 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
+----------------+------------+---------+
4 rows in set (0.00 sec)
```

其他方法：
```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where prod_price >= 10 and vend_id=1002 or prod_price >=10 and vend_id=1003 ;
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Detonator      |      13.00 |    1003 |
| Bird seed      |      10.00 |    1003 |
| Safe           |      50.00 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
+----------------+------------+---------+
4 rows in set (0.00 sec)
```

**在WHERE子句中使用圆括号** 任何时候使用具有 AND 和 OR 操作符的 WHERE 子句,都应该使用圆括号明确地分组操作符。
不要过分依赖默认计算次序,即使它确实是你想要的东西也是如此。使用圆括号没有什么坏处,它能消除歧义。

> IN 操作符

圆括号在 WHERE 子句中还有另外一种用法。 IN 操作符用来指定条件范
围,范围中的每个条件都可以进行匹配。 IN 取合法值的由逗号分隔的清
单,全都括在圆括号中。

>> 检索products表，列出由 1002 或 1003 制造的所有产品。

```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id in (1002,1003);
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Detonator      |      13.00 |    1003 |
| Bird seed      |      10.00 |    1003 |
| Carrots        |       2.50 |    1003 |
| Fuses          |       3.42 |    1002 |
| Oil can        |       8.99 |    1002 |
| Safe           |      50.00 |    1003 |
| Sling          |       4.49 |    1003 |
| TNT (1 stick)  |       2.50 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
+----------------+------------+---------+
9 rows in set (0.00 sec)
```

`IN` WHERE 子句中用来指定要匹配值的清单的关键字,功能与 OR相当 。

IN 操作符与 OR 相同的功能，下面一些实例完成之前的一些操作：

```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id in (1002,1003) and prod_price >= 10;
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Detonator      |      13.00 |    1003 |
| Bird seed      |      10.00 |    1003 |
| Safe           |      50.00 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
+----------------+------------+---------+
4 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id in (1002,1003) order by prod_price;
+----------------+------------+---------+
| prod_name      | prod_price | vend_id |
+----------------+------------+---------+
| Carrots        |       2.50 |    1003 |
| TNT (1 stick)  |       2.50 |    1003 |
| Fuses          |       3.42 |    1002 |
| Sling          |       4.49 |    1003 |
| Oil can        |       8.99 |    1002 |
| Bird seed      |      10.00 |    1003 |
| TNT (5 sticks) |      10.00 |    1003 |
| Detonator      |      13.00 |    1003 |
| Safe           |      50.00 |    1003 |
+----------------+------------+---------+
9 rows in set (0.00 sec)
```

**为什么要使用 IN 操作符?** 其优点具体如下。
* 在使用长的合法选项清单时, IN 操作符的语法更清楚且更直观。
* 在使用 IN 时,计算的次序更容易管理(因为使用的操作符更少)。
* IN 操作符一般比 OR 操作符清单执行更快。
* IN 的最大优点是可以包含其他 SELECT 语句,使得能够更动态地建
立 WHERE 子句。之后章节将对此进行详细介绍。

> NOT 操作符

WHERE 子句中的 NOT 操作符有且只有一个功能,那就是否定它之后所跟的任何条件。

`NOT` WHERE 子句中用来否定后跟条件的关键字。

>> 列出除 1002和1003 之外的所有供应商制造的产品

```shell
MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id not in (1002,1003);
+--------------+------------+---------+
| prod_name    | prod_price | vend_id |
+--------------+------------+---------+
| .5 ton anvil |       5.99 |    1001 |
| 1 ton anvil  |       9.99 |    1001 |
| 2 ton anvil  |      14.99 |    1001 |
| JetPack 1000 |      35.00 |    1005 |
| JetPack 2000 |      55.00 |    1005 |
+--------------+------------+---------+
5 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id != 1002 and vend_id != 1003;
+--------------+------------+---------+
| prod_name    | prod_price | vend_id |
+--------------+------------+---------+
| .5 ton anvil |       5.99 |    1001 |
| 1 ton anvil  |       9.99 |    1001 |
| 2 ton anvil  |      14.99 |    1001 |
| JetPack 1000 |      35.00 |    1005 |
| JetPack 2000 |      55.00 |    1005 |
+--------------+------------+---------+
5 rows in set (0.00 sec)

MariaDB [test]> select prod_name,prod_price,vend_id from products where vend_id <> 1002 and vend_id <> 1003;
+--------------+------------+---------+
| prod_name    | prod_price | vend_id |
+--------------+------------+---------+
| .5 ton anvil |       5.99 |    1001 |
| 1 ton anvil  |       9.99 |    1001 |
| 2 ton anvil  |      14.99 |    1001 |
| JetPack 1000 |      35.00 |    1005 |
| JetPack 2000 |      55.00 |    1005 |
+--------------+------------+---------+
5 rows in set (0.00 sec)
```
**为什么使用 NOT ?** 对于简单的 WHERE 子句,使用 NOT 确实没有什么优势。但在更复杂的子句中, NOT 是非常有用的。
例如,在与 IN 操作符联合使用时, NOT 使找出与条件列表不匹配的行非常简单。


**MySQL 中的NOT** MySQL 支 持 使 用 NOT 对 IN 、 BETWEEN 和EXISTS子句取反,这与多数其他 DBMS允许使用 NOT 对各种条件取反有很大的差别。

> 小结
>> 本章讲授如何用 AND 和 OR 操作符组合成 WHERE 子句,而且还讲授了如何明确地管理计算的次序,如何使用 IN 和 NOT 操作符。

---

#### 用通配符进行过滤

本章介绍什么是通配符、如何使用通配符以及怎样使用 LIKE 操作符进行通配搜索,以便对数据进行复杂过滤。

> LIKE 操作符

前面介绍的所有操作符都是针对已知值进行过滤的。不管是匹配一
个还是多个值,测试大于还是小于已知值,或者检查某个范围的值,共
同点是过滤中使用的值都是已知的。但是,这种过滤方法并不是任何时
候都好用。例如,怎样搜索产品名中包含文本anvil的所有产品?用简单
的比较操作符肯定不行,必须使用通配符。利用通配符可创建比较特定
数据的搜索模式。

**通配符(wildcard)** 用来匹配值的一部分的特殊字符。

**搜索模式(search pattern)** 由字面值、通配符或两者组合构成的搜索条件。

通配符本身实际是SQL的 WHERE 子句中有特殊含义的字符,SQL支持几种通配符(%,_)。

为在搜索子句中使用通配符,必须使用 LIKE 操作符。 LIKE 指示MySQL,后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较。

**谓词** 操作符何时不是操作符?答案是在它作为谓词(predi-cate)时。从技术上说, LIKE 是谓词而不是操作符。
虽然最终的结果是相同的,但应该对此术语有所了解,以免在SQL文档中遇到此术语时不知道。

1.百分号( % )通配符

最常使用的通配符是百分号( % )。在搜索串中, % 表示任何字符出现任意次数。

>> 找出所有以词 jet 起头的产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_name like "jet%" ;
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| JetPack 1000 |      35.00 |
| JetPack 2000 |      55.00 |
+--------------+------------+
2 rows in set (0.00 sec)
```

通配符可在搜索模式中任意位置使用,并且可以使用多个通配符。

>> 找出所有包含anvil的产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_name like "%anvil%" ;
+--------------+------------+
| prod_name    | prod_price |
+--------------+------------+
| .5 ton anvil |       5.99 |
| 1 ton anvil  |       9.99 |
| 2 ton anvil  |      14.99 |
+--------------+------------+
3 rows in set (0.00 sec)
```
通配符也可以出现在搜索模式的中间,虽然这样做不太有用。

>> 找出以 s 起头以 e 结尾的所有产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_name like "s%e" ;
+-----------+------------+
| prod_name | prod_price |
+-----------+------------+
| Safe      |      50.00 |
+-----------+------------+
1 row in set (0.00 sec)
```

重要的是要注意到,除了一个或多个字符外, % 还能匹配0个字符。 %代表搜索模式中给定位置的0个、1个或多个字符。

**注意尾空格** 尾空格可能会干扰通配符匹配。例如,在保存词
anvil 时 , 如 果 它 后 面 有 一 个 或 多 个 空 格 , 则 子 句 WHERE
prod_name LIKE '%anvil' 将不会匹配它们,因为在最后的 l
后有多余的字符。解决这个问题的一个简单的办法是在搜索模
式最后附加一个 % 。一个更好的办法是使用函数(之后讲解将会
介绍)去掉首尾空格。

**注意NULL** 虽然似乎 % 通配符可以匹配任何东西,但有一个例
外,即 NULL 。即使是 WHERE prod_name LIKE '%' 也不能匹配
用值 NULL 作为产品名的行。

2.下划线(_)通配符

另一个有用的通配符是下划线( _ )。下划线的用途与 % 一样,但下划线只匹配单个字符而不是多个字符。

>> 找出以 开头有一位，之后是空格，然后为ton anvil的所有产品

```shell
MariaDB [test]> select prod_name,prod_price from products where prod_name like "_ ton anvil" ;
+-------------+------------+
| prod_name   | prod_price |
+-------------+------------+
| 1 ton anvil |       9.99 |
| 2 ton anvil |      14.99 |
+-------------+------------+
2 rows in set (0.00 sec)
```

> 使用通配符的技巧

正如所见,MySQL的通配符很有用。但这种功能是有代价的:通配符搜索的处理一般要比前面讨论的其他搜索所花时间更长。
这里给出一些使用通配符要记住的技巧。

* 不要过度使用通配符。如果其他操作符能达到相同的目的,应该使用其他操作符。
* 在确实需要使用通配符时,除非绝对有必要,否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处,搜索起
来是最慢的。
* 仔细注意通配符的位置。如果放错地方,可能不会返回想要的数据。

总之,通配符是一种极重要和有用的搜索工具,以后我们经常会用到它。

> 小结
>> 本章介绍了什么是通配符以及如何在 WHERE 子句中使用SQL通配符,并且还说明了通配符应该细心使用,不要过度使用。

---

#### 用正则表达式进行搜索

本章将学习如何在MySQL WHERE 子句内使用正则表达式来更好地控制数据过滤。

> 正则表达式介绍

前两章中的过滤例子允许用匹配、比较和通配操作符寻找数据。对于基本的过滤(或者甚至是某些不那么基本的过滤),这样就足够了。
但随着过滤条件的复杂性的增加, WHERE 子句本身的复杂性也有必要增加。
这也就是正则表达式变得有用的地方。正则表达式是用来匹配文本
的特殊的串(字符集合)。

如果你想从一个文本文件中提取电话号码,可以使用正则表达式。
如果你需要查找名字中间有数字的所有文件,可以使用一个正则表达式。
如果你想在一个文本块中找到所有重复的单词,可以使用一个正则表达式。
如果你想替换一个页面中的所有URL为这些URL的实际HTML链接,也可以使用一个正则表达式(对于最后这个例子,
或者是两个正则表达式)。

所有种类的程序设计语言、文本编辑器、操作系统等都支持正则表达式。
有见识的程序员和网络管理员已经关注作为他们技术工具重要内容的正则表达式很长时间了。
正则表达式用正则表达式语言来建立,正则表达式语言是用来完成刚讨论的所有工作以及更多工作的一种特殊语言。
与任意语言一样,正则表达式具有你必须学习的特殊的语法和指令。

完全覆盖正则表达式的内容超出了MySQL数据库教学的范围。虽然基础知识都在这里做了介绍,但对正则表达式更为透
彻的介绍我们在前面shell课程中有。此处不再赘述。

> 使用MySQL正则表达式

那么,正则表达式与MySQL有何关系?已经说过,正则表达式的作用是匹配文本,将一个模式(正则表达式)与一个文本串进行比较。
MySQL用 WHERE 子句对正则表达式提供了初步的支持,允许你指定正则表达式,过滤 SELECT 检索出的数据。

**仅为正则表达式语言的一个子集** 如果你熟悉正则表达式,需要注意:MySQL仅支持多数正则表达式实现的一个很小的子集。本章介绍MySQL支持的大多数内容。

1.基本字符匹配

>> 产品名称中匹配1000的

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '1000';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
+--------------+
1 row in set (0.00 sec)
```

>> 产品名称中匹配000的

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '.000';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name like '%000%';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

LIKE 匹配整个列。如果被匹配的文本在列值中出现, LIKE 将不会找到它,相应的行也不被返回(除非使用通配符)。
而 REGEXP 在列值内进行匹配,如果被匹配的文本在列值中出现, REGEXP 将会找到它,相应的行将被返回。
这是一个非常重要的差别。

那么, REGEXP 能不能用来匹配整个列值(从而起与 LIKE 相同的作用)?
答案是肯定的,使用 ^ 和 $ 定位符(anchor)即可

**匹配不区分大小写** MySQL中的正则表达式匹配(自版本3.23.4后)不区分大小写(即,大写和小写都匹配)。
为区分大小写,可使用 BINARY 关键字,如 `WHERE prod_name REGEXP BINARY 'JetPack .000'` 。

```shell
MariaDB [test]> select prod_name from products where prod_name regexp 'jetpack';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name regexp binary 'jetpack';
Empty set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name regexp binary 'JetPack';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

2.进行OR匹配

为搜索两个串之一(或者为这个串,或者为另一个串),使用 |

>> 产品名称中匹配1000或者2000的

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '1000|2000';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

>> 产品名称中匹配 1000 或 2000 或 3000

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '1000|2000|3000';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

**两个以上的 OR 条件** 可以给出两个以上的 OR 条件。

3.匹配几个字符之一

匹配任何单一字符。但是,如果你只想匹配特定的字符,怎么办?可通过指定一组用 [ 和 ] 括起来的字符来完成

>> 产品名称中匹配 1 或 2 或 3后面空格ton

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '[123] ton';
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
| 2 ton anvil |
+-------------+
2 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name regexp '[1|2|3] ton';
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
| 2 ton anvil |
+-------------+
2 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name regexp '[1,2,3] ton';
+-------------+
| prod_name   |
+-------------+
| 1 ton anvil |
| 2 ton anvil |
+-------------+
2 rows in set (0.00 sec)
```

4.匹配范围

集合可用来定义要匹配的一个或多个字符。例如,下面的集合将匹配数字0到9:[0123456789]
为简化这种类型的集合,可使用 - 来定义一个范围。
下面的式子功能上等同于上述数字列表:
范围不限于完整的集合, [1-3] 和 [6-9] 也是合法的范围。
此外,范围不一定只是数值的, [a-z] 匹配任意字母字符。

>> 匹配 1 到 5 后面空格接ton

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '[1-5] ton';
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
+--------------+
3 rows in set (0.00 sec)
```

5.匹配特殊字符

正则表达式语言由具有特定含义的特殊字符构成。
我们已经看到 . 、[] 、| 和 - 等,还有其他一些字符。请问,如果你需要匹配这些字符,应该怎么办呢?

例如,如果要找出包含 . 字符的值,怎样搜索?

```shell
MariaDB [test]> select vend_name from vendors where vend_name regexp '.';
+----------------+
| vend_name      |
+----------------+
| Anvils R Us    |
| LT Supplies    |
| ACME           |
| Furball Inc.   |
| Jet Set        |
| Jouets Et Ours |
+----------------+
6 rows in set (0.00 sec)

MariaDB [test]> select vend_name from vendors where vend_name regexp '\.';
+----------------+
| vend_name      |
+----------------+
| Anvils R Us    |
| LT Supplies    |
| ACME           |
| Furball Inc.   |
| Jet Set        |
| Jouets Et Ours |
+----------------+
6 rows in set (0.00 sec)

MariaDB [test]> select vend_name from vendors where vend_name regexp '\\.';
+--------------+
| vend_name    |
+--------------+
| Furball Inc. |
+--------------+
1 row in set (0.00 sec)
```

这才是期望的输出。 `\\.` 匹配 `.` ,所以只检索出一行。这种处理就是所谓的 **转义(escaping)**,
正则表达式内具有特殊意义的所有字符都必须以这种方式转义。
这包括 . 、 | 、 [] 以及迄今为止使用过的其他特殊字符。

为了匹配特殊字符,必须用 `\\` 为前导。 `\\-` 表示查找 `-` , `\\.` 表示查找 `.` 。

匹配 `\` 为了匹配反斜杠(` \ `)字符本身,需要使用 `\\\` 。

`\\ `也用来引用元字符(具有特殊含义的字符),如下表所列。

|元 字 符|说 明|
|:--|:--|
|\\f| 换页|
|\\n| 换行|
|\\r| 回车|
|\\t| 制表|
|\\v| 纵向制表|

**`\` 或 `\\`?** 多数正则表达式实现使用单个反斜杠转义特殊字符,以便能使用这些字符本身。
但MySQL要求两个反斜杠(MySQL自己解释一个,正则表达式库解释另一个)。

6.匹配字符类

存在找出你自己经常使用的数字、所有字母字符或所有数字字母字符等的匹配。
为更方便工作,可以使用预定义的字符集,称为 **字符类(character class)**
下表列出字符类以及它们的含义：

|类|说 明|
|:-|:-|
|[:alnum:]|任意字母和数字(同[a-zA-Z0-9])|
|[:alpha:]|任意字符(同[a-zA-Z])|
|[:blank:]|空格和制表(同[\\t])|
|[:cntrl:]|ASCII控制字符(ASCII 0到31和127)|
|[:digit:]|任意数字(同[0-9])|
|[:graph:]|与[:print:]相同,但不包括空格|
|[:lower:]|任意小写字母(同[a-z])|
|[:print:]|任意可打印字符|
|[:punct:]|既不在[:alnum:]又不在[:cntrl:]中的任意字符|
|[:space:]|包括空格在内的任意空白字符(同[\\f\\n\\r\\t\\v])|
|[:upper:]|任意大写字母(同[A-Z])|
|[:xdigit:]|任意十六进制数字(同[a-fA-F0-9]|

7.匹配多个实例

目前为止使用的所有正则表达式都试图匹配单次出现。
如果存在一个匹配,该行被检索出来,如果不存在,检索不出任何行。
但有时需要对匹配的数目进行更强的控制。
例如,你可能需要寻找所有的数,不管数中包含多少数字,或者你可能想寻找一个单词并且还能够适应一个尾
随的 s (如果存在),等等。

这可以用下表列出的正则表达式重复元字符来完成。

|重复元字符|说明|
|:--|:-|
|*|0个或多个匹配|
|+|1个或多个匹配(等于{1,})|
|?|0个或1个匹配(等于{0,1})|
|{n}|指定数目的匹配|
|{n,}|不少于指定数目的匹配|
|{n,m}|匹配数目的范围(m不超过255)|

>> prod_name列中匹配包括括号，并且括号中的内容依次为 一位数字，一个空格，stick，s有或者没有

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '\\([0-9] sticks?\\)';
+----------------+
| prod_name      |
+----------------+
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
2 rows in set (0.00 sec)
```

>> prod_name列中匹配4个数字

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '[[:digit:]]{4}';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec）
```

需要注意的是,在使用正则表达式时,编写某个特殊的表达式几乎总是有不止一种方法。上面的例子也可以如下编写:

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '[0-9][0-9][0-9][0-9]';
+--------------+
| prod_name    |
+--------------+
| JetPack 1000 |
| JetPack 2000 |
+--------------+
2 rows in set (0.00 sec)
```

8.定位符

目前为止的所有例子都是匹配一个串中任意位置的文本。为了匹配特定位置的文本,需要使用下表列出的定位符。
|定位元字符|说 明|
|:--|:--|
|^| 文本的开始|
|$| 文本的结尾|
|[[:<:]]| 词的开始|
|[[:>:]]| 词的结尾|

>> 找出以一个数(包括以小数点开始的数)开始的所有产品

```shell
MariaDB [test]> select prod_name from products where prod_name regexp '[0-9\\.]';+----------------+
| prod_name      |
+----------------+
| .5 ton anvil   |
| 1 ton anvil    |
| 2 ton anvil    |
| JetPack 1000   |
| JetPack 2000   |
| TNT (1 stick)  |
| TNT (5 sticks) |
+----------------+
7 rows in set (0.00 sec)

MariaDB [test]> select prod_name from products where prod_name regexp '^[0-9\\.]';
+--------------+
| prod_name    |
+--------------+
| .5 ton anvil |
| 1 ton anvil  |
| 2 ton anvil  |
+--------------+
3 rows in set (0.00 sec)
```

简单搜索 [0-9\\.] (或 [[:digit:]\\.] )不行,因为
它将在文本内任意位置查找匹配。解决办法是使用 ^ 定位符

**`^` 的双重用途** `^` 有两种用法。在集合中(用 [ 和 ] 定义),用它来否定该集合,否则,用来指串的开始处。

**使 REGEXP 起类似 LIKE 的作用** 本章前面说过, LIKE 和 REGEXP的不同在于, LIKE 匹配整个串而 REGEXP 匹配子串。
利用定位符,通过用 ^ 开始每个表达式,用 $ 结束每个表达式,可以使REGEXP 的作用与 LIKE 一样。

**简单的正则表达式测试** 可以在不使用数据库表的情况下用SELECT 来测试正则表达式。
REGEXP 检查总是返回 0 (没有匹配)或 1 (匹配)。可以用带文字串的 REGEXP 来测试表达式,并试
验它们。相应的语法如下:

>> 测试文本 hello 中是否能匹配到数字

```shell
MariaDB [test]> select 'hello' regexp '[0-9]';
+------------------------+
| 'hello' regexp '[0-9]' |
+------------------------+
|                      0 |
+------------------------+
1 row in set (0.00 sec)
```

>> 测试文本中是否能匹配到制定数量的o

```shell
MariaDB [test]> select 'booboo' regexp '[o]{3,}';
+---------------------------+
| 'booboo' regexp '[o]{3,}' |
+---------------------------+
|                         0 |
+---------------------------+
1 row in set (0.00 sec)

MariaDB [test]> select 'boobooo' regexp '[o]{3,}';
+----------------------------+
| 'boobooo' regexp '[o]{3,}' |
+----------------------------+
|                          1 |
+----------------------------+
1 row in set (0.00 sec)
```

> 小结
>> 本章介绍了正则表达式的基础知识,学习了如何在MySQL的 SELECT语句中通过 REGEXP 关键字使用它们。

---

#### 创建计算字段

本章介绍什么是计算字段,如何创建计算字段以及怎样从应用程序
中使用别名引用它们。

> 计算字段

存储在数据库表中的数据一般不是应用程序所需要的格式。下面举
几个例子。
* 如果想在一个字段中既显示公司名,又显示公司的地址,但这两
个信息一般包含在不同的表列中。
* 城市、州和邮政编码存储在不同的列中(应该这样),但邮件标签
打印程序却需要把它们作为一个恰当格式的字段检索出来。
* 列数据是大小写混合的,但报表程序需要把所有数据按大写表示
出来。
* 物品订单表存储物品的价格和数量,但不需要存储每个物品的总
价格(用价格乘以数量即可)。为打印发票,需要物品的总价格。
* 需要根据表数据进行总数、平均数计算或其他计算。

在上述每个例子中,存储在表中的数据都不是应用程序所需要的。
我们需要直接从数据库中检索出转换、计算或格式化过的数据;而不是
检索出数据,然后再在客户机应用程序或报告程序中重新格式化。
这就是计算字段发挥作用的所在了。与前面各章介绍过的列不同,
计算字段并不实际存在于数据库表中。计算字段是运行时在 SELECT 语句
内创建的。

**字段(field)** 基本上与列(column)的意思相同,经常互换使
用,不过数据库列一般称为列,而术语字段通常用在计算字段的连接上。
重要的是要注意到,只有数据库知道 SELECT 语句中哪些列是实际的
表列,哪些列是计算字段。从客户机(如应用程序)的角度来看,计算
字段的数据是以与其他列的数据相同的方式返回的。

**客户机与服务器的格式** 可在SQL语句内完成的许多转换
和格式化工作都可以直接在客户机应用程序内完成。但一
般来说,在数据库服务器上完成这些操作比在客户机中完
成要快得多,因为DBMS是设计来快速有效地完成这种处理的。

> 拼接字段

为了说明如何使用计算字段,举一个创建由两列组成的标题的简单例子。

>> `vendors` 表包含供应商名和位置信息。假如要生成一个供应商报表,
需要在供应商的名字中按照 `name (location)` 这样的格式列出供应商的位
置。

此报表需要单个值,而表中数据存储在两个列 `vend_name` 和 `vend_country` 中。
此外,需要用括号将 `vend_country` 括起来,这些东西都没有
明确存储在数据库表中。我们来看看怎样编写返回供应商名和位置的SELECT 语句。

**拼接(concatenate)** 将值联结到一起构成单个值。

解决办法是把两个列拼接起来。在MySQL的 SELECT 语句中,可使用`Concat()` 函数来拼接两个列。

**MySQL的不同之处** 多数DBMS使用 + 或 || 来实现拼接,
MySQL则使用 `Concat()` 函数来实现。当把SQL语句转换成
MySQL语句时一定要把这个区别铭记在心。

```shell
MariaDB [test]> select concat(vend_name,vend_country) from vendors;
+--------------------------------+
| concat(vend_name,vend_country) |
+--------------------------------+
| Anvils R UsUSA                 |
| LT SuppliesUSA                 |
| ACMEUSA                        |
| Furball Inc.USA                |
| Jet SetEngland                 |
| Jouets Et OursFrance           |
+--------------------------------+
6 rows in set (0.01 sec)

MariaDB [test]> select concat(vend_name,' (',vend_country,')') from vendors;
+-----------------------------------------+
| concat(vend_name,' (',vend_country,')') |
+-----------------------------------------+
| Anvils R Us (USA)                       |
| LT Supplies (USA)                       |
| ACME (USA)                              |
| Furball Inc. (USA)                      |
| Jet Set (England)                       |
| Jouets Et Ours (France)                 |
+-----------------------------------------+
6 rows in set (0.00 sec)
```

Concat() 拼接串,即把多个串连接起来形成一个较长的串。
Concat() 需要一个或多个指定的串,各个串之间用逗号分隔。

上面的 SELECT 语句连接以下4个元素:
* 存储在 vend_name 列中的名字;
* 包含一个空格和一个左圆括号的串;
* 存储在 vend_country 列中的国家;
* 包含一个右圆括号的串。

从上述输出中可以看到, SELECT 语句返回包含上述4个元素的单个列(计算字段)。

>> 在前面曾提到通过删除数据右侧多余的空格来整理数据,这可以使用MySQL的 `RTrim()` 函数来完成,如下所示:

```shell
MariaDB [test]> select concat(rtrim(vend_name),' (',rtrim(vend_country),')') from vendors;
+-------------------------------------------------------+
| concat(rtrim(vend_name),' (',rtrim(vend_country),')') |
+-------------------------------------------------------+
| Anvils R Us (USA)                                     |
| LT Supplies (USA)                                     |
| ACME (USA)                                            |
| Furball Inc. (USA)                                    |
| Jet Set (England)                                     |
| Jouets Et Ours (France)                               |
+-------------------------------------------------------+
6 rows in set (0.00 sec)
```

**Trim 函数** MySQL除了支持 `RTrim()` (正如刚才所见,它去掉
串右边的空格),还支持 `LTrim()` (去掉串左边的空格)以及`Trim()` (去掉串左右两边的空格)。

**使用别名** 从前面的输出中可以看到, SELECT 语句拼接地址字段工作得很好。
但此新计算列的名字是什么呢?实际上它没有名字,它只是一个值。如
果仅在SQL查询工具中查看一下结果,这样没有什么不好。但是,一个未
命名的列不能用于客户机应用中,因为客户机没有办法引用它。
为了解决这个问题,SQL支持列别名。

**别名(alias)** 是一个字段或值的替换名。别名用 AS 关键字赋予。

>> 创建一个包含指定计算的名为 vend_title 的计算字段

```shell
MariaDB [test]> select concat(rtrim(vend_name),' (',rtrim(vend_country),')') as vend_title from vendors order by vend_name;
+-------------------------+
| vend_title              |
+-------------------------+
| ACME (USA)              |
| Anvils R Us (USA)       |
| Furball Inc. (USA)      |
| Jet Set (England)       |
| Jouets Et Ours (France) |
| LT Supplies (USA)       |
+-------------------------+
6 rows in set (0.00 sec)
```

**别名的其他用途** 别名还有其他用途。常见的用途包括在实际
的表列名包含不符合规定的字符(如空格)时重新命名它,在
原来的名字含混或容易误解时扩充它,等等。

**导出列** 别名有时也称为导出列(derived column),不管称为什么,它们所代表的都是相同的东西。

> 执行算术计算

计算字段的另一常见用途是对检索出的数据进行算术计算。

>> `orders` 表包含收到的所有订单, `orderitems` 表包含每个订单中的各项物品。
SQL语句检索订单号 20005 中的所有物品，并汇总物品的价格(单价乘以订购数量)

```shell
MariaDB [test]> select *,quantity*item_price as expanded_price from orderitems where order_num = 20005;
+-----------+------------+---------+----------+------------+----------------+
| order_num | order_item | prod_id | quantity | item_price | expanded_price |
+-----------+------------+---------+----------+------------+----------------+
|     20005 |          1 | ANV01   |       10 |       5.99 |          59.90 |
|     20005 |          2 | ANV02   |        3 |       9.99 |          29.97 |
|     20005 |          3 | TNT2    |        5 |      10.00 |          50.00 |
|     20005 |          4 | FB      |        1 |      10.00 |          10.00 |
+-----------+------------+---------+----------+------------+----------------+
4 rows in set (0.00 sec)

MariaDB [test]> select *,quantity*item_price  expanded_price from orderitems where order_num = 20005;
+-----------+------------+---------+----------+------------+----------------+
| order_num | order_item | prod_id | quantity | item_price | expanded_price |
+-----------+------------+---------+----------+------------+----------------+
|     20005 |          1 | ANV01   |       10 |       5.99 |          59.90 |
|     20005 |          2 | ANV02   |        3 |       9.99 |          29.97 |
|     20005 |          3 | TNT2    |        5 |      10.00 |          50.00 |
|     20005 |          4 | FB      |        1 |      10.00 |          10.00 |
+-----------+------------+---------+----------+------------+----------------+
4 rows in set (0.00 sec)
```

MySQL支持下表中列出的基本算术操作符。此外,圆括号可用来区分优先顺序。

|MySQL算术操作符|说 明|
|:--|:--|
|+ |加|
|- |减|
|* |乘|
|/ |除|

**如何测试计算** SELECT 提供了测试和试验函数与计算的一个很好的办法。
虽然 SELECT 通常用来从表中检索数据,但可以省略 FROM 子句以便简单地访问和处理表达式。
通过下面一些例子,可以明白如何根据需要使用 SELECT 进行试验。

>> SELECT3*2; 将返回 6

```shell
MariaDB [(none)]> select 3*2;
+-----+
| 3*2 |
+-----+
|   6 |
+-----+
1 row in set (0.00 sec)
```

>> SELECT Trim(' abc '); 将返回 abc

```shell
MariaDB [(none)]> select trim(' abc ');
+---------------+
| trim(' abc ') |
+---------------+
| abc           |
+---------------+
1 row in set (0.00 sec)
```

>> SELECT Now() 利用 Now() 函数返回当前日期和时间。

```shell
MariaDB [(none)]> select now();
+---------------------+
| now()               |
+---------------------+
| 2016-09-17 21:25:08 |
+---------------------+
1 row in set (0.00 sec)
```

> 小结
>> 本章介绍了计算字段以及如何创建计算字段。我们用例子说明了计
算字段在串拼接和算术计算的用途。此外,还学习了如何创建和使用别
名,以便应用程序能引用计算字段。

---

#### 使用数据处理函数

本章介绍什么是函数, MySQL支持何种函数,以及如何使用这些函数。

> 函数

与其他大多数计算机语言一样, SQL支持利用函数来处理数据。函数
一般是在数据上执行的,它给数据的转换和处理提供了方便。

在前一章中用来去掉串尾空格的 RTrim() 就是一个函数的例子。

**函数没有SQL的可移植性强** 能运行在多个系统上的代码称
为可移植的(portable)。相对来说,多数SQL语句是可移植的,
在SQL实现之间有差异时,这些差异通常不那么难处理。而函
数的可移植性却不强。几乎每种主要的DBMS的实现都支持其
他实现不支持的函数,而且有时差异还很大。
为了代码的可移植,许多SQL程序员不赞成使用特殊实现的功
能。虽然这样做很有好处,但不总是利于应用程序的性能。如
果不使用这些函数,编写某些应用程序代码会很艰难。必须利
用其他方法来实现DBMS非常有效地完成的工作。
如果你决定使用函数,应该保证做好代码注释,以便以后你(或
其他人)能确切地知道所编写SQL代码的含义。

> 使用函数

大多数SQL实现支持以下类型的函数。
* 用于处理文本串(如删除或填充值,转换值为大写或小写)的文本函数。
* 用于在数值数据上进行算术操作(如返回绝对值,进行代数运算)的数值函数。
* 用于处理日期和时间值并从这些值中提取特定成分(例如,返回
两个日期之差,检查日期有效性等)的日期和时间函数。
* 返回DBMS正使用的特殊信息(如返回用户登录信息,检查版本细节)的系统函数。

1.文本处理函数

上一章中我们已经看过一个文本处理函数的例子,其中使用 `RTrim()`
函数来去除列值右边的空格。下面是另一个例子,这次使用 `Upper()` 函数Upper() ：将文本转换为大写。

>> 检索供应商表vendors中的供应商名vend_name，并都转换为大写，排序。

```shell
MariaDB [test]> select vend_name,upper(vend_name) as vend_name_upcase from vendors order by vend_name;
+----------------+------------------+
| vend_name      | vend_name_upcase |
+----------------+------------------+
| ACME           | ACME             |
| Anvils R Us    | ANVILS R US      |
| Furball Inc.   | FURBALL INC.     |
| Jet Set        | JET SET          |
| Jouets Et Ours | JOUETS ET OURS   |
| LT Supplies    | LT SUPPLIES      |
+----------------+------------------+
6 rows in set (0.00 sec)
```

下表列出了某些常用的文本处理函数。

|常用的文本处理函数|说 明|
|:--|:--|
|Left()|返回串左边的字符|
|Right()|返回串右边的字符|
|Lower()|将串转换为小写|
|Upper() |将串转换为大写|
|LTrim()|去掉串左边的空格|
|RTrim()|去掉串右边的空格|
|Length()|返回串的长度|
|Soundex()| 返回串的SOUNDEX值|
|Locate()|找出串的一个子串|
|SubString()| 返回子串的字符|

上表中的 SOUNDEX 需要做进一步的解释。 SOUNDEX 是一个将任何文
本串转换为描述其语音表示的字母数字模式的算法。 SOUNDEX 考虑了类似
的发音字符和音节,使得能对串进行发音比较而不是字母比较。虽然
SOUNDEX 不是SQL概念,但MySQL(就像多数DBMS一样)都提供对
SOUNDEX 的支持。

下面给出一个使用 Soundex() 函数的例子。

  >> `customers` 表中有一个顾客 `Coyote Inc.` ,其联系名为` Y Lee `。但如果这是输入错误,此联系名实
际应该是 `Y Lie` ,怎么办?显然,按正确的联系名搜索不会返回数据。

```shell
MariaDB [test]> select cust_name,cust_contact from customers where cust_contact = 'Y Lie';
Empty set (0.00 sec)

MariaDB [test]> select cust_name,cust_contact from customers where soundex(cust_contact) = soundex('Y Lie');
+-------------+--------------+
| cust_name   | cust_contact |
+-------------+--------------+
| Coyote Inc. | Y Lee        |
+-------------+--------------+
1 row in set (0.00 sec)
```

2.日期和时间处理函数

日期和时间采用相应的数据类型和特殊的格式存储,以便能快速和
有效地排序或过滤,并且节省物理存储空间。

一般,应用程序不使用用来存储日期和时间的格式,因此日期和时
间函数总是被用来读取、统计和处理这些值。由于这个原因,日期和时
间函数在MySQL语言中具有重要的作用。

下表列出了某些常用的日期和时间处理函数。

|常用日期和时间处理函数|说 明|
|:--|:--|
|AddDate()| 增加一个日期(天、周等)|
|AddTime()| 增加一个时间(时、分等)|
|CurDate()| 返回当前日期|
|CurTime()| 返回当前时间|
|Date() |返回日期时间的日期部分|
|DateDiff()| 计算两个日期之差|
|Date_Add()| 高度灵活的日期运算函数|
|Date_Format()| 返回一个格式化的日期或时间串|
|Day() |返回一个日期的天数部分|
|DayOfWeek()| 对于一个日期,返回对应的星期几|
|Hour() |返回一个时间的小时部分|
|Minute()| 返回一个时间的分钟部分|
|Month() |返回一个日期的月份部分|
|Now() |返回当前日期和时间|
|Second()| 返回一个时间的秒部分|
|Time() |返回一个日期时间的时间部分|
|Year() |返回一个日期的年份部分|

这是重新复习用 WHERE 进行数据过滤的一个好时机。迄今为止,我
们都是用比较数值和文本的 WHERE 子句过滤数据,但数据经常需要用日
期进行过滤。用日期进行过滤需要注意一些别的问题和使用特殊的
MySQL函数。

首先需要注意的是MySQL使用的日期格式。无论你什么时候指定一
个日期,不管是插入或更新表值还是用 WHERE 子句进行过滤,日期必须为
格式yyyy-mm-dd。因此,2005年9月1日,给出为2005-09-01。虽然其他的
日期格式可能也行,但这是首选的日期格式,因为它排除了多义性(如,
04/05/06是2006年5月4日或2006年4月5日或2004年5月6日或......)。

**应该总是使用4位数字的年份** 支持2位数字的年份,MySQL
处理00-69为2000-2069,处理70-99为1970-1999。虽然它们可
能是打算要的年份,但使用完整的4位数字年份更可靠,因为
MySQL不必做出任何假定。

>> 检索出一个订单记录,该订单记录的 `order_date` 为 `2005-09-01`

```shell
MariaDB [test]> select * from orders where order_date='2005-09-01';
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
+-----------+---------------------+---------+
1 row in set (0.00 sec)
```

但是,使用 `WHERE order_date = '2005-09-01'` 可靠吗? `order_date `的数据类型为 `datetime` 。
这种类型存储日期及时间值。样例表中的值全都具有时间值 00:00:00 ,但实际中很可能并不总是这样。如果
用当前日期和时间存储订单日期(因此你不仅知道订单日期,还知道下订单当天的时间),怎么办?

比如,存储的 `order_date` 值为`2005-09-01 11:30:05` ,则 `WHERE order_date = '2005-09-01'` 失败。
即使给出具有该日期的一行,也不会把它检索出来,因为 WHERE 匹配失败。

解决办法是指示MySQL仅将给出的日期与列中的日期部分进行比较,而不是将给出的日期与整个列值进行比较。
为此,必须使用 `Date()`函数。 `Date(order_date) `指示MySQL仅提取列的日期部分,更可靠的SELECT 语句为:

```shell
MariaDB [test]> select * from orders where date(order_date)='2005-09-01';
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
+-----------+---------------------+---------+
1 row in set (0.00 sec)
```

**如果要的是日期,请使用 Date()** 如果你想要的仅是日期,
则使用 Date() 是一个良好的习惯,即使你知道相应的列只包
含日期也是如此。这样,如果由于某种原因表中以后有日期和
时间值,你的SQL代码也不用改变。当然,也存在一个 Time()
函数,在你只想要时间时应该使用它。
Date() 和 Time() 都是在MySQL 4.1.1中第一次引入的。

在你知道了如何用日期进行相等测试后,其他操作符的使用也就很清楚了。不过,还有一种日期比较需要说明。

>> 检索出2005年9月下的所有订单

```shell
MariaDB [test]> select * from orders where date(order_date) between '2005-09-01' and '2005-09-30';
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
|     20006 | 2005-09-12 00:00:00 |   10003 |
|     20007 | 2005-09-30 00:00:00 |   10004 |
+-----------+---------------------+---------+
3 rows in set (0.00 sec)
```

还有另外一种办法(一种不需要记住每个月中有多少天或不需要操心闰年2月的办法)

```shell
MariaDB [test]> select * from orders where year(order_date) = 2005 and month(order_date)= 9 ;
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
|     20006 | 2005-09-12 00:00:00 |   10003 |
|     20007 | 2005-09-30 00:00:00 |   10004 |
+-----------+---------------------+---------+
3 rows in set (0.01 sec)
```

**MySQL的版本差异** MySQL 4.1.1中增加了许多日期和时间
函数。如果你使用的是更早的MySQL版本,应该查阅具体的
文档以确定可以使用哪些函数。


3.数值处理函数

数值处理函数仅处理数值数据。这些函数一般主要用于代数、三角
或几何运算,因此没有串或日期—时间处理函数的使用那么频繁。

具有讽刺意味的是,在主要DBMS的函数中,数值函数是最一致最统
一的函数。下表列出一些常用的数值处理函数。

|常用数值处理函数|说 明|
|:--|:--|
|Abs()| 返回一个数的绝对值|
|Cos()| 返回一个角度的余弦|
|Exp()| 返回一个数的指数值|
|Mod()| 返回除操作的余数|
|Pi()| 返回圆周率|
|Rand()| 返回一个随机数|
|Sin()| 返回一个角度的正弦|
|Sqrt()| 返回一个数的平方根|
|Tan()| 返回一个角度的正切|

>> 测试函数功能

```shell
MariaDB [test]> select pi();
+----------+
| pi()     |
+----------+
| 3.141593 |
+----------+
1 row in set (0.00 sec)

MariaDB [test]> select rand();
+--------------------+
| rand()             |
+--------------------+
| 0.7680412305221346 |
+--------------------+
1 row in set (0.00 sec)
```

> 小结
>> 本章介绍了如何使用SQL的数据处理函数,并着重介绍了日期处理函数。

---

#### 汇总数据

本章介绍什么是SQL的聚集函数以及如何利用它们汇总表的数据。

> 聚集函数

我们经常需要汇总数据而不用把它们实际检索出来,为此MySQL提
供了专门的函数。使用这些函数,MySQL查询可用于检索数据,以便分
析和报表生成。这种类型的检索例子有以下几种。

* 确定表中行数(或者满足某个条件或包含某个特定值的行数)。
* 获得表中行组的和。
* 找出表列(或所有行或某些特定的行)的最大值、最小值和平均值。

上述例子都需要对表中数据(而不是实际数据本身)汇总。因此,
返回实际表数据是对时间和处理资源的一种浪费(更不用说带宽了)。
重复一遍,实际想要的是汇总信息。

为方便这种类型的检索,MySQL给出了5个聚集函数，见下表，这些函数能进行上述罗列的检索。

**聚集函数(aggregate function)** 运行在行组上,计算和返回单个值的函数。

|SQL聚集函数|说明|
|:--|:--|
|AVG()| 返回某列的平均值|
|COUNT()| 返回某列的行数|
|MAX()| 返回某列的最大值|
|MIN()| 返回某列的最小值|
|SUM()| 返回某列值之和|

**标准偏差** MySQL还支持一系列的标准偏差聚集函数,但该教案并未涉及这些内容。

1.AVG() 函数

AVG() 通过对表中行数计数并计算特定列值之和,求得该列的平均
值。 AVG() 可用来返回所有列的平均值,也可以用来返回特定列或行的平
均值。

>> 使用 `AVG()` 返回 `products` 表中所有产品的平均价格 `avg_Price`

```shell
MariaDB [test]> select avg(prod_price) as avg_price from products;
+-----------+
| avg_price |
+-----------+
| 16.133571 |
+-----------+
1 row in set (0.00 sec)
```

>> 使用 `AVG()` 返回 `products` 表中产品供应商1003提供的商品的平均价格 `avg_Price`

```shell
MariaDB [test]> select avg(prod_price) as avg_price from products where vend_id=1003;
+-----------+
| avg_price |
+-----------+
| 13.212857 |
+-----------+
1 row in set (0.00 sec)
```

**只用于单个列** AVG() 只能用来确定特定数值列的平均值,而
且列名必须作为函数参数给出。为了获得多个列的平均值,
必须使用多个 AVG() 函数。

**NULL 值** AVG() 函数忽略列值为 NULL 的行

2.COUNT() 函数

COUNT() 函数进行计数。可利用 COUNT() 确定表中行的数目或符合特
定条件的行的数目。

COUNT() 函数有两种使用方式。
* 使用 COUNT(*) 对表中行的数目进行计数,不管表列中包含的是空值( NULL )还是非空值。
* 使用 COUNT(column) 对特定列中具有值的行进行计数,忽略NULL 值。

>> 返回 customers 表中客户的总数

```shell
MariaDB [test]> select count(prod_name) from products;
+------------------+
| count(prod_name) |
+------------------+
|               14 |
+------------------+
1 row in set (0.00 sec)

MariaDB [test]> select count(*) from products;
+----------+
| count(*) |
+----------+
|       14 |
+----------+
1 row in set (0.00 sec)
```

>> 对具有电子邮件地址的客户计数,别名num_cust

```shell
MariaDB [test]> select cust_email from customers;
+---------------------+
| cust_email          |
+---------------------+
| ylee@coyote.com     |
| NULL                |
| rabbit@wascally.com |
| sam@yosemite.com    |
| NULL                |
+---------------------+
5 rows in set (0.00 sec)

MariaDB [test]> select count(cust_email) from customers;
+-------------------+
| count(cust_email) |
+-------------------+
|                 3 |
+-------------------+
1 row in set (0.00 sec)

MariaDB [test]> select count(*) from customers;
+----------+
| count(*) |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)

MariaDB [test]> select count(cust_email) as num_cust from customers;
+----------+
| num_cust |
+----------+
|        3 |
+----------+
1 row in set (0.00 sec)
```

**NULL 值** 如果指定列名,则指定列的值为空的行被 COUNT()
函数忽略,但如果 COUNT() 函数中用的是星号( * ),则不忽
略。


3.MAX() 函数

MAX() 返回指定列中的最大值。 MAX() 要求指定列名。

>> MAX() 返回 products 表中最贵的物品的价格

```shell
MariaDB [test]> select prod_name,max(prod_price) as max_price from products;
+--------------+-----------+
| prod_name    | max_price |
+--------------+-----------+
| .5 ton anvil |     55.00 |
+--------------+-----------+
1 row in set (0.00 sec)
```

**对非数值数据使用 MAX()** 虽然 MAX() 一般用来找出最大的
数值或日期值,但MySQL允许将它用来返回任意列中的最大
值,包括返回文本列中的最大值。在用于文本数据时,如果数
据按相应的列排序,则 MAX() 返回最后一行。

```shell
MariaDB [test]> select prod_name,max(prod_name) as max_name from products;
+--------------+----------------+
| prod_name    | max_name       |
+--------------+----------------+
| .5 ton anvil | TNT (5 sticks) |
+--------------+----------------+
1 row in set (0.00 sec)
```

**NULL 值** MAX() 函数忽略列值为 NULL 的行。

4.MIN() 函数

MIN() 的功能正好与 MAX() 功能相反,它返回指定列的最小值。与MAX() 一样, MIN() 要求指定列名。

>> MIN() 返回 products 表中最便宜物品的价格

```shell
MariaDB [test]> select prod_name,min(prod_price) as min_price from products;
+--------------+-----------+
| prod_name    | min_price |
+--------------+-----------+
| .5 ton anvil |      2.50 |
+--------------+-----------+
1 row in set (0.00 sec)
```

**对非数值数据使用 MIN()** MIN() 函数与 MAX() 函数类似,
MySQL允许将它用来返回任意列中的最小值,包括返回文本
列中的最小值。在用于文本数据时,如果数据按相应的列排序,
则 MIN() 返回最前面的行。

**NULL 值** MIN() 函数忽略列值为 NULL 的行。

5.SUM() 函数

SUM() 用来返回指定列值的和(总计)。

>> `orderitems`表 包含订单中实际的物品,每个物品有相应的数量( `quantity` )。
检索所订购物品的总数(所有`quantity` 值之和)

```shell
MariaDB [test]> desc orderitems;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| order_num  | int(11)      | NO   | PRI | NULL    |       |
| order_item | int(11)      | NO   | PRI | NULL    |       |
| prod_id    | char(10)     | NO   | MUL | NULL    |       |
| quantity   | int(11)      | NO   |     | NULL    |       |
| item_price | decimal(8,2) | NO   |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

MariaDB [test]> select sum(quantity) from orderitems;
+---------------+
| sum(quantity) |
+---------------+
|           174 |
+---------------+
1 row in set (0.00 sec)

MariaDB [test]> select sum(quantity) as items_ordered  from orderitems where order_num = 20005;
+---------------+
| items_ordered |
+---------------+
|            19 |
+---------------+
1 row in set (0.00 sec)
```

>> `SUM()` 也可以用来合计计算值。在下面的例子中,合计每项物品的`item_price*quantity` ,得出总的订单金额`total_price`

```shell
MariaDB [test]> select item_price*quantity from orderitems;
+---------------------+
| item_price*quantity |
+---------------------+
|               59.90 |
|               29.97 |
|               50.00 |
|               10.00 |
|               55.00 |
|             1000.00 |
|              125.00 |
|               10.00 |
|                8.99 |
|                4.49 |
|               14.99 |
+---------------------+
11 rows in set (0.00 sec)

MariaDB [test]> select sum(item_price*quantity) as total_price from orderitems;
+-------------+
| total_price |
+-------------+
|     1368.34 |
+-------------+
1 row in set (0.01 sec)
```

**在多个列上进行计算** 如本例所示,利用标准的算术操作符,
所有聚集函数都可用来执行多个列上的计算。

**NULL 值** SUM() 函数忽略列值为 NULL 的行。

> 聚集不同值

**MySQL 5 及 后 期 版 本** 下面将要介绍的聚集函数的
DISTINCT 的使用,已经被添加到MySQL 5.0.3中。下面所述内容在MySQL 4.x中不能正常运行。

以上5个聚集函数都可以如下使用:
* 对所有的行执行计算,指定 ALL 参数或不给参数(因为 ALL 是默认行为);
* 只包含不同的值,指定 DISTINCT 参数。

**ALL 为默认** ALL 参数不需要指定,因为它是默认行为。如果不指定 DISTINCT ,则假定为 ALL 。

>> 使用 AVG() 函数返回特定供应商提供的产品的平均价格。
它与上面的 SELECT 语句相同,但使用了 DISTINCT 参数,因此平均值只
考虑各个不同的价格:

```shell
MariaDB [test]> select prod_price from products order by prod_price;
+------------+
| prod_price |
+------------+
|       2.50 |
|       2.50 |
|       3.42 |
|       4.49 |
|       5.99 |
|       8.99 |
|       9.99 |
|      10.00 |
|      10.00 |
|      13.00 |
|      14.99 |
|      35.00 |
|      50.00 |
|      55.00 |
+------------+
14 rows in set (0.00 sec)

MariaDB [test]> select distinct prod_price from products order by prod_price;
+------------+
| prod_price |
+------------+
|       2.50 |
|       3.42 |
|       4.49 |
|       5.99 |
|       8.99 |
|       9.99 |
|      10.00 |
|      13.00 |
|      14.99 |
|      35.00 |
|      50.00 |
|      55.00 |
+------------+
12 rows in set (0.00 sec)

MariaDB [test]> select avg(distinct prod_price) from products where vend_id = 1003;
+--------------------------+
| avg(distinct prod_price) |
+--------------------------+
|                15.998000 |
+--------------------------+
1 row in set (0.00 sec)

MariaDB [test]> select avg(prod_price) from products where vend_id = 1003;
+-----------------+
| avg(prod_price) |
+-----------------+
|       13.212857 |
+-----------------+
1 row in set (0.00 sec)

```
可以看到,在使用了 `DISTINCT` 后,此例子中的 `avg_price` 比
较高,因为有多个物品具有相同的较低价格。排除它们提升了
平均价格。

**注意** 如果指定列名,则 DISTINCT 只能用于 COUNT() 。 DISTINCT
不能用于 COUNT(*),因此不允许使用COUNT(DISTINCT),
否则会产生错误 。类似地, DISTINCT 必须使用列名,不能用
于计算或表达式。

**将 DISTINCT 用于 MIN() 和 MAX()** 虽然 DISTINCT 从技术上可
用于 MIN() 和 MAX() ,但这样做实际上没有价值。一个列中的
最小值和最大值不管是否包含不同值都是相同的。


> 组合聚集函数

目前为止的所有聚集函数例子都只涉及单个函数。但实际上 SELECT
语句可根据需要包含多个聚集函数。

>> 检索products 表中物品的数目`num_items`,产品价格的最高`price_max`、最低`price_min`以及平均
值`price_avg`

```shell
MariaDB [test]> select count(*) as num_items,min(prod_price)as price_min,max(prod_price) as price_max,avg(prod_price) as price_avg from products;
+-----------+-----------+-----------+-----------+
| num_items | price_min | price_max | price_avg |
+-----------+-----------+-----------+-----------+
|        14 |      2.50 |     55.00 | 16.133571 |
+-----------+-----------+-----------+-----------+
1 row in set (0.00 sec)
```

**取别名** 在指定别名以包含某个聚集函数的结果时,不应该使
用表中实际的列名。虽然这样做并非不合法,但使用唯一的名
字会使你的SQL更易于理解和使用(以及将来容易排除故障)。

> 小结
>> 聚集函数用来汇总数据。MySQL支持一系列聚集函数,可以用多种
方法使用它们以返回所需的结果。这些函数是高效设计的,它们返回结
果一般比你在自己的客户机应用程序中计算要快得多。

---

#### 分组数据

本章将介绍如何分组数据,以便能汇总表内容的子集。这涉及两个
新 SELECT 语句子句,分别是 GROUP BY 子句和 HAVING 子句。

> 数据分组

从上一章知道, SQL聚集函数可用来汇总数据。这使我们能够对行进
行计数,计算和与平均数,获得最大和最小值而不用检索所有数据。
目前为止的所有计算都是在表的所有数据或匹配特定的 WHERE 子句的
数据上进行的。

>> 下面的例子返回供应商`vend_id` 1003 提供的产品数目

```shell
MariaDB [test]> select vend_id,count(vend_id) as num_prods from products where vend_id=1003;
+---------+----------+
| vend_id | num_prods |
+---------+----------+
|    1003 |        7 |
+---------+----------+
1 row in set (0.00 sec)
```

但如果要返回每个供应商提供的产品数目怎么办?或者返回只提供
单项产品的供应商所提供的产品,或返回提供10个以上产品的供应商怎
么办?

这就是分组显身手的时候了。分组允许把数据分为多个逻辑组,以
便能对每个组进行聚集计算。

> 创建分组

分组是在 SELECT 语句的 GROUP BY 子句中建立的。理解分组的最好办
法是看一个例子

>> 按 `vend_id` 排序并分组数据计算每个供应商的商品总数 `num_prods`

```shell
MariaDB [test]> select vend_id,count(vend_id) as num_prods from products group by vend_id;
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    1001 |         3 |
|    1002 |         2 |
|    1003 |         7 |
|    1005 |         2 |
+---------+-----------+
4 rows in set (0.00 sec)
```

因为使用了 GROUP BY ,就不必指定要计算和估值的每个组了。系统
会自动完成。 GROUP BY 子句指示MySQL分组数据,然后对每个组而不是
整个结果集进行聚集。

在具体使用 GROUP BY 子句前,需要知道一些重要的规定。
* GROUP BY 子句可以包含任意数目的列。这使得能对分组进行嵌套,
为数据分组提供更细致的控制。
* 如果在 GROUP BY 子句中嵌套了分组,数据将在最后规定的分组上
进行汇总。换句话说,在建立分组时,指定的所有列都一起计算
(所以不能从个别的列取回数据)。
* GROUP BY 子句中列出的每个列都必须是检索列或有效的表达式
(但不能是聚集函数)。如果在 SELECT 中使用表达式,则必须在
GROUP BY 子句中指定相同的表达式。不能使用别名。
* 除聚集计算语句外, SELECT 语句中的每个列都必须在 GROUP BY 子
句中给出。
* 如果分组列中具有 NULL 值,则 NULL 将作为一个分组返回。如果列
中有多行 NULL 值,它们将分为一组。
* GROUP BY 子句必须出现在 WHERE 子句之后, ORDER BY 子句之前。

**使用 ROLLUP** 使用 WITH ROLLUP 关键字,可以得到每个分组以
及每个分组汇总级别(针对每个分组)的值,如下所示:

```shell
MariaDB [test]> select vend_id,count(vend_id) as num_prods from products group by vend_id with rollup;
+---------+-----------+
| vend_id | num_prods |
+---------+-----------+
|    1001 |         3 |
|    1002 |         2 |
|    1003 |         7 |
|    1005 |         2 |
|    NULL |        14 |
+---------+-----------+
5 rows in set (0.00 sec)
```

> 过滤分组

除了能用 GROUP BY 分组数据外,MySQL还允许过滤分组,规定包括
哪些分组,排除哪些分组。例如,可能想要列出至少有两个订单的所有 113
顾客。为得出这种数据,必须基于完整的分组而不是个别的行进行过滤。
。但是,在这个例
我们已经看到了 WHERE 子句的作用(第6章中引入)
子中 WHERE 不能完成任务,因为 WHERE 过滤指定的是行而不是分组。事实
上, WHERE 没有分组的概念。
那么,不使用 WHERE 使用什么呢?MySQL为此目的提供了另外的子
句,那就是 HAVING 子句。 HAVING 非常类似于 WHERE 。事实上,目前为止所
学过的所有类型的 WHERE 子句都可以用 HAVING 来替代。唯一的差别是
WHERE 过滤行,而 HAVING 过滤分组。
HAVING 支持所有 WHERE 操作符
在第6章和第7章中,我们学习
了 WHERE 子句的条件(包括通配符条件和带多个操作符的子
句)。所学过的有关 WHERE 的所有这些技术和选项都适用于
HAVING 。它们的句法是相同的,只是关键字有差别。
那么,怎么过滤分组呢?请看以下的例子:

>> 过滤两个以上的订单的那些分组

```shell
MariaDB [test]> select cust_id,count(*) as orders from orders group by cust_id;
+---------+--------+
| cust_id | orders |
+---------+--------+
|   10001 |      2 |
|   10003 |      1 |
|   10004 |      1 |
|   10005 |      1 |
+---------+--------+
4 rows in set (0.00 sec)

MariaDB [test]> select cust_id,count(*) as orders from orders group by cust_id having count(*) >= 2;
+---------+--------+
| cust_id | orders |
+---------+--------+
|   10001 |      2 |
+---------+--------+
1 row in set (0.00 sec)
```

正如所见,这里 WHERE 子句不起作用,因为过滤是基于分组聚集值而
不是特定行值的。

**HAVING 和 WHERE 的差别** 这里有另一种理解方法, WHERE 在数据
分组前进行过滤, HAVING 在数据分组后进行过滤。这是一个重
要的区别, WHERE 排除的行不包括在分组中。这可能会改变计
算值,从而影响 HAVING 子句中基于这些值过滤掉的分组。

那么,有没有在一条语句中同时使用 WHERE 和 HAVING 子句的需要呢?事实上,确实有。
假如想进一步过滤上面的语句,使它返回过去12个月内具有两个以上订单的顾客。
为达到这一点,可增加一条 WHERE 子句,过滤出过去12个月内下过的订单。然后再增加 HAVING 子句过滤出具有两个
以上订单的分组。

为更好地理解,请看下面的例子。

>> 列出具有 2 个(含)以上、价格为 10 (含)以上的产品的供应商:

```shell
MariaDB [test]> select vend_id,count(vend_id) from products where prod_price >= 10 group by vend_id ;
+---------+----------------+
| vend_id | count(vend_id) |
+---------+----------------+
|    1001 |              1 |
|    1003 |              4 |
|    1005 |              2 |
+---------+----------------+
3 rows in set (0.00 sec)

MariaDB [test]> select vend_id,count(vend_id) from products where prod_price >= 10 group by vend_id having count(vend_id) >= 2 ;
+---------+----------------+
| vend_id | count(vend_id) |
+---------+----------------+
|    1003 |              4 |
|    1005 |              2 |
+---------+----------------+
2 rows in set (0.01 sec)
```

这条语句中,使用了聚集函数的基本 `SELECT` ,它与前
面的例子很相像。 `WHERE` 子句过滤所有 `prod_price` 至少为 10 的
行。然后按 `vend_id `分组数据, `HAVING` 子句过滤计数为 2 或 2 以上的分组。
如果没有 `WHERE` 子句,将会多检索出两行(供应商 1002 ,销售的所有产品
价格都在 10 以下;供应商 1001 ,销售3个产品,但只有一个产品的价格大
于等于 10 ):

```shell
MariaDB [test]> select vend_id,count(vend_id) from products group by vend_id having count(vend_id) >= 2 ;
+---------+----------------+
| vend_id | count(vend_id) |
+---------+----------------+
|    1001 |              3 |
|    1002 |              2 |
|    1003 |              7 |
|    1005 |              2 |
+---------+----------------+
4 rows in set (0.00 sec)
```

> 分组和排序

虽然 GROUP BY 和 ORDER BY 经常完成相同的工作,但它们是非常不同的。

下表汇总了它们之间的差别

|ORDER BY |GROUP BY|
|:-|:--|
|排序产生的输出|分组行。但输出可能不是分组的顺序|
|任意列都可以使用(甚至非选择的列也可以使用)|只可能使用选择列或表达式列,而且必须使用每个选择列表达式|
|不一定需要|如果与聚集函数一起使用列(或表达式),则必须使用|

表中列出的第一项差别极为重要。我们经常发现用 GROUP BY 分组的数据确实是以分组顺序输出的。
但情况并不总是这样,它并不是SQL规范所要求的。此外,用户也可能会要求以不同于分组的顺序排序。
仅因为你以某种方式分组数据(获得特定的分组聚集值),并不表示你需要以相同的方式排序输出。
应该提供明确的 ORDER BY 子句,即使其效果等同于 GROUP BY 子句也是如此。

**不要忘记 ORDER BY** 一般在使用 GROUP BY 子句时,应该也给
出 ORDER BY 子句。这是保证数据正确排序的唯一方法。千万
不要仅依赖 GROUP BY 排序数据。

为说明 GROUP BY 和 ORDER BY 的使用方法,请看一个例子。下面的
SELECT 语句类似于前面那些例子。

>> 检索总计订单价格大于等于 50 的订单的订单号和总计订单价格

```shell
MariaDB [test]> select * from orderitems;
+-----------+------------+---------+----------+------------+
| order_num | order_item | prod_id | quantity | item_price |
+-----------+------------+---------+----------+------------+
|     20005 |          1 | ANV01   |       10 |       5.99 |
|     20005 |          2 | ANV02   |        3 |       9.99 |
|     20005 |          3 | TNT2    |        5 |      10.00 |
|     20005 |          4 | FB      |        1 |      10.00 |
|     20006 |          1 | JP2000  |        1 |      55.00 |
|     20007 |          1 | TNT2    |      100 |      10.00 |
|     20008 |          1 | FC      |       50 |       2.50 |
|     20009 |          1 | FB      |        1 |      10.00 |
|     20009 |          2 | OL1     |        1 |       8.99 |
|     20009 |          3 | SLING   |        1 |       4.49 |
|     20009 |          4 | ANV03   |        1 |      14.99 |
+-----------+------------+---------+----------+------------+
11 rows in set (0.00 sec)

MariaDB [test]> select order_num,sum(quantity*item_price) from orderitems group by order_num order by sum(quantity*item_price);
+-----------+--------------------------+
| order_num | sum(quantity*item_price) |
+-----------+--------------------------+
|     20009 |                    38.47 |
|     20006 |                    55.00 |
|     20008 |                   125.00 |
|     20005 |                   149.87 |
|     20007 |                  1000.00 |
+-----------+--------------------------+
5 rows in set (0.00 sec)

MariaDB [test]> select order_num,sum(quantity*item_price) as ordertotal from orderitems group by order_num order by ordertotal;
+-----------+------------+
| order_num | ordertotal |
+-----------+------------+
|     20009 |      38.47 |
|     20006 |      55.00 |
|     20008 |     125.00 |
|     20005 |     149.87 |
|     20007 |    1000.00 |
+-----------+------------+
5 rows in set (0.00 sec)

MariaDB [test]> select order_num,sum(quantity*item_price) as ordertotal from orderitems group by order_num having ordertotal >= 50 order by ordertotal;
+-----------+------------+
| order_num | ordertotal |
+-----------+------------+
|     20006 |      55.00 |
|     20008 |     125.00 |
|     20005 |     149.87 |
|     20007 |    1000.00 |
+-----------+------------+
4 rows in set (0.00 sec)

MariaDB [test]> select order_num,sum(quantity*item_price) as ordertotal from orderitems group by order_num having sum(quantity*item_price) >= 50 order by sum(quantity*item_price);
+-----------+------------+
| order_num | ordertotal |
+-----------+------------+
|     20006 |      55.00 |
|     20008 |     125.00 |
|     20005 |     149.87 |
|     20007 |    1000.00 |
+-----------+------------+
4 rows in set (0.00 sec)
```

在这个例子中, `GROUP BY` 子句用来按订单号( `order_num` 列)
分组数据,以便 `SUM(*)` 函数能够返回总计订单价格。 `HAVING` 子
句过滤数据,使得只返回总计订单价格大于等于 50 的订单。最后,用 `ORDER BY` 子句排序输出。

> SELECT 子句顺序

下面回顾一下 SELECT 语句中子句的顺序。下表以在 SELECT 语句中
使用时必须遵循的次序,列出迄今为止所学过的子句。

|SELECT子句|说明|是否必须使用|
|SELECT|要返回的列或表达式|是|
|FROM|从中检索数据的表|仅在从表选择数据时使用|
|WHERE|行级过滤|否|
|GROUP BY|分组说明|仅在按组计算聚集时使用|
|HAVING|组级过滤|否|
|ORDER BY|输出排序顺序|否|
|LIMIT|要检索的行数|否|

> 小结
>> 在本章中,我们学习了如何用SQL聚集函数对数据进行汇总计算。
本章讲授了如何使用 GROUP BY 子句对数据组进行这些汇总计算,返回每
个组的结果。我们看到了如何使用 HAVING 子句过滤特定的组,还知道了
ORDER BY 和 GROUP BY 之间以及 WHERE 和 HAVING 之间的差异。

---

#### 使用子查询

本章介绍什么是子查询以及如何使用它们。

> 子查询
**版本要求** MySQL 4.1引入了对子查询的支持,所以要想使用
本章描述的SQL,必须使用MySQL 4.1或更高级的版本。

**SELECT语句** 是SQL的查询。迄今为止我们所看到的所有 SELECT 语句
都是简单查询,即从单个数据库表中检索数据的单条语句。

**查询(query)** 任何SQL语句都是查询。但此术语一般指 SELECT语句。

SQL还允许创建子查询(subquery),即嵌套在其他查询中的查询。
为什么要这样做呢?理解这个概念的最好方法是考察几个例子。

> 利用子查询进行过滤

订单信息存储在两个表中。对于包含订单号、客户ID、订单日期的每个订单, orders 表存储一行。
各订单的物品存储在相关的orderitems 表中。 orders 表不存储客户信息。它只存储客户的ID。
实际的客户信息存储在 customers 表中。

>> 现在,假如需要列出订购物品 TNT2 的所有客户,应该怎样检索?下面列出具体的步骤。

(1) 检索包含物品 TNT2 的所有订单的编号。

(2) 检索具有前一步骤列出的订单编号的所有客户的ID。

(3) 检索前一步骤返回的所有客户ID的客户信息。

上述每个步骤都可以单独作为一个查询来执行。可以把一条 SELECT
语句返回的结果用于另一条 SELECT 语句的 WHERE 子句。
也可以使用子查询来把3个查询组合成一条语句

```shell
MariaDB [test]> select order_num from orderitems where prod_id='TNT2';
+-----------+
| order_num |
+-----------+
|     20005 |
|     20007 |
+-----------+
2 rows in set (0.00 sec)

MariaDB [test]> select cust_id from orders where order_num = 20005 or order_num = 20007;
+---------+
| cust_id |
+---------+
|   10001 |
|   10004 |
+---------+
2 rows in set (0.00 sec)

MariaDB [test]> select cust_name from customers where cust_id in (10001,10004);
+----------------+
| cust_name      |
+----------------+
| Coyote Inc.    |
| Yosemite Place |
+----------------+
2 rows in set (0.00 sec)

MariaDB [test]> select cust_name from customers where cust_id in
> (
> select cust_id from orders where order_num in
> (select order_num from orderitems where prod_id='TNT2')
> );
+----------------+
| cust_name      |
+----------------+
| Coyote Inc.    |
| Yosemite Place |
+----------------+
2 rows in set (0.00 sec)
```

可见,在 WHERE 子句中使用子查询能够编写出功能很强并且很灵活的
SQL语句。对于能嵌套的子查询的数目没有限制,不过在实际使用时由于
性能的限制,不能嵌套太多的子查询。

**列必须匹配** 在 WHERE 子句中使用子查询(如这里所示),应
该保证 SELECT 语句具有与 WHERE 子句中相同数目的列。通常,
子查询将返回单个列并且与单个列匹配,但如果需要也可以
使用多个列。

虽然子查询一般与 `IN `操作符结合使用,但也可以用于测试等于(` =` )、不等于( `<>` )等。

**子查询和性能** 这里给出的代码有效并获得所需的结果。但
是,使用子查询并不总是执行这种类型的数据检索的最有效
的方法。更多的论述,请参阅下一章,其中将再次给出这个
例子。

> 作为计算字段使用子查询

使用子查询的另一方法是创建计算字段。

>> 查询每个客户的姓名`cust_name`和状态`cust_state`以及每个客户的订单总数`orders`。订单与相应的客户ID存储在 orders 表中，客户信息存储在customers表中。

为了执行这个操作,遵循下面的步骤。

(1) 从 customers 表中检索客户列表。

(2) 对于检索出的每个客户,统计其在 orders 表中的订单数目。

正如前两章所述,可使用 SELECT COUNT ( *) 对表中的行进行计数,并
且通过提供一条 WHERE 子句来过滤某个特定的客户ID,可仅对该客户的订单进行计数。

```shell
MariaDB [test]> select cust_name,cust_state,(select count(*) from orders where orders.cust_id=customers.cust_id) as orders from customers order by cust_name;
+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         |      2 |
| E Fudd         | IL         |      1 |
| Mouse House    | OH         |      0 |
| Wascals        | IN         |      1 |
| Yosemite Place | AZ         |      1 |
+----------------+------------+--------+
5 rows in set (0.00 sec)
```

`cust_name` 、` cust_state` 和 `orders` 。 `orders` 是一个计算字段,
它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一
次。在此例子中,该子查询执行了5次,因为检索出了5个客户。

分析子查询中的 WHERE 子句与前面使用的 WHERE 子句稍有不同,因为它使
用了完全限定列名(在前面提到过)。

**相关子查询(correlated subquery)** 涉及外部查询的子查询。
这种类型的子查询称为相关子查询。任何时候只要列名可能有多义
性,就必须使用这种语法(表名和列名由一个句点分隔)。为什么这样?

>> 我们来看看如果不使用完全限定的列名会发生什么情况

```shell
MariaDB [test]> select cust_name,cust_state,(select count(*) from orders where cust_id=cust_id) as orders from customers order by cust_name;+----------------+------------+--------+
| cust_name      | cust_state | orders |
+----------------+------------+--------+
| Coyote Inc.    | MI         |      5 |
| E Fudd         | IL         |      5 |
| Mouse House    | OH         |      5 |
| Wascals        | IN         |      5 |
| Yosemite Place | AZ         |      5 |
+----------------+------------+--------+
5 rows in set (0.00 sec)
```

显然,返回的结果不正确(请比较前面的结果),那么,为什么会这样呢?
有两个 `cust_id` 列,一个在 `customers` 中,另一个在
`orders` 中,需要比较这两个列以正确地把订单与它们相应的顾客匹配。
如果不完全限定列名,MySQL将假定你是对 `orders` 表中的 `cust_id` 进行
自身比较。而 `SELECT COUNT(*) FROM orders WHERE cust_id = cust_id;`
总是返回 `orders` 表中的订单总数(因为MySQL查看每个订单的 `cust_id`
是否与本身匹配,当然,它们总是匹配的)。

虽然子查询在构造这种 SELECT 语句时极有用,但必须注意限制有歧
义性的列名。

**不止一种解决方案** 正如本章前面所述,虽然这里给出的样
例代码运行良好,但它并不是解决这种数据检索的最有效的
方法。在后面的章节中我们还要遇到这个例子。

**逐渐增加子查询来建立查询** 用子查询测试和调试查询很有
技巧性,特别是在这些语句的复杂性不断增加的情况下更是如
此。用子查询建立(和测试)查询的最可靠的方法是逐渐进行,
这与MySQL处理它们的方法非常相同。首先,建立和测试最
内层的查询。然后,用硬编码数据建立和测试外层查询,并且
仅在确认它正常后才嵌入子查询。这时,再次测试它。对于要
增加的每个查询,重复这些步骤。这样做仅给构造查询增加了
一点点时间,但节省了以后(找出查询为什么不正常)的大量
时间,并且极大地提高了查询一开始就正常工作的可能性。

> 小结
>> 本章学习了什么是子查询以及如何使用它们。子查询最常见的使用
是在 WHERE 子句的 IN 操作符中,以及用来填充计算列。我们举了这两种操
作类型的例子。

---

#### 联结表

本章将介绍什么是联结,为什么要使用联结,如何编写使用联结的
SELECT 语句。

> 联结

SQL最强大的功能之一就是能在数据检索查询的执行中联结(join)
表。联结是利用SQL的 SELECT 能执行的最重要的操作,很好地理解联结
及其语法是学习SQL的一个极为重要的组成部分。

在能够有效地使用联结前,必须了解关系表以及关系数据库设计的
一些基础知识。下面的介绍并不是这个内容的全部知识,但作为入门已
经足够了。

1.关系表

理解关系表的最好方法是来看一个现实世界中的例子。
假如有一个包含产品目录的数据库表,其中每种类别的物品占一行。
对于每种物品要存储的信息包括产品描述和价格,以及生产该产品的供
应商信息。

现在,假如有由同一供应商生产的多种物品,那么在何处存储供应
商信息(如,供应商名、地址、联系方法等)呢?将这些数据与产品信
息分开存储的理由如下。

* 因为同一供应商生产的每个产品的供应商信息都是相同的,对每
个产品重复此信息既浪费时间又浪费存储空间。
* 如果供应商信息改变(例如,供应商搬家或电话号码变动)
,只需改动一次即可。
* 如果有重复数据(即每种产品都存储供应商信息),很难保证每次输入该数据的方式都相同。
不一致的数据在报表中很难利用。

关键是,相同数据出现多次决不是一件好事,此因素是关系数据库
设计的基础。关系表的设计就是要保证把信息分解成多个表,一类数据
一个表。各表通过某些常用的值(即关系设计中的关系(relational))互
相关联。

在这个例子中,可建立两个表,一个存储供应商信息,另一个存储
产品信息。 vendors 表包含所有供应商信息,每个供应商占一行,每个供
应商具有唯一的标识。此标识称为主键(primary key)(在第1章中首次
提到),可以是供应商ID或任何其他唯一值。

products 表只存储产品信息,它除了存储供应商ID( vendors 表的主
键)外不存储其他供应商信息。 vendors 表的主键又叫作 products 的外键,
它将 vendors 表与 products 表关联,利用供应商ID能从 vendors 表中找出
相应供应商的详细信息。

**外键(foreign key)** 外键为某个表中的一列,它包含另一个表的主键值,定义了两个表之间的关系。

这样做的好处如下:

* 供应商信息不重复,从而不浪费时间和空间;
* 如果供应商信息变动,可以只更新 vendors 表中的单个记录,相
关表中的数据不用改动;
* 由于数据无重复,显然数据是一致的,这使得处理数据更简单。

总之,关系数据可以有效地存储和方便地处理。因此,关系数据库
的可伸缩性远比非关系数据库要好。

**可伸缩性(scale)** 能够适应不断增加的工作量而不失败。设
计良好的数据库或应用程序称之为可伸缩性好(scale well)。

2.为什么要使用联结

正如所述,分解数据为多个表能更有效地存储,更方便地处理,并
且具有更大的可伸缩性。但这些好处是有代价的。

如果数据存储在多个表中,怎样用单条 SELECT 语句检索出数据?

答案是使用联结。简单地说,联结是一种机制,用来在一条 SELECT
语句中关联表,因此称之为联结。使用特殊的语法,可以联结多个表返
回一组输出,联结在运行时关联表中正确的行。

**维护引用完整性** 重要的是,要理解联结不是物理实体。换句
话说,它在实际的数据库表中不存在。联结由MySQL根据需
要建立,它存在于查询的执行当中。
在使用关系表时,仅在关系列中插入合法的数据非常重要。回
到这里的例子,如果在 products 表中插入拥有非法供应商ID
(即没有在 vendors 表中出现)的供应商生产的产品,则这些
产品是不可访问的,因为它们没有关联到某个供应商。
为防止这种情况发生,可指示MySQL只允许在 products 表的
供应商ID列中出现合法值(即出现在 vendors 表中的供应商)。
这就是维护引用完整性,它是通过在表的定义中指定主键和外
键来实现的。


> 创建联结

联结的创建非常简单,规定要联结的所有表以及它们如何关联即可。

>> 检索出所有的供应商名`vend_name`，以及供应商提供的商品名`prod_name`和价格`prod_price`。

```shell
MariaDB [test]> describe vendors;
+--------------+----------+------+-----+---------+----------------+
| Field        | Type     | Null | Key | Default | Extra          |
+--------------+----------+------+-----+---------+----------------+
| vend_id      | int(11)  | NO   | PRI | NULL    | auto_increment |
| vend_name    | char(50) | NO   |     | NULL    |                |
| vend_address | char(50) | YES  |     | NULL    |                |
| vend_city    | char(50) | YES  |     | NULL    |                |
| vend_state   | char(5)  | YES  |     | NULL    |                |
| vend_zip     | char(10) | YES  |     | NULL    |                |
| vend_country | char(50) | YES  |     | NULL    |                |
+--------------+----------+------+-----+---------+----------------+
7 rows in set (0.00 sec)

MariaDB [test]> describe products;
+------------+--------------+------+-----+---------+-------+
| Field      | Type         | Null | Key | Default | Extra |
+------------+--------------+------+-----+---------+-------+
| prod_id    | char(10)     | NO   | PRI | NULL    |       |
| vend_id    | int(11)      | NO   | MUL | NULL    |       |
| prod_name  | char(255)    | NO   |     | NULL    |       |
| prod_price | decimal(8,2) | NO   |     | NULL    |       |
| prod_desc  | text         | YES  |     | NULL    |       |
+------------+--------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

MariaDB [test]> select vend_name,prod_name,prod_price from vendors,products where vendors.vend_id = products.vend_id order by vend_name,prod_name;
+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Detonator      |      13.00 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
+-------------+----------------+------------+
14 rows in set (0.00 sec)
```

**完全限定列名** 在引用的列可能出现二义性时,必须使用完
全限定列名(用一个点分隔的表名和列名)。如果引用一个
没有用表名限制的具有二义性的列名,MySQL将返回错误。

1.WHERE子句的重要性

利用 WHERE 子句建立联结关系似乎有点奇怪,但实际上,有一个很充
分的理由。请记住,在一条 SELECT 语句中联结几个表时,相应的关系是
在运行中构造的。在数据库表的定义中不存在能指示MySQL如何对表进
行联结的东西。你必须自己做这件事情。在联结两个表时,你实际上做
的是将第一个表中的每一行与第二个表中的每一行配对。 WHERE 子句作为
过滤条件,它只包含那些匹配给定条件(这里是联结条件)的行。没有
WHERE 子句,第一个表中的每个行将与第二个表中的每个行配对,而不管
它们逻辑上是否可以配在一起。

**笛卡儿积(cartesian product)** 由没有联结条件的表关系返回
的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘
以第二个表中的行数。

为理解这一点,请看下面的 SELECT 语句及其输出:

```shell
MariaDB [test]> select vend_name,prod_name,prod_price from vendors,products order by vend_name,prod_name;
+----------------+----------------+------------+
| vend_name      | prod_name      | prod_price |
+----------------+----------------+------------+
| ACME           | .5 ton anvil   |       5.99 |
| ACME           | 1 ton anvil    |       9.99 |
| ACME           | 2 ton anvil    |      14.99 |
| ACME           | Bird seed      |      10.00 |
| ACME           | Carrots        |       2.50 |
| ACME           | Detonator      |      13.00 |
| ACME           | Fuses          |       3.42 |
| ACME           | JetPack 1000   |      35.00 |
| ACME           | JetPack 2000   |      55.00 |
| ACME           | Oil can        |       8.99 |
| ACME           | Safe           |      50.00 |
| ACME           | Sling          |       4.49 |
| ACME           | TNT (1 stick)  |       2.50 |
| ACME           | TNT (5 sticks) |      10.00 |
| Anvils R Us    | .5 ton anvil   |       5.99 |
| Anvils R Us    | 1 ton anvil    |       9.99 |
| Anvils R Us    | 2 ton anvil    |      14.99 |
| Anvils R Us    | Bird seed      |      10.00 |
| Anvils R Us    | Carrots        |       2.50 |
| Anvils R Us    | Detonator      |      13.00 |
| Anvils R Us    | Fuses          |       3.42 |
| Anvils R Us    | JetPack 1000   |      35.00 |
| Anvils R Us    | JetPack 2000   |      55.00 |
| Anvils R Us    | Oil can        |       8.99 |
| Anvils R Us    | Safe           |      50.00 |
| Anvils R Us    | Sling          |       4.49 |
| Anvils R Us    | TNT (1 stick)  |       2.50 |
| Anvils R Us    | TNT (5 sticks) |      10.00 |
| Furball Inc.   | .5 ton anvil   |       5.99 |
| Furball Inc.   | 1 ton anvil    |       9.99 |
| Furball Inc.   | 2 ton anvil    |      14.99 |
| Furball Inc.   | Bird seed      |      10.00 |
| Furball Inc.   | Carrots        |       2.50 |
| Furball Inc.   | Detonator      |      13.00 |
| Furball Inc.   | Fuses          |       3.42 |
| Furball Inc.   | JetPack 1000   |      35.00 |
| Furball Inc.   | JetPack 2000   |      55.00 |
| Furball Inc.   | Oil can        |       8.99 |
| Furball Inc.   | Safe           |      50.00 |
| Furball Inc.   | Sling          |       4.49 |
| Furball Inc.   | TNT (1 stick)  |       2.50 |
| Furball Inc.   | TNT (5 sticks) |      10.00 |
| Jet Set        | .5 ton anvil   |       5.99 |
| Jet Set        | 1 ton anvil    |       9.99 |
| Jet Set        | 2 ton anvil    |      14.99 |
| Jet Set        | Bird seed      |      10.00 |
| Jet Set        | Carrots        |       2.50 |
| Jet Set        | Detonator      |      13.00 |
| Jet Set        | Fuses          |       3.42 |
| Jet Set        | JetPack 1000   |      35.00 |
| Jet Set        | JetPack 2000   |      55.00 |
| Jet Set        | Oil can        |       8.99 |
| Jet Set        | Safe           |      50.00 |
| Jet Set        | Sling          |       4.49 |
| Jet Set        | TNT (1 stick)  |       2.50 |
| Jet Set        | TNT (5 sticks) |      10.00 |
| Jouets Et Ours | .5 ton anvil   |       5.99 |
| Jouets Et Ours | 1 ton anvil    |       9.99 |
| Jouets Et Ours | 2 ton anvil    |      14.99 |
| Jouets Et Ours | Bird seed      |      10.00 |
| Jouets Et Ours | Carrots        |       2.50 |
| Jouets Et Ours | Detonator      |      13.00 |
| Jouets Et Ours | Fuses          |       3.42 |
| Jouets Et Ours | JetPack 1000   |      35.00 |
| Jouets Et Ours | JetPack 2000   |      55.00 |
| Jouets Et Ours | Oil can        |       8.99 |
| Jouets Et Ours | Safe           |      50.00 |
| Jouets Et Ours | Sling          |       4.49 |
| Jouets Et Ours | TNT (1 stick)  |       2.50 |
| Jouets Et Ours | TNT (5 sticks) |      10.00 |
| LT Supplies    | .5 ton anvil   |       5.99 |
| LT Supplies    | 1 ton anvil    |       9.99 |
| LT Supplies    | 2 ton anvil    |      14.99 |
| LT Supplies    | Bird seed      |      10.00 |
| LT Supplies    | Carrots        |       2.50 |
| LT Supplies    | Detonator      |      13.00 |
| LT Supplies    | Fuses          |       3.42 |
| LT Supplies    | JetPack 1000   |      35.00 |
| LT Supplies    | JetPack 2000   |      55.00 |
| LT Supplies    | Oil can        |       8.99 |
| LT Supplies    | Safe           |      50.00 |
| LT Supplies    | Sling          |       4.49 |
| LT Supplies    | TNT (1 stick)  |       2.50 |
| LT Supplies    | TNT (5 sticks) |      10.00 |
+----------------+----------------+------------+
84 rows in set (0.00 sec)
```

从上面的输出中可以看到,相应的笛卡儿积不是我们所想要
的。这里返回的数据用每个供应商匹配了每个产品,它包括了
供应商不正确的产品。实际上有的供应商根本就没有产品。
分析

**不要忘了 WHERE 子句** 应该保证所有联结都有 WHERE 子句,否
则MySQL将返回比想要的数据多得多的数据。同理,应该保
证 WHERE 子句的正确性。不正确的过滤条件将导致MySQL返回
不正确的数据。

**叉联结** 有时我们会听到返回称为叉联结(cross join)的笛卡
儿积的联结类型


2.内部联结

目前为止所用的联结称为等值联结(equijoin),它基于两个表之间的
相等测试。这种联结也称为内部联结。其实,对于这种联结可以使用稍
微不同的语法来明确指定联结的类型。下面的 SELECT 语句返回与前面例
子完全相同的数据:

```shell
MariaDB [test]> select vend_name,prod_name,prod_price from vendors inner join products on vendors.vend_id = products.vend_id;
+-------------+----------------+------------+
| vend_name   | prod_name      | prod_price |
+-------------+----------------+------------+
| Anvils R Us | .5 ton anvil   |       5.99 |
| Anvils R Us | 1 ton anvil    |       9.99 |
| Anvils R Us | 2 ton anvil    |      14.99 |
| LT Supplies | Fuses          |       3.42 |
| LT Supplies | Oil can        |       8.99 |
| ACME        | Detonator      |      13.00 |
| ACME        | Bird seed      |      10.00 |
| ACME        | Carrots        |       2.50 |
| ACME        | Safe           |      50.00 |
| ACME        | Sling          |       4.49 |
| ACME        | TNT (1 stick)  |       2.50 |
| ACME        | TNT (5 sticks) |      10.00 |
| Jet Set     | JetPack 1000   |      35.00 |
| Jet Set     | JetPack 2000   |      55.00 |
+-------------+----------------+------------+
14 rows in set (0.00 sec)
```

此语句中的 `SELECT` 与前面的` SELECT` 语句相同,但 `FROM` 子句不
同。这里,两个表之间的关系是 `FROM` 子句的组成部分,以 `INNER JOIN` 指定。
在使用这种语法时,联结条件用特定的 `ON` 子句而不是 `WHERE`
子句给出。传递给 `ON` 的实际条件与传递给 `WHERE` 的相同。

**使用哪种语法** ANSI SQL规范首选 `INNER JOIN` 语法。此外,
尽管使用 `WHERE` 子句定义联结的确比较简单,但是使用明确的
联结语法能够确保不会忘记联结条件,有时候这样做也能影响
性能。

3.联结多个表

SQL对一条 SELECT 语句中可以联结的表的数目没有限制。创建联结
的基本规则也相同。首先列出所有表,然后定义表之间的关系。例如:

>> 检索出订单号为20005的所有商品的商品名称，商品的供应商名称，商品的价格，商品的数量（`prod_name,vend_name,prod_price,quantit`）。

```shell
MariaDB [test]> select prod_name,vend_name,prod_price,quantity from orderitems,products,vendors where products.vend_id = vendors.vend_id and orderitems.prod_id  = products.prod_id and order_num = 20005;
+----------------+-------------+------------+----------+
| prod_name      | vend_name   | prod_price | quantity |
+----------------+-------------+------------+----------+
| .5 ton anvil   | Anvils R Us |       5.99 |       10 |
| 1 ton anvil    | Anvils R Us |       9.99 |        3 |
| TNT (5 sticks) | ACME        |      10.00 |        5 |
| Bird seed      | ACME        |      10.00 |        1 |
+----------------+-------------+------------+----------+
4 rows in set (0.00 sec)
```

此例子显示编号为 20005 的订单中的物品。订单物品存储在
orderitems 表中。每个产品按其产品ID存储,它引用 products
表中的产品。这些产品通过供应商ID联结到 vendors 表中相应的供应商,
供应商ID存储在每个产品的记录中。这里的 FROM 子句列出了3个表,而
WHERE 子句定义了这两个联结条件,而第三个联结条件用来过滤出订单
20005 中的物品。

**性能考虑** MySQL在运行时关联指定的每个表以处理联结。
这种处理可能是非常耗费资源的,因此应该仔细,不要联结
不必要的表。联结的表越多,性能下降越厉害。

现在可以回顾一下前面章中的例子了。该例子如下所示

>> 返回订购产品 TNT2 的客户列表

```shell
MariaDB [test]> select cust_name from customers where cust_id in (select cust_id from orders where order_num in (select order_num from orderitems where prod_id='TNT2'));
+----------------+
| cust_name      |
+----------------+
| Coyote Inc.    |
| Yosemite Place |
+----------------+
2 rows in set (0.00 sec)
```

子查询并不总是执行复杂 SELECT 操作的最有效的
方法,下面是使用联结的相同查询:

```shell
MariaDB [test]> select cust_name from customers where cust_id in (select cust_id from orders where order_num in (select order_num from orderitems where prod_id='TNT2'));
+----------------+
| cust_name      |
+----------------+
| Coyote Inc.    |
| Yosemite Place |
+----------------+
2 rows in set (0.00 sec)
```

这个查询中返回数据需要使用3个表。但这里
我们没有在嵌套子查询中使用它们,而是使用了两个联结。这
里有3个 WHERE 子句条件。前两个关联联结中的表,后一个过滤产品 TNT2
的数据。

**多做实验** 正如所见,为执行任一给定的SQL操作,一般存在
不止一种方法。很少有绝对正确或绝对错误的方法。性能可能
会受操作类型、表中数据量、是否存在索引或键以及其他一些
条件的影响。因此,有必要对不同的选择机制进行实验,以找
出最适合具体情况的方法。

> 小结
>> 联结是SQL中最重要最强大的特性,有效地使用联结需要对关系数据
库设计有基本的了解。本章随着对联结的介绍讲述了关系数据库设计的
一些基本知识,包括等值联结(也称为内部联结)这种最经常使用的联
结形式。下一章将介绍如何创建其他类型的联结。

---

#### 创建高级联结

本章将讲解另外一些联结类型(包括它们的含义和使用方法),介
绍如何对被联结的表使用表别名和聚集函数。

> 使用表别名

前面章节中介绍了如何使用别名引用被检索的表列。给列起别名的语
法如下:

>> 检索出供应商的名字和国家，并以指定格式显示【`vend_name`(`vend_country`)】

```shell
MariaDB [test]> select concat(rtrim(vend_name),'(',rtrim(vend_country),')') as vend_tile from vendors order by vend_name;
+------------------------+
| vend_tile              |
+------------------------+
| ACME(USA)              |
| Anvils R Us(USA)       |
| Furball Inc.(USA)      |
| Jet Set(England)       |
| Jouets Et Ours(France) |
| LT Supplies(USA)       |
+------------------------+
6 rows in set (0.00 sec)
```

别名除了用于列名和计算字段外, SQL还允许给表名起别名。这样做
有两个主要理由:
* 缩短SQL语句;
* 允许在单条 SELECT 语句中多次使用相同的表。

请看下面的 SELECT 语句。它与前一章的例子中所用的语句基本相同,
但改成了使用别名:

>> 返回订购产品 TNT2 的客户列表

```shell
MariaDB [test]> select cust_name,cust_contact from customers as c,orders as o,orderitems as oi where c.cust_id = o.cust_id and oi.order_num = o.order_num and prod_id = 'TNT2';
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
2 rows in set (0.00 sec)
```

可以看到, FROM 子句中3个表全都具有别名。 customers AS c
建立 c 作为 customers 的别名,等等。这使得能使用省写的 c 而
不是全名 customers 。在此例子中,表别名只用于 WHERE 子句。但是,表
别名不仅能用于 WHERE 子句,它还可以用于 SELECT 的列表、 ORDER BY 子句
以及语句的其他部分。

应该注意,表别名只在查询执行中使用。与列别名不一样,表别名
不返回到客户机。

> 使用不同类型的联结

迄今为止,我们使用的只是称为内部联结或等值联结(equijoin)的简
单联结。现在来看3种其他联结,它们分别是自联结、自然联结和外部联结。

1.自联结

如前所述,使用表别名的主要原因之一是能在单条 SELECT 语句中不
止一次引用相同的表。下面举一个例子。

假如你发现某物品(其ID为 DTNTR )存在问题,因此想知道生产该物
品的供应商生产的其他物品是否也存在这些问题。此查询要求首先找到
生产ID为 DTNTR 的物品的供应商,然后找出这个供应商生产的其他物品。
下面是解决此问题的一种方法:

>> 查找商品  `prod_id ='DTNTR'` 的供应商生产的商品名和id号`prod_id,prod_name`

```shell
MariaDB [test]> select prod_id,prod_name from products where vend_id = (select vend_id from products where prod_id ='DTNTR');
+---------+----------------+
| prod_id | prod_name      |
+---------+----------------+
| DTNTR   | Detonator      |
| FB      | Bird seed      |
| FC      | Carrots        |
| SAFE    | Safe           |
| SLING   | Sling          |
| TNT1    | TNT (1 stick)  |
| TNT2    | TNT (5 sticks) |
+---------+----------------+
7 rows in set (0.00 sec)
```

这是第一种解决方案,它使用了子查询。内部的 `SELECT` 语句做
了一个 简单的检索 ,返回生产 ID`prod_id`为 `DTNTR` 的 物品供应商 的
`vend_id` 。该ID用于外部查询的 `WHERE` 子句中,以便检索出这个供应商生
产的所有物品

现在来看使用联结的相同查询:

```shell
MariaDB [test]> select p1.prod_id,p1.prod_name from products as p1,products as p2 where p1.vend_id = p2.vend_id and p2.prod_id = 'DTNTR';
+---------+----------------+
| prod_id | prod_name      |
+---------+----------------+
| DTNTR   | Detonator      |
| FB      | Bird seed      |
| FC      | Carrots        |
| SAFE    | Safe           |
| SLING   | Sling          |
| TNT1    | TNT (1 stick)  |
| TNT2    | TNT (5 sticks) |
+---------+----------------+
7 rows in set (0.00 sec)
```

此查询中需要的两个表实际上是相同的表,因此 `products` 表在
`FROM` 子句中出现了两次。虽然这是完全合法的,但对 `products`
的引用具有二义性,因为MySQL不知道你引用的是 `products` 表中的哪个
实例。

为解决此问题,使用了表别名。 `products` 的第一次出现为别名 `p1` ,
第二次出现为别名 `p2 `。现在可以将这些别名用作表名。例如, `SELECT `语
句使用 `p1` 前缀明确地给出所需列的全名。如果不这样,`MySQL`将返回错
误,因为分别存在两个名为` prod_id` 、 `prod_name` 的列。MySQL不知道想
要的是哪一个列(即使它们事实上是同一个列)。 `WHERE` (通过匹配 `p1` 中
的 `vend_id` 和 `p2` 中的 `vend_id` )首先联结两个表,然后按第二个表中的
`prod_id` 过滤数据,返回所需的数据。


**用自联结而不用子查询** 自联结通常作为外部语句用来替代
从相同表中检索数据时使用的子查询语句。虽然最终的结果是
相同的,但有时候处理联结远比处理子查询快得多。应该试一
下两种方法,以确定哪一种的性能更好。

2.自然联结

无论何时对表进行联结,应该至少有一个列出现在不止一个表中(被
联结的列)。标准的联结(前一章中介绍的内部联结)返回所有数据,甚
至相同的列多次出现。自然联结排除多次出现,使每个列只返回一次。

怎样完成这项工作呢?答案是,系统不完成这项工作,由你自己完
成它。自然联结是这样一种联结,其中你只能选择那些唯一的列。
这一般是通过对表使用通配符( SELECT * ),
对所有其他表的列使用明确的子集来完成的。下面举一个例子:

```shell
MariaDB [test]> select c.*,o.order_num,o.order_date,oi.prod_id,oi.quantity,oi.item_price from customers as c,orders as o,orderitems as oi where c.cust_id=o.cust_id and oi.order_num=o.order_num and prod_id='FB';
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
| cust_id | cust_name   | cust_address   | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email      | order_num | order_date          | prod_id | quantity | item_price |
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
|   10001 | Coyote Inc. | 200 Maple Lane | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com |     20005 | 2005-09-01 00:00:00 | FB      |        1 |      10.00 |
|   10001 | Coyote Inc. | 200 Maple Lane | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com |     20009 | 2005-10-08 00:00:00 | FB      |        1 |      10.00 |
+---------+-------------+----------------+-----------+------------+----------+--------------+--------------+-----------------+-----------+---------------------+---------+----------+------------+
2 rows in set (0.00 sec)
```

在这个例子中,通配符只对第一个表使用。所有其他列明确列
出,所以没有重复的列被检索出来。

事实上,迄今为止我们建立的每个内部联结都是自然联结,很可能
我们永远都不会用到不是自然联结的内部联结。

3.外部联结

许多联结将一个表中的行与另一个表中的行相关联。但有时候会需
要包含没有关联行的那些行。例如,可能需要使用联结来完成以下工作:

* 对每个客户下了多少订单进行计数,包括那些至今尚未下订单的客户;
* 列出所有产品以及订购数量,包括没有人订购的产品;
* 计算平均销售规模,包括那些至今尚未下订单的客户。

在上述例子中,联结包含了那些在相关表中没有关联行的行。这种
类型的联结称为外部联结。

下面的 SELECT 语句给出一个简单的内部联结。

>> 它检索所有客户及其订单

```shell
MariaDB [test]> select customers.cust_id,orders.order_num from customers inner join orders on customers.cust_id=orders.cust_id;
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10001 |     20009 |
|   10003 |     20006 |
|   10004 |     20007 |
|   10005 |     20008 |
+---------+-----------+
5 rows in set (0.00 sec)
```

>> 外部联结语法类似。为了检索所有客户,包括那些没有订单的客户,可如下进行:

```shell
MariaDB [test]> select customers.cust_id,orders.order_num from customers left outer join orders on customers.cust_id=orders.cust_id;
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10001 |     20009 |
|   10002 |      NULL |
|   10003 |     20006 |
|   10004 |     20007 |
|   10005 |     20008 |
+---------+-----------+
6 rows in set (0.00 sec)
```

类似于上一章中所看到的内部联结,这条 `SELECT` 语句使用了关
键字 `OUTER JOIN` 来指定联结的类型(而不是在 `WHERE` 子句中指
定)。但是,与内部联结关联两个表中的行不同的是,外部联结还包括没
有关联行的行。在使用 `OUTER JOIN` 语法时,必须使用 `RIGHT` 或 `LEFT` 关键字
指定包括其所有行的表( `RIGHT` 指出的是 `OUTER JOIN` 右边的表,而 `LEFT`
指出的是 `OUTER JOIN` 左边的表)。

上面的例子使用 `LEFT OUTER JOIN` 从 `FROM`子句的左边表( `customers` 表)中选择所有行。为了从右边的表中选择所
有行,应该使用 `RIGHT OUTER JOIN` ,如下例所示:

```shell
MariaDB [test]> select customers.cust_id,orders.order_num from customers right outer join orders on orders.cust_id = customers.cust_id;
+---------+-----------+
| cust_id | order_num |
+---------+-----------+
|   10001 |     20005 |
|   10001 |     20009 |
|   10003 |     20006 |
|   10004 |     20007 |
|   10005 |     20008 |
+---------+-----------+
5 rows in set (0.00 sec)
```

**没有 `*=` 操作符** MySQL不支持简化字符 `*=` 和 `=*` 的使用,这两
种操作符在其他DBMS中是很流行的。

**外部联结的类型** 存在两种基本的外部联结形式:左外部联结
和右外部联结。它们之间的唯一差别是所关联的表的顺序不
同。换句话说,左外部联结可通过颠倒 FROM 或 WHERE 子句中
表的顺序转换为右外部联结。因此,两种类型的外部联结可互
换使用,而究竟使用哪一种纯粹是根据方便而定。


> 使用带聚集函数的联结

正如第12章所述,聚集函数用来汇总数据。虽然至今为止聚集函数
的所有例子只是从单个表汇总数据,但这些函数也可以与联结一起使用。
为说明这一点,请看一个例子。

>> 如果要检索所有客户及每个客户所下的订单数,下面使用了 COUNT() 函数的代码可完成此工作:

```shell
MariaDB [test]> select customers.cust_name,customers.cust_id,count(orders.order_num) as num_ord from customers inner join orders on customers.cust_id = orders.cust_id group by customers.cust_id;
+----------------+---------+---------+
| cust_name      | cust_id | num_ord |
+----------------+---------+---------+
| Coyote Inc.    |   10001 |       2 |
| Wascals        |   10003 |       1 |
| Yosemite Place |   10004 |       1 |
| E Fudd         |   10005 |       1 |
+----------------+---------+---------+
4 rows in set (0.00 sec)
```

此` SELECT` 语句使用 `INNER JOIN` 将 `customers` 和 `orders` 表互相关联。
`GROUP BY` 子句按客户分组数据,因此,函数调用 `COUNT (orders.order_num) `
对每个客户的订单计数,将它作为 `num_ord` 返回。

```shell
MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus  | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
5 rows in set (0.00 sec)

MariaDB [test]> select  * from orders;
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
|     20006 | 2005-09-12 00:00:00 |   10003 |
|     20007 | 2005-09-30 00:00:00 |   10004 |
|     20008 | 2005-10-03 00:00:00 |   10005 |
|     20009 | 2005-10-08 00:00:00 |   10001 |
+-----------+---------------------+---------+
5 rows in set (0.00 sec)
# 内部联结或等值联结(equijoin)的简单联结
MariaDB [test]> select orders.*,customers.* from customers inner join orders on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
5 rows in set (0.00 sec)
# 内部联结或等值联结(equijoin)的简单联结
MariaDB [test]> select orders.*,customers.* from orders inner join customers on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
5 rows in set (0.00 sec)
# 左外部联结
MariaDB [test]> select orders.*,customers.* from customers left outer join orders on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|      NULL | NULL                |    NULL |   10002 | Mouse House    | 333 Fromage Lane    | Columbus  | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
6 rows in set (0.00 sec)
# 左外部联结
MariaDB [test]> select orders.*,customers.* from orders left outer join customers on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
5 rows in set (0.00 sec)
# 右外部联结
MariaDB [test]> select orders.*,customers.* from customers right outer join orders on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
5 rows in set (0.00 sec)
# 右外部联结
MariaDB [test]> select orders.*,customers.* from orders right outer join customers on customers.cust_id = orders.cust_id;
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
| order_num | order_date          | cust_id | cust_id | cust_name      | cust_address        | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
|     20005 | 2005-09-01 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|     20006 | 2005-09-12 00:00:00 |   10003 |   10003 | Wascals        | 1 Sunny Place       | Muncie    | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|     20007 | 2005-09-30 00:00:00 |   10004 |   10004 | Yosemite Place | 829 Riverside Drive | Phoenix   | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|     20008 | 2005-10-03 00:00:00 |   10005 |   10005 | E Fudd         | 4545 53rd Street    | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL                |
|     20009 | 2005-10-08 00:00:00 |   10001 |   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit   | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|      NULL | NULL                |    NULL |   10002 | Mouse House    | 333 Fromage Lane    | Columbus  | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
+-----------+---------------------+---------+---------+----------------+---------------------+-----------+------------+----------+--------------+--------------+---------------------+
6 rows in set (0.00 sec)
```

聚集函数也可以方便地与其他联结一起使用。请看下面的例子:

>> 检索所有客户及每个客户所下的订单数(包括没有订单的客户)

```shell
MariaDB [test]> select customers.cust_name,customers.cust_id,count(orders.order_num) as num_ord from customers left outer join orders on customers.cust_id = orders.cust_id group by customers.cust_id;
+----------------+---------+---------+
| cust_name      | cust_id | num_ord |
+----------------+---------+---------+
| Coyote Inc.    |   10001 |       2 |
| Mouse House    |   10002 |       0 |
| Wascals        |   10003 |       1 |
| Yosemite Place |   10004 |       1 |
| E Fudd         |   10005 |       1 |
+----------------+---------+---------+
5 rows in set (0.00 sec)
```

这个例子使用左外部联结来包含所有客户,甚至包含那些没有
任何下订单的客户。结果显示也包含了客户 `Mouse House` ,它
有 0 个订单。

> 使用联结和联结条件

在总结关于联结的这两章前,有必要汇总一下关于联结及其使用的
某些要点。
* 注意所使用的联结类型。一般我们使用内部联结,但使用外部联
结也是有效的。
* 保证使用正确的联结条件,否则将返回不正确的数据。
* 应该总是提供联结条件,否则会得出笛卡儿积。
* 在一个联结中可以包含多个表,甚至对于每个联结可以采用不同
的联结类型。虽然这样做是合法的,一般也很有用,但应该在一
起测试它们前,分别测试每个联结。这将使故障排除更为简单。

> 小结
>> 本章是上一章关于联结的继续。本章从讲授如何以及为什么要使用
别名开始,然后讨论不同的联结类型及对每种类型的联结使用的各种语
法形式。我们还介绍了如何与联结一起使用聚集函数,以及在使用联结
时应该注意的某些问题。

---

#### 组合查询

本章讲述如何利用 UNION 操作符将多条 SELECT 语句组合成一个结果集。

> 组合查询

多数SQL查询都只包含从一个或多个表中返回数据的单条 SELECT 语
句。MySQL也允许执行多个查询(多条 SELECT 语句),
并将结果作为单个查询结果集返回。这些组合查询通常称为**并(union)或复合查询(compound query)**。

有两种基本情况,其中需要使用组合查询:

* 在单个查询中从不同的表返回类似结构的数据;
* 对单个表执行多个查询,按单个查询返回数据。

组合查询和多个 WHERE 条件 多数情况下,组合相同表的两个
查询完成的工作与具有多个 WHERE 子句条件的单条查询完成的
工作相同。换句话说,任何具有多个 WHERE 子句的 SELECT 语句
都可以作为一个组合查询给出,在以下段落中可以看到这一点。
这两种技术在不同的查询中性能也不同。因此,应该试一下这
两种技术,以确定对特定的查询哪一种性能更好。

> 创建组合查询

可用 UNION 操作符来组合数条SQL查询。利用 UNION ,可给出多条
SELECT 语句,将它们的结果组合成单个结果集。

1.使用 UNION

UNION 的使用很简单。所需做的只是给出每条 SELECT 语句,在各条语
句之间放上关键字 UNION 。

>> 举一个例子,假如需要价格小于等于 5 的所有物品的一个列表,而且
还想包括供应商 1001 和 1002 生产的所有物品(不考虑价格)。

当然,可以利用 WHERE 子句来完成此工作,不过这次我们将使用 UNION 。
正如所述,创建 UNION 涉及编写多条 SELECT 语句。首先来看单条语句:

```shell
MariaDB [test]> select vend_id,prod_id,prod_price from products where prod_price <=5;
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
+---------+---------+------------+
4 rows in set (0.00 sec)

MariaDB [test]> select vend_id,prod_id,prod_price from products where vend_id in (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
5 rows in set (0.00 sec)
```

第一条 SELECT 检索价格不高于 5 的所有物品。第二条 SELECT 使
用 IN 找出供应商 1001 和 1002 生产的所有物品。
为了组合这两条语句,按如下进行:

```shell
MariaDB [test]>  select vend_id,prod_id,prod_price from products where prod_price <=5 union select vend_id,prod_id,prod_price from products where vend_id in (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
8 rows in set (0.00 sec)
```

这条语句由前面的两条 SELECT 语句组成,语句中用 UNION 关键
字分隔。 UNION 指示MySQL执行两条 SELECT 语句,并把输出组
合成单个查询结果集。

作为参考,这里给出使用多条 WHERE 子句而不是使用 UNION 的相同查询:

```shell
MariaDB [test]> select vend_id,prod_id,prod_price from products where prod_price <=5 or vend_id in (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
+---------+---------+------------+
8 rows in set (0.00 sec)
```

在这个简单的例子中,使用 UNION 可能比使用 WHERE 子句更为复杂。
但对于更复杂的过滤条件,或者从多个表(而不是单个表)中检索数据
的情形,使用 UNION 可能会使处理更简单。

>> 检索客户信息表中客户名包含Mouse的客户id，以及订单表中，订单号为20005的客户id。

```shell
MariaDB [test]> select cust_id from orders where order_num=20005 union select cust_id from customers where cust_name regexp 'Mouse';
+---------+
| cust_id |
+---------+
|   10001 |
|   10002 |
+---------+
2 rows in set (0.00 sec)
```
2.UNION 规则

正如所见,并是非常容易使用的。但在进行并时有几条规则需要注意。

* UNION 必须由两条或两条以上的 SELECT 语句组成,语句之间用关
键字 UNION 分隔(因此,如果组合4条 SELECT 语句,将要使用3个
UNION 关键字)。
* UNION 中的每个查询必须包含相同的列、表达式或聚集函数(不过
各个列不需要以相同的次序列出)。
* 列数据类型必须兼容:类型不必完全相同,但必须是DBMS可以
隐含地转换的类型(例如,不同的数值类型或不同的日期类型)。

如果遵守了这些基本规则或限制,则可以将并用于任何数据检索任务。

3.包含或取消重复的行

我们注意到,在分别执行时,第一条 SELECT 语句返回4行,第二条 SELECT 语句返回5行。
但在用 UNION 组合两条 SELECT 语句后,只返回了8行而不是9行。

UNION 从查询结果集中自动去除了重复的行(换句话说,它的行为与。
因为供应商 1002 生产单条 SELECT 语句中使用多个 WHERE 子句条件一样)
的一种物品的价格也低于 5 ,所以两条 SELECT 语句都返回该行。在使用
UNION 时,重复的行被自动取消。

这是 UNION 的默认行为,但是如果需要,可以改变它。事实上,如果
想返回所有匹配行,可使用 UNION ALL 而不是 UNION 。

请看下面的例子:

```shell
MariaDB [test]>  select vend_id,prod_id,prod_price from products where prod_price <=5 union all select vend_id,prod_id,prod_price from products where vend_id in (1001,1002);
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1003 | FC      |       2.50 |
|    1002 | FU1     |       3.42 |
|    1003 | SLING   |       4.49 |
|    1003 | TNT1    |       2.50 |
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
+---------+---------+------------+
9 rows in set (0.00 sec)
```

使用 `UNION ALL` ,MySQL不取消重复的行。因此这里的例子返
回9行,其中有一行出现两次。

**UNION 与 WHERE** 本章开始时说过, `UNION` 几乎总是完成与多个
`WHERE` 条件相同的工作。 `UNION ALL` 为 `UNION` 的一种形式,它完成
`WHERE` 子句完成不了的工作。如果确实需要每个条件的匹配行全
部出现(包括重复行),则必须使用 `UNION ALL` 而不是 `WHERE` 。

4.对组合查询结果排序

`SELECT` 语句的输出用 `ORDER BY` 子句排序。在用 `UNION` 组合查询时,只
能使用一条 `ORDER BY` 子句,它必须出现在最后一条 `SELECT` 语句之后。对
于结果集,不存在用一种方式排序一部分,而又用另一种方式排序另一
部分的情况,因此不允许使用多条 `ORDER BY` 子句。

>> 下面的例子排序前面 UNION 返回的结果:

```shell
MariaDB [test]> select vend_id,prod_id,prod_price from products where prod_price <=5 union all select vend_id,prod_id,prod_price from products where vend_id in (1001,1002) order by vend_id,prod_price;
+---------+---------+------------+
| vend_id | prod_id | prod_price |
+---------+---------+------------+
|    1001 | ANV01   |       5.99 |
|    1001 | ANV02   |       9.99 |
|    1001 | ANV03   |      14.99 |
|    1002 | FU1     |       3.42 |
|    1002 | FU1     |       3.42 |
|    1002 | OL1     |       8.99 |
|    1003 | FC      |       2.50 |
|    1003 | TNT1    |       2.50 |
|    1003 | SLING   |       4.49 |
+---------+---------+------------+
9 rows in set (0.00 sec)
```

这条 `UNION` 在最后一条 `SELECT` 语句后使用了 `ORDER BY` 子句。虽
然 `ORDER BY` 子句似乎只是最后一条 `SELECT` 语句的组成部分,但
实际上MySQL将用它来排序所有 `SELECT` 语句返回的所有结果。

**组合不同的表** 为使表述比较简单,本章例子中的组合查询使
用的均是相同的表。但是其中使用 UNION 的组合查询可以应用
不同的表。

> 小结
>> 本章讲授如何用 UNION 操作符来组合 SELECT 语句。利用 UNION ,可把
多条查询的结果作为一条组合查询返回,不管它们的结果中包含还是不
包含重复。使用 UNION 可极大地简化复杂的 WHERE 子句,简化从多个表中
检索数据的工作。

---

#### 全文本搜索

本章将学习如何使用MySQL的全文本搜索功能进行高级的数据查询
和选择。

> 理解全文本搜索

**并非所有引擎都支持全文本搜索** MySQL支持几种基本的数据库引擎。
并非所有的引擎都支持全文本搜索。两个最常使用的引擎为 MyISAM 和 InnoDB ,
前者支持全文本搜索,而后者不支持。这就是为什么虽然本书
中创建的多数样例表使用 InnoDB ,而有一个样例表
( productnotes 表)却使用 MyISAM 的原因。如果你的应用中需
要全文本搜索功能,应该记住这一点。

前面介绍了 LIKE 关键字,它利用通配操作符匹配文本(和部分文本)。
使用 LIKE ,能够查找包含特殊值或部分值的行(不管这些值位于列
内什么位置)。

还学习了,用基于文本的搜索作为正则表达式匹配列值的更进一
步的介绍。使用正则表达式,可以编写查找所需行的非常复杂的匹配模式。

虽然这些搜索机制非常有用,但存在几个重要的限制。

* 性能——通配符和正则表达式匹配通常要求MySQL尝试匹配表
中所有行(而且这些搜索极少使用表索引)。因此,由于被搜索行
数不断增加,这些搜索可能非常耗时。
* 明确控制——使用通配符和正则表达式匹配,很难(而且并不总
是能)明确地控制匹配什么和不匹配什么。例如,指定一个词必
须匹配,一个词必须不匹配,而一个词仅在第一个词确实匹配的
情况下才可以匹配或者才可以不匹配。
* 智能化的结果——虽然基于通配符和正则表达式的搜索提供了非
常灵活的搜索,但它们都不能提供一种智能化的选择结果的方法。
例如,一个特殊词的搜索将会返回包含该词的所有行,而不区分
包含单个匹配的行和包含多个匹配的行(按照可能是更好的匹配
来排列它们)。类似,一个特殊词的搜索将不会找出不包含该词但
包含其他相关词的行。

所有这些限制以及更多的限制都可以用全文本搜索来解决。在使用
全文本搜索时,MySQL不需要分别查看每个行,不需要分别分析和处理
每个词。MySQL创建指定列中各词的一个索引,搜索可以针对这些词进
行。这样,MySQL可以快速有效地决定哪些词匹配(哪些行包含它们)
,哪些词不匹配,它们匹配的频率,等等。

> 使用全文本搜索

为了进行全文本搜索,必须索引被搜索的列,而且要随着数据的改
变不断地重新索引。在对表列进行适当设计后,MySQL会自动进行所有
的索引和重新索引。

在索引之后, SELECT 可与 Match() 和 Against() 一起使用以实际执行
搜索。

1.启用全文本搜索支持

一般在创建表时启用全文本搜索。 `CREATE TABLE` 语句接受 `FULLTEXT` 子句,它给出被索引列的一个逗号分隔的列表。

>> 下面的 CREATE 语句演示了 FULLTEXT 子句的使用:

```
MariaDB [test]> create table productnotes1 (note_id int not null auto_increment,prod_id char(10) not null,note_date datetime not null,note_text text null,primary key(note_id),fulltext(note_text)) engine=myisam;
Query OK, 0 rows affected (0.37 sec)

MariaDB [test]> show table status where name='productnotes1'\G;
*************************** 1. row ***************************
           Name: productnotes1
         Engine: MyISAM
        Version: 10
     Row_format: Dynamic
           Rows: 0
 Avg_row_length: 0
    Data_length: 0
Max_data_length: 281474976710655
   Index_length: 1024
      Data_free: 0
 Auto_increment: 1
    Create_time: 2016-09-20 15:20:19
    Update_time: 2016-09-20 15:20:19
     Check_time: NULL
      Collation: latin1_swedish_ci
       Checksum: NULL
 Create_options:
        Comment:
1 row in set (0.00 sec)

```

后面将详细考察 `CREATE TABLE` 语句。现在,只需知道这条
`CREATE TABLE` 语句定义表 `productnotes1` 并列出它所包含的列
即可。这些列中有一个名为 `note_text` 的列,为了进行全文本搜索,
MySQL根据子句 `FULLTEXT(note_text)` 的指示对它进行索引。这里的
`FULLTEXT` 索引单个列,如果需要也可以指定多个列。

在定义之后,MySQL自动维护该索引。在增加、更新或删除行时,
索引随之自动更新。可以在创建表时指定 `FULLTEXT` ,或者在稍后指定(在这种情况下所
有已有数据必须立即索引)。

**不要在导入数据时使用 FULLTEXT** 更新索引要花时间,虽然
不是很多,但毕竟要花时间。如果正在导入数据到一个新表,
此时不应该启用 `FULLTEXT` 索引。应该首先导入所有数据,然
后再修改表,定义` FULLTEXT` 。这样有助于更快地导入数据(而
且使索引数据的总时间小于在导入每行时分别进行索引所需
的总时间)

2.进行全文本搜索

在索引之后,使用两个函数 Match() 和 Against() 执行全文本搜索,
其中 Match() 指定被搜索的列, Against() 指定要使用的搜索表达式。
下面举一个例子:

```shell
MariaDB [test]> select * from productnotes;
+---------+---------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_id | prod_id | note_date           | note_text                                                                                                                                                 |
+---------+---------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
|     101 | TNT2    | 2005-08-17 00:00:00 | Customer complaint:
Sticks not individually wrapped, too easy to mistakenly detonate all at once.
Recommend individual wrapping.                          |
|     102 | OL1     | 2005-08-18 00:00:00 | Can shipped full, refills not available.
Need to order new can if refill needed.                                                                          |
|     103 | SAFE    | 2005-08-18 00:00:00 | Safe is combination locked, combination not provided with safe.
This is rarely a problem as safes are typically blown up or dropped by customers.         |
|     104 | FC      | 2005-08-19 00:00:00 | Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                      |
|     105 | TNT2    | 2005-08-20 00:00:00 | Included fuses are short and have been known to detonate too quickly for some customers.
Longer fuses are available (item FU1) and should be recommended. |
|     106 | TNT2    | 2005-08-22 00:00:00 | Matches not included, recommend purchase of matches or detonator (item DTNTR).                                                                            |
|     107 | SAFE    | 2005-08-23 00:00:00 | Please note that no returns will be accepted if safe opened using explosives.                                                                             |
|     108 | ANV01   | 2005-08-25 00:00:00 | Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils.  |
|     109 | ANV03   | 2005-09-01 00:00:00 | Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.                                       |
|     110 | FC      | 2005-09-01 00:00:00 | Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                              |
|     111 | SLING   | 2005-09-02 00:00:00 | Shipped unassembled, requires common tools (including oversized hammer).                                                                                  |
|     112 | SAFE    | 2005-09-02 00:00:00 | Customer complaint:
Circular hole in safe floor can apparently be easily cut with handsaw.                                                                |
|     113 | ANV01   | 2005-09-05 00:00:00 | Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |
|     114 | SAFE    | 2005-09-07 00:00:00 | Call from individual trapped in safe plummeting to the ground, suggests an escape hatch be added.
Comment forwarded to vendor.                            |
+---------+---------+---------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------+
14 rows in set (0.00 sec)

MariaDB [test]> select note_text from productnotes where note_text regexp 'rabbit';
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                         |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

MariaDB [test]> select note_text from productnotes where match(note_text) against('rabbit');
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                         |
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

```

此 SELECT 语句检索单个列 `note_text` 。由于 WHERE 子句,一个全
文本搜索被执行。 `Match(note_text)` 指示MySQL针对指定的
列进行搜索, `Against('rabbit')` 指定词 `rabbit` 作为搜索文本。由于有
两行包含词 `rabbit` ,这两个行被返回。

**使用完整的Match()说明** 传递给 Match() 的值必须与
FULLTEXT() 定义中的相同。如果指定多个列,则必须列出它
们(而且次序正确)。

**搜索不区分大小写** 除非使用 BINARY 方式(本章中没有介绍)
,否则全文本搜索不区分大小写。

事实是刚才的搜索可以简单地用 LIKE 子句完成,如下所示:

```shell
MariaDB [test]> select note_text from productnotes where note_text like '%rabbit%';
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                         |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

这条 SELECT 语句同样检索出两行,但次序不同(虽然并不总是
出现这种情况)。

上述两条 SELECT 语句都不包含 ORDER BY 子句。后者(使用 LIKE )以
不特别有用的顺序返回数据。前者(使用全文本搜索)返回以文本匹配
的良好程度排序的数据。两个行都包含词 rabbit ,但包含词 rabbit 作为
第3个词的行的等级比作为第20个词的行高。这很重要。全文本搜索的一
个重要部分就是对结果排序。具有较高等级的行先返回(因为这些行很
可能是你真正想要的行)。

为演示排序如何工作,请看以下例子:

```shell
MariaDB [test]> select note_text,match(note_text) against('rabbit') as rank from productnotes;
+-----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| note_text                                                                                                                                                 | rank               |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
| Customer complaint:
Sticks not individually wrapped, too easy to mistakenly detonate all at once.
Recommend individual wrapping.                          |                  0 |
| Can shipped full, refills not available.
Need to order new can if refill needed.                                                                          |                  0 |
| Safe is combination locked, combination not provided with safe.
This is rarely a problem as safes are typically blown up or dropped by customers.         |                  0 |
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait.                                      | 1.5905543565750122 |
| Included fuses are short and have been known to detonate too quickly for some customers.
Longer fuses are available (item FU1) and should be recommended. |                  0 |
| Matches not included, recommend purchase of matches or detonator (item DTNTR).                                                                            |                  0 |
| Please note that no returns will be accepted if safe opened using explosives.                                                                             |                  0 |
| Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils.  |                  0 |
| Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.                                       |                  0 |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                              | 1.6408053636550903 |
| Shipped unassembled, requires common tools (including oversized hammer).                                                                                  |                  0 |
| Customer complaint:
Circular hole in safe floor can apparently be easily cut with handsaw.                                                                |                  0 |
| Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.   |                  0 |
| Call from individual trapped in safe plummeting to the ground, suggests an escape hatch be added.
Comment forwarded to vendor.                            |                  0 |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+
14 rows in set (0.00 sec)

```

这里,在 `SELECT` 而不是 `WHERE` 子句中使用 `Match()` 和 `Against()` 。这
使所有行都被返回(因为没有 WHERE 子句)。 `Match()` 和 `Against()`
用来建立一个计算列(别名为 `rank` ),此列包含全文本搜索计算出的等级
值。等级由MySQL根据行中词的数目、唯一词的数目、整个索引中词的
总数以及包含该词的行的数目计算出来。正如所见,不包含词 `rabbit` 的
行等级为0(因此不被前一例子中的 `WHERE` 子句选择)。
确实包含词 `rabbit`的两个行每行都有一个等级值,文本中词靠前的行的等级值比词靠后的
行的等级值高。

这个例子有助于说明全文本搜索如何排除行(排除那些等级为0的行)
,如何排序结果(按等级以降序排序)。

**排序多个搜索项** 如果指定多个搜索项,则包含多数匹配词的
那些行将具有比包含较少词(或仅有一个匹配)的那些行高的
等级值。

正如所见,全文本搜索提供了简单 LIKE 搜索不能提供的功能。而且,
由于数据是索引的,全文本搜索还相当快。


3.使用查询扩展

查询扩展用来设法放宽所返回的全文本搜索结果的范围。考虑下面
的情况。你想找出所有提到 anvils 的注释。
只有一个注释包含词 anvils ,
但你还想找出可能与你的搜索有关的所有其他行,即使它们不包含词
anvils 。

这也是查询扩展的一项任务。在使用查询扩展时,MySQL对数据和
索引进行两遍扫描来完成搜索:
* 首先,进行一个基本的全文本搜索,找出与搜索条件匹配的所有
行;
* 其次,MySQL检查这些匹配行并选择所有有用的词(我们将会简
要地解释MySQL如何断定什么有用,什么无用)。
* 再其次, MySQL再次进行全文本搜索,这次不仅使用原来的条件,
而且还使用所有有用的词。

利用查询扩展,能找出可能相关的结果,即使它们并不精确包含所
查找的词。

**只用于MySQL版本4.1.1或更高级的版本** 查询扩展功能是在
MySQL 4.1.1中引入的,因此不能用于之前的版本。

下面举一个例子,首先进行一个简单的全文本搜索,没有查询扩展:

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('anvils');
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                                                                |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

只有一行包含词 anvils ,因此只返回一行。

下面是相同的搜索,这次使用查询扩展:

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('anvils' with query expansion);
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                                                                |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils. |
| Customer complaint:
Sticks not individually wrapped, too easy to mistakenly detonate all at once.
Recommend individual wrapping.                         |
| Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.  |
| Please note that no returns will be accepted if safe opened using explosives.                                                                            |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                                                             |
| Customer complaint:
Circular hole in safe floor can apparently be easily cut with handsaw.                                                               |
| Matches not included, recommend purchase of matches or detonator (item DTNTR).                                                                           |
+----------------------------------------------------------------------------------------------------------------------------------------------------------+
7 rows in set (0.00 sec)

```

这次返回了7行。第一行包含词 `anvils` ,因此等级最高。第二
行与 `anvils` 无关,但因为它包含第一行中的两个词( `customer`
和 `recommend` ),所以也被检索出来。第3行也包含这两个相同的词,但它
们在文本中的位置更靠后且分开得更远,因此也包含这一行,但等级为
第三。第三行确实也没有涉及 `anvils` (按它们的产品名)。

正如所见,查询扩展极大地增加了返回的行数,但这样做也增加了
你实际上并不想要的行的数目。

**行越多越好** 表中的行越多(这些行中的文本就越多),使用查询扩展返回的结果越好。

4.布尔文本搜索

MySQL支持全文本搜索的另外一种形式,称为布尔方式(boolean mode)。
以布尔方式,可以提供关于如下内容的细节:

* 要匹配的词;
* 要排斥的词(如果某行包含这个词,则不返回该行,即使它包含
其他指定的词也是如此);
* 排列提示(指定某些词比其他词更重要,更重要的词等级更高);
* 表达式分组;
* 另外一些内容。

**即使没有 FULLTEXT 索引也可以使用** 布尔方式不同于迄今为
止使用的全文本搜索语法的地方在于,即使没有定义
FULLTEXT 索引,也可以使用它。但这是一种非常缓慢的操作
(其性能将随着数据量的增加而降低)。

为演示 IN BOOLEAN MODE 的作用,举一个简单的例子:

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('heavy' in boolean mode);
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                                                               |
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.                                     |
| Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

此全文本搜索检索包含词 heavy 的所有行(有两行)
。其中使用了关键字 IN BOOLEAN MODE ,但实际上没有指定布尔操作符,
因此,其结果与没有指定布尔方式的结果相同。

**IN BOOLEAN MODE 的行为差异** 虽然这个例子的结果与没有
IN BOOLEAN MODE 的相同,但其行为有一个重要的差别(即
使在这个特殊的例子没有表现出来)。

>> 为了匹配包含 heavy 但不包含任意以 rope 开始的词的行,可使用以下
查询:

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('heavy -rope*' in boolean mode);
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                                                               |
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
| Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

这次只返回一行。这一次仍然匹配词 `heavy` ,但 `-rope*` 明确地
指示 MySQL排除 包含 `rope*` (任何以 `rope` 开始 的词,包 括
`ropes` )的行,这就是为什么上一个例子中的第一行被排除的原因。


**在MySQL 4.x中所需的代码更改** 如果你使用的是MySQL
4.x,则上面的例子可能不返回任何行。这是 * 操作符处理中的
一个错误。为在MySQL 4.x中使用这个例子,使用 -ropes 而不
是 -rope* (排除 ropes 而不是排除任何以 rope 开始的词)。

|全文本布尔操作符|说明|
|:--|:--|
|+|包含,词必须存在|
|-|排除,词必须不出现|
|>|包含,而且增加等级值|
|<|包含,且减少等级值|
|()|把词组成子表达式(允许这些子表达式作为一个组被包含、排除、排列等)|
|~|取消一个词的排序值|
|*|词尾的通配符|
|""|定义一个短语(与单个词的列表不一样,它匹配整个短语以便包含或排除这个短语)|

下面举几个例子,说明某些操作符如何使用:

>> 搜索匹配包含词 rabbit 和 bait 的行。

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('+rabbit +bait' in boolean mode);
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
+----------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

>> 搜索匹配包含 rabbit 和 bait 中的至少一个词的行。

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('rabbit bait' in boolean mode);
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                         |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

>> 搜索匹配短语 rabbit bait 而不是匹配两个词 rabbit 和 bait 。

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('"rabbit bait"' in boolean mode);
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
+----------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

>> 搜索匹配 rabbit 和 carrot ,增加前者的等级,降低后者的等级。

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('>rabbit <carrot' in boolean mode);
+----------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                            |
+----------------------------------------------------------------------------------------------------------------------+
| Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait. |
| Customer complaint: rabbit has been able to detect trap, food apparently less effective now.                         |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

>> 搜索匹配词 safe 和 combination ,降低后者的等级。

```shell
MariaDB [test]> select note_text from productnotes where match(note_text) against('+safe +(<combination)' in boolean mode);
+---------------------------------------------------------------------------------------------------------------------------------------------------+
| note_text                                                                                                                                         |
+---------------------------------------------------------------------------------------------------------------------------------------------------+
| Safe is combination locked, combination not provided with safe.
This is rarely a problem as safes are typically blown up or dropped by customers. |
+---------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

**排列而不排序** 在布尔方式中,不按等级值降序排序返回的行。

5.全文本搜索的使用说明

在结束本章之前,给出关于全文本搜索的某些重要的说明。
* 在索引全文本数据时,短词被忽略且从索引中排除。短词定义为
那些具有3个或3个以下字符的词(如果需要,这个数目可以更改)。
* MySQL带有一个内建的非用词(stopword)列表,这些词在索引
全文本数据时总是被忽略。如果需要,可以覆盖这个列表(请参
阅MySQL文档以了解如何完成此工作)。
* 许多词出现的频率很高,搜索它们没有用处(返回太多的结果)。
因此,MySQL规定了一条50%规则,如果一个词出现在50%以上
的行中,则将它作为一个非用词忽略。 50%规则不用于 IN BOOLEAN
MODE 。
* 如果表中的行数少于3行,则全文本搜索不返回结果(因为每个词
或者不出现,或者至少出现在50%的行中)。
* 忽略词中的单引号。例如, don't 索引为 dont 。
* 不具有词分隔符(包括日语和汉语)的语言不能恰当地返回全文
本搜索结果。
* 如前所述,仅在 MyISAM 数据库引擎中支持全文本搜索。

**没有邻近操作符** 邻近搜索是许多全文本搜索支持的一个特
性,它能搜索相邻的词(在相同的句子中、相同的段落中或者
在特定数目的词的部分中,等等)。
MySQL全文本搜索现在还不支持邻近操作符,
不过未来的版本有支持这种操作符的计划。

> 小结
>> 本章介绍了为什么要使用全文本搜索,以及如何使用MySQL的
Match() 和 Against() 函数进行全文本搜索。我们还学习了查询扩展
(它能增加找到相关匹配的机会)和如何使用布尔方式进行更细致的查找控制。

---

### DDL语言

> DDL 是数据定义语言的缩写,简单来说,就是对数据库内部的对象进行创建、删除、修改的
操作语言。它和 DML 语言的最大区别是 DML 只是对表内部数据的操作,而不涉及到表的定
义、结构的修改,更不会涉及到其他对象。 DDL 语句更多的被数据库管理员(DBA)所使用,
一般的开发人员很少使用。

#### 创建和操纵表

本章讲授表的创建、更改和删除的基本知识。

> 创建表

MySQL不仅用于表数据操纵,而且还可以用来执行数据库和表的所
有操作,包括表本身的创建和处理。
一般有两种创建表的方法:

* 使用具有交互式创建和管理表的工具(如第2章讨论的工具);
* 表也可以直接用MySQL语句操纵。

为了用程序创建表,可使用SQL的 `CREATE TABLE` 语句。值得注意的
是,在使用交互式工具时,实际上使用的是MySQL语句。但是,这些语
句不是用户编写的,界面工具会自动生成并执行相应的MySQL语句(更
改现有表时也是这样)。

1.表创建基础

为利用 CREATE TABLE 创建表,必须给出下列信息:

* 新表的名字,在关键字 CREATE TABLE 之后给出;
* 表列的名字和定义,用逗号分隔。

CREATE TABLE 语句也可能会包括其他关键字或选项,但至少要包括表的
名字和列的细节。下面的MySQL语句创建我们所用的 customers 表:

```shell
MariaDB [test]> create table customers
    -> (
    -> cust_id int not null auto_increment,
    -> cust_name char(50) not null,
    -> cust_address char(50) null,
    -> cust_city char(50) null,
    -> cust_state char(5) null,
    -> cust_zip char(10) null,
    -> cust_country char(50) null,
    -> cust_contact char(50) null,
    -> cust_email char(255) null,
    -> primary key (cust_id)
    -> ) engine=innodb;
```

从上面的例子中可以看到,表名紧跟在 `CREATE TABLE` 关键字后
面。实际的表定义(所有列)括在圆括号之中。各列之间用逗
号分隔。这个表由9列组成。每列的定义以列名(它在表中必须是唯一的)
开始,后跟列的数据类型(关于数据类型的解释,请参阅第1章。此外,
附录D列出了MySQL支持的数据类型)。表的主键可以在创建表时用
`PRIMARY KEY` 关键字指定。这里,列 `cust_id` 指定作为主键列。整条语句
由右圆括号后的分号结束。( 现在先忽略`ENGINE=InnoDB` 和
`AUTO_INCREMENT` ,后面会对它们进行介绍。)

**语句格式化** 可回忆一下,以前说过MySQL语句中忽略空格。
语句可以在一个长行上输入,也可以分成许多行。它们的作
用都相同。这允许你以最适合自己的方式安排语句的格式。
前面的 CREATE TABLE 语句就是语句格式化的一个很好的例
子,它被安排在多个行上,其中的列定义进行了恰当的缩进,
以便阅读和编辑。以何种缩进格式安排SQL语句没有规定,
但我强烈推荐采用某种缩进格式。

**处理现有的表** 在创建新表时,指定的表名必须不存在,否则
将出错。如果要防止意外覆盖已有的表,SQL要求首先手工删
除该表,然后再重建它,而不是简单地
用创建表语句覆盖它。如果你仅想在一个表不存在时创建它,
应该在表名后给出 `IF NOT EXISTS` 。
这样做不检查已有表的模式是否与你打算创建
的表模式相匹配。它只是查看表名是否存在,并且仅在表名不
存在时创建它。

```shell
DROP TABLE IF EXISTS columns_priv;
CREATE TABLE columns_priv (
  Host char(60) COLLATE utf8_bin NOT NULL DEFAULT '',
  Db char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  User char(16) COLLATE utf8_bin NOT NULL DEFAULT '',
  Table_name char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  Column_name char(64) COLLATE utf8_bin NOT NULL DEFAULT '',
  Timestamp timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  Column_priv set('Select','Insert','Update','References') CHARACTER SET utf8 NOT NULL DEFAULT '',
  PRIMARY KEY (Host,Db,User,Table_name,Column_name)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='Column privileges';
```
2.使用 NULL 值

前面章节中说过, NULL 值就是没有值或缺值。允许 NULL 值的列也允许在
插入行时不给出该列的值。不允许 NULL 值的列不接受该列没有值的行,
换句话说,在插入或更新行时,该列必须有值。

每个表列或者是 NULL 列,或者是 NOT NULL 列,这种状态在创建时由
表的定义规定。请看下面的例子:

>> 创建 orders 表

```shell
MariaDB [test]> create table orders
    -> (
    -> order_num int not null auto_increment,
    -> order_date datetime not null,
    -> cust_id int not null,
    -> primary key (order_num)
    -> ) engine=innodb;
```

`orders` 包含3个列,分别是订单号、订单日期和客户ID。所有3个列都需要,因此每
个列的定义都含有关键字 `NOT NULL` 。这将会阻止插入没有值的列。如果
试图插入没有值的列,将返回错误,且插入失败。

>> 创建 vendors 表

```shell
CREATE TABLE vendors (
  vend_id int NOT NULL AUTO_INCREMENT,
  vend_name char(50) NOT NULL,
  vend_address char(50) NULL,
  vend_city char(50) NULL,
  vend_state char(5) NULL,
  vend_zip char(10) NULL,
  vend_country char(50) NULL,
  PRIMARY KEY (vend_id)
) ENGINE=InnoDB;
```

供应商ID和供应商名字列是必需的,因此指定为 `NOT NULL` 。其余5个列全都允许 `NULL` 值,所以
不指定 `NOT NULL` 。` NULL` 为默认设置,如果不指定 `NOT NULL` ,则认为指定的是 `NULL` 。

**理解 NULL** 不要把 NULL 值与空串相混淆。 NULL 值是没有值,它不是空串。
如果指定 '' (两个单引号,其间没有字符),这在 NOT NULL 列中是允许的。空串是一个有效的值,它不是无
值。 NULL 值用关键字 NULL 而不是空串指定。

3.主键再介绍

正如所述,主键值必须唯一。即,表中的每个行必须具有唯一的主
键值。如果主键使用单个列,则它的值必须唯一。如果使用多个列,则
这些列的组合值必须唯一。

迄今为止我们看到的 CREATE TABLE 例子都是用单个列作为主键。其
中主键用以下的类似的语句定义:`primary key (vend_id)`

为创建由多个列组成的主键,应该以逗号分隔的列表给出各列名,
如下所示:

```shell
CREATE TABLE orderitems (
  order_num int NOT NULL,
  order_item int NOT NULL,
  prod_id char(10) NOT NULL,
  quantity int NOT NULL,
  item_price decimal(8,2) NOT NULL,
  PRIMARY KEY (order_num,order_item),
  ) ENGINE=InnoDB;
```

`orderitems` 表包含orders表中每个订单的细节。每个订单有多项物
品,但每个订单任何时候都只有1个第一项物品,1个第二项物品,如此
等等。因此,订单号( `order_num` 列)和订单物品( `order_item` 列)的组
合是唯一的,从而适合作为主键,其定义为:`RIMARY KEY (order_num,order_item)`

主键可以在创建表时定义(如这里所示),或者在创建表之后定义(本章稍后讨论)。

4.使用 AUTO_INCREMENT

让我们再次考察 `customers` 和 `orders `表。 `customers` 表中的顾客由列
`cust_id` 唯一标识,每个顾客有一个唯一编号。类似, `orders` 表中的每个
订单有一个唯一的订单号,这个订单号存储在列 `order_num` 中。
这些编号除它们是唯一的以外没有别的特殊意义。在增加一个新顾
客或新订单时,需要一个新的顾客ID或订单号。这些编号可以任意,只
要它们是唯一的即可。

显然,使用的最简单的编号是下一个编号,所谓下一个编号是大于
当前最大编号的编号。例如,如果 `cust_id` 的最大编号为 `10005` ,则插入
表中的下一个顾客可以具有等于 `10006` 的 `cust_id` 。

简单吗?不见得。你怎样确定下一个要使用的值?当然,你可以使
用 `SELECT `语句得出最大的数,然后对它加1。
但这样做并不可靠(你需要找出一种办法来保证,在你执行 `SELECT`
和 `INSERT` 两条语句之间没有其他人插入行,对于多用户应用,这种情况
是很有可能出现的),而且效率也不高(执行额外的MySQL操作肯定不是
理想的办法)。

这就是 `AUTO_INCREMENT` 发挥作用的时候了。请看以下代码行(用来
创建 customers 表的 CREATE TABLE 语句的组成部分):
`cust_id int NOT NULL AUTO_INCREMENT`

`AUTO_INCREMENT` 告诉MySQL,本列每当增加一行时自动增量。每次
执行一个 `INSERT` 操作时,MySQL自动对该列增量(从而才有这个关键字
`AUTO_INCREMENT` ),给该列赋予下一个可用的值。这样给每个行分配一个
唯一的` cust_id `,从而可以用作主键值。

每个表只允许一个 `AUTO_INCREMENT` 列,而且它必须被索引(如,通过使它成为主键)

**覆 盖 AUTO_INCREMENT** 如果一个列被指定为 AUTO_INCREMENT ,
则它需要使用特殊的值吗?你可以简单地在 INSERT 语句
中指定一个值,只要它是唯一的(至今尚未使用过)即可,该
值将被用来替代自动生成的值。后续的增量将开始使用该手工
插入的值。

**确定 AUTO_INCREMENT 值** 让MySQL生成(通过自动增量)主
键的一个缺点是你不知道这些值都是谁。

考虑这个场景:你正在增加一个新订单。这要求在 `orders` 表
中创建一行,然后在 `orderitms` 表中对订购的每项物品创建一
行。 `order_num` 在 `orderitem`s 表中与订单细节一起存储。这
就是为什么 `orders` 表和 `orderitems` 表为相互关联的表的原
因。这显然要求你在插入 `orders` 行之后,插入 `orderitems` 行
之前知道生成的 `order_num` 。
那么,如何在使用 `AUTO_INCREMENT` 列时获得这个值呢?可使
用 `last_insert_id()` 函数获得这个值,如下所示:
`select last_insert_id()`
此语句返回最后一个 `AUTO_INCREMENT` 值,然后可以将它用于
后续的MySQL语句。

5.指定默认值

如果在插入行时没有给出值,MySQL允许指定此时使用的默认值。
默认值用 CREATE TABLE 语句的列定义中的 DEFAULT 关键字指定。
请看下面的例子:

```shell
CREATE TABLE orderitems (
  order_num int NOT NULL,
  order_item int NOT NULL,
  prod_id char(10) NOT NULL,
  quantity int NOT NULL default 1,
  item_price decimal(8,2) NOT NULL,
  PRIMARY KEY (order_num,order_item),
  ) ENGINE=InnoDB;
```

这条语句创建包含组成订单的各物品的 `orderitems` 表(订单本
身存储在 orders 表中)。 `quantity` 列包含订单中每项物品的数
量。在此例子中,给该列的描述添加文本 `DEFAULT 1` 指示MySQL,在未
给出数量的情况下使用数量 1 。

**不允许函数** 与大多数DBMS不一样,MySQL不允许使用函
数作为默认值,它只支持常量。

**使用默认值而不是 NULL 值** 许多数据库开发人员使用默认
值而不是 NULL 列,特别是对用于计算或数据分组的列更是如此。

6.引擎类型

你可能已经注意到,迄今为止使用的 CREATE TABLE 语句全都以
ENGINE=InnoDB 语句结束。

与其他DBMS一样, MySQL有一个具体管理和处理数据的内部引擎。

在你使用 CREATE TABLE 语句时,该引擎具体创建表,而在你使用 SELECT
语句或进行其他数据库处理时,该引擎在内部处理你的请求。多数时候,
此引擎都隐藏在DBMS内,不需要过多关注它。

但MySQL与其他DBMS不一样,它具有多种引擎。它打包多个引擎,
这些引擎都隐藏在MySQL服务器内,全都能执行 CREATE TABLE 和 SELECT
等命令。

为什么要发行多种引擎呢?

因为它们具有各自不同的功能和特性,
为不同的任务选择正确的引擎能获得良好的功能和灵活性。

当然,你完全可以忽略这些数据库引擎。如果省略 ENGINE= 语句,则
,多数SQL语句都会默认使用它。但并
使用默认引擎(很可能是 MyISAM )
不是所有语句都默认使用它,这就是为什么 ENGINE= 语句很重要的原因
(也就是为什么本书的样列表中使用两种引擎的原因)。

以下是几个需要知道的引擎:
* InnoDB 是一个可靠的事务处理引擎,它不支持全文本搜索;
* MEMORY 在功能等同于 MyISAM ,但由于数据存储在内存(不是磁盘)中,速度很快(特别适合于临时表);
* MyISAM 是一个性能极高的引擎,它支持全文本搜索,但不支持事务处理。

引擎类型可以混用。除 productnotes 表使用 MyISAM 外,本书中的样
例表都使用 InnoDB 。原因是希望支持事务处理(因此,使用 InnoDB ),
但也需要在 productnotes 中支持全文本搜索(因此,使用 MyISAM )。

**外键不能跨引擎** 混用引擎类型有一个大缺陷。外键(用于
强制实施引用完整性,如第1章所述)不能跨引擎,即使用一
个引擎的表不能引用具有使用不同引擎的表的外键。

那么,你应该使用哪个引擎?这有赖于你需要什么样的特性。 MyISAM
由于其性能和特性可能是最受欢迎的引擎。但如果你不需要可靠的事务
处理,可以使用其他引擎。

> 更新表

为更新表定义,可使用 `ALTER TABLE` 语句。但是,理想状态下,当表
中存储数据以后,该表就不应该再被更新。在表的设计过程中需要花费
大量时间来考虑,以便后期不对该表进行大的改动。

为了使用 ALTER TABLE 更改表结构,必须给出下面的信息:

* 在 ALTER TABLE 之后给出要更改的表名(该表必须存在,否则将出错);
* 所做更改的列表。

下面的例子给表添加一个列:

>> 给 vendors 表增加一个名为 vend_phone 的列

```shell
MariaDB [test]> alter table vendors add vend_phone char(20);
Query OK, 6 rows affected (0.11 sec)
Records: 6  Duplicates: 0  Warnings: 0

MariaDB [test]> desc vendors;
+--------------+----------+------+-----+---------+----------------+
| Field        | Type     | Null | Key | Default | Extra          |
+--------------+----------+------+-----+---------+----------------+
| vend_id      | int(11)  | NO   | PRI | NULL    | auto_increment |
| vend_name    | char(50) | NO   |     | NULL    |                |
| vend_address | char(50) | YES  |     | NULL    |                |
| vend_city    | char(50) | YES  |     | NULL    |                |
| vend_state   | char(5)  | YES  |     | NULL    |                |
| vend_zip     | char(10) | YES  |     | NULL    |                |
| vend_country | char(50) | YES  |     | NULL    |                |
| vend_phone   | char(20) | YES  |     | NULL    |                |
+--------------+----------+------+-----+---------+----------------+
8 rows in set (0.00 sec)
```

>> 删除刚刚添加的列vend_phone

```shell
MariaDB [test]> alter table vendors drop column vend_phone;
Query OK, 6 rows affected (0.12 sec)
Records: 6  Duplicates: 0  Warnings: 0

MariaDB [test]> desc vendors;
+--------------+----------+------+-----+---------+----------------+
| Field        | Type     | Null | Key | Default | Extra          |
+--------------+----------+------+-----+---------+----------------+
| vend_id      | int(11)  | NO   | PRI | NULL    | auto_increment |
| vend_name    | char(50) | NO   |     | NULL    |                |
| vend_address | char(50) | YES  |     | NULL    |                |
| vend_city    | char(50) | YES  |     | NULL    |                |
| vend_state   | char(5)  | YES  |     | NULL    |                |
| vend_zip     | char(10) | YES  |     | NULL    |                |
| vend_country | char(50) | YES  |     | NULL    |                |
+--------------+----------+------+-----+---------+----------------+
7 rows in set (0.01 sec)
```

>> ALTER TABLE 的一种常见用途是定义外键。下面是用来定义讲义中的
表所用的外键的代码:

```shell
MariaDB [test]> alter table orderitems
  -> add constraint fk_orderitems_orders
  -> foreign key (order_num) references orders (order_num);

MariaDB [test]> alter table orderitems
  -> add constraint fk_orderitems_products
  -> foreign key (prod_id) references orders (prod_id);

MariaDB [test]> alter table orders
  -> add constraint fk_orderitems_customers
  -> foreign key (cust_id) references orders (cust_id);

MariaDB [test]> alter table products
  -> add constraint fk_orderitems_vendors
  -> foreign key (vend_id) references orders (vend_id);
```

这里,由于要更改4个不同的表,使用了4条 ALTER TABLE 语句。为了
对单个表进行多个更改,可以使用单条 ALTER TABLE 语句,每个更改用逗
号分隔。

复杂的表结构更改一般需要手动删除过程,它涉及以下步骤:

* 用新的列布局创建一个新表;
* 使用 INSERT SELECT 语句从旧表复制数据到新表。如果有必要,可使用转换函数和
计算字段;
* 检验包含所需数据的新表;
* 重命名旧表(如果确定,可以删除它);
* 用旧表原来的名字重命名新表;
* 根据需要,重新创建触发器、存储过程、索引和外键。

**小心使用 ALTER TABLE** 使用 ALTER TABLE 要极为小心,应该
在进行改动前做一个完整的备份(模式和数据的备份)。数据
库表的更改不能撤销,如果增加了不需要的列,可能不能删
除它们。类似地,如果删除了不应该删除的列,可能会丢失
该列中的所有数据。

> 删除表

删除表(删除整个表而不是其内容)非常简单,使用 DROP TABLE 语
句即可: `drop table customers2;`

```shell
ariaDB [test]> create table customer2 as select * from customers;
Query OK, 15 rows affected (0.08 sec)
Records: 15  Duplicates: 0  Warnings: 0

MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | The Fudds      | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
|   20001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   20002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   20003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   20004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   20005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   20006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
15 rows in set (0.00 sec)
```

这条语句删除 customers2 表(假设它存在)
也不能撤销,执行这条语句将永久删除该表。

> 重命名表

使用 RENAME TABLE 语句可以重命名一个表:

```shell
MariaDB [test]> rename table customers to customers3;
Query OK, 0 rows affected (0.05 sec)

MariaDB [test]> rename table customers3 to customers;
Query OK, 0 rows affected (0.04 sec)
```

RENAME TABLE 所做的仅是重命名一个表。可以使用下面的语
句对多个表重命名:

```shell
MariaDB [test]> rename table vendors to vendors0,products to products0,orders to orders0;
Query OK, 0 rows affected (0.08 sec)

MariaDB [test]> rename table vendors0 to vendors,products0 to products,orders0 to orders;
Query OK, 0 rows affected (0.06 sec)
```

> 小结
>> 本章介绍了几条新SQL语句。 CREATE TABLE 用来创建新表, ALTER
TABLE 用来更改表列(或其他诸如约束或索引等对象)
,而 DROP TABLE 用来完整地删除一个表。这些语句必须小心使用,并且应在做了备份后使
用。本章还介绍了数据库引擎、定义主键和外键,以及其他重要的表和
列选项。


### DML语言

DML(Data Manipulation Language)语句:数据操纵语句,用于添加、删除、更新和查
询数据库记录,并检查数据完整性,常用的语句关键字主要包括 insert、delete、udpate 和
select 等。

#### 插入数据

本章介绍如何利用SQL的 INSERT 语句将数据插入表中。

> 数据插入

顾名思义, INSERT 是用来插入(或添加)行到数据库表的。插入可
以用几种方式使用:

* 插入完整的行;
* 插入行的一部分;
* 插入多行;
* 插入某些查询的结果。

下面将介绍这些内容。

**插入及系统安全** 可针对每个表或每个用户,利用MySQL的
安全机制禁止使用 INSERT 语句。

> 插入完整的行

把数据插入表中的最简单的方法是使用基本的 INSERT 语法,它要求
指定表名和被插入到新行中的值。下面举一个例子:

```shell
MariaDB [test]> insert into customers values(null,'Pep E. Lapew','100 Main Street','Los Angeles','CA','90046','USA',null,null);
Query OK, 1 row affected (0.02 sec)

MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
6 rows in set (0.00 sec)
```

**没有输出** INSERT 语句一般不会产生输出。

此例子插入一个新客户到 `customers` 表。存储到每个表列中的
数据在 `VALUES` 子句中给出,对每个列必须提供一个值。如果某
个列没有值(如上面的 `cust_contact` 和 `cust_email` 列),应该使用 NULL
值(假定表允许对该列指定空值)。各个列必须以它们在表定义中出现的
次序填充。第一列 `cust_id` 也为 NULL 。这是因为每次插入一个新行时,该
列由MySQL自动增量。你不想给出一个值(这是MySQL的工作),又不
能省略此列(如前所述,必须给出每个列)
,所以指定一个 NULL 值(它被MySQL忽略,
MySQL在这里插入下一个可用的 `cust_id `值)。

虽然这种语法很简单,但并不安全,应该尽量避免使用。上面的SQL
语句高度依赖于表中列的定义次序,并且还依赖于其次序容易获得的信
息。即使可得到这种次序信息,也不能保证下一次表结构变动后各个列
保持完全相同的次序。因此,编写依赖于特定列次序的SQL语句是很不安
全的。如果这样做,有时难免会出问题。

编写 INSERT 语句的更安全(不过更烦琐)的方法如下:

```shell
MariaDB [test]> insert into customers(cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country,cust_contact,cust_email)
  -> values (null,'Pep E. Lapew','100 Main Street','Los Angeles','CA','90046','USA',null,null);
```

此例子完成与前一个 `INSERT` 语句完全相同的工作,但在表名后
的括号里明确地给出了列名。在插入行时,MySQL将用 `VALUES`
列表中的相应值填入列表中的对应项。 `VALUES` 中的第一个值对应于第一
个指定的列名。第二个值对应于第二个列名,如此等等。

因为提供了列名, `VALUES` 必须以其指定的次序匹配指定的列名,不
一定按各个列出现在实际表中的次序。其优点是,即使表的结构改变,
此 `INSERT` 语句仍然能正确工作。你会发现 `cust_id` 的` NULL` 值是不必要的,
`cust_id` 列并没有出现在列表中,所以不需要任何值。
下面的 `INSERT` 语句填充所有列(与前面的一样),但以一种不同的次
序填充。因为给出了列名,所以插入结果仍然正确:

```shell
MariaDB [test]> insert into customers(cust_contact,cust_email,cust_address,cust_city,cust_state,cust_zip,cust_country)
  -> values ('Pep E. Lapew',null,null,'100 Main Street','Los Angeles','CA','90046','USA');
```

**总是使用列的列表** 一般不要使用没有明确给出列的列表的
INSERT 语句。使用列的列表能使SQL代码继续发挥作用,即使
表结构发生了变化。

**仔细地给出值** 不管使用哪种 INSERT 语法,都必须给出
VALUES 的正确数目。如果不提供列名,则必须给每个表列提供
一个值。如果提供列名,则必须对每个列出的列给出一个值。
如果不这样,将产生一条错误消息,相应的行插入不成功。

使用这种语法,还可以省略列。这表示可以只给某些列提供值,给
其他列不提供值。
(事实上你已经看到过这样的例子:当列名被明确列出时, cust_id 可以省略)

**省略列** 如果表的定义允许,则可以在 INSERT 操作中省略某
些列。省略的列必须满足以下某个条件。
* 该列定义为允许 NULL 值(无值或空值)。
* 在表定义中给出默认值。这表示如果不给出值,将使用默认值。

如果对表中不允许 NULL 值且没有默认值的列不给出值,则
MySQL将产生一条错误消息,并且相应的行插入不成功。

**提高整体性能** 数据库经常被多个客户访问,对处理什么请
求以及用什么次序处理进行管理是MySQL的任务。 INSERT 操
作可能很耗时(特别是有很多索引需要更新时),而且它可能
降低等待处理的 SELECT 语句的性能。

如果数据检索是最重要的(通常是这样),则你可以通过在
INSERT 和 INTO 之间添加关键字 LOW_PRIORITY ,指示MySQL
降低 INSERT 语句的优先级,如下所示:`insert low_priority into`
顺便说一下,这也适用于下一章介绍的 UPDATE 和 DELETE 语句。

> 插入多个行

INSERT 可以插入一行到一个表中。但如果你想插入多个行怎么办?

可以使用多条 INSERT 语句,甚至一次提交它们,每条语句用一个分号结
束,或者,只要每条 INSERT 语句中的列名(和次序)相同,可以如下组
合各语句:

```shell
MariaDB [test]> insert into customers (cust_name,cust_address,cust_city,cust_state,cust_zip,cust_country)
    -> values ('Pep E. LaPew','100 Main Street','Los Angeles','CA','90046','USA'),
    -> ('M. Martian','42 Galaxy Way','New York','NY','11213','USA');
Query OK, 2 rows affected (0.05 sec)
Records: 2  Duplicates: 0  Warnings: 0

MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
```

其中单条 INSERT 语句有多组值,每组值用一对圆括号括起来,
用逗号分隔。

**提高 INSERT 的性能** 此技术可以提高数据库处理的性能,因
为MySQL用单条 INSERT 语句处理多个插入比使用多条 INSERT
语句快。

> 插入检索出的数据

INSERT 一般用来给表插入一个指定列值的行。但是, INSERT 还存在
另一种形式,可以利用它将一条 SELECT 语句的结果插入表中。这就是所
谓的 INSERT SELECT ,顾名思义,它是由一条 INSERT 语句和一条 SELECT
语句组成的。

假如你想从另一表中合并客户列表到你的 customers 表。不需要每次
读取一行,然后再将它用 INSERT 插入,可以如下进行:

**新例子的说明** 这个例子把一个名为 `custnew` 的表中的数据
导入 `customers` 表中。为了试验这个例子,应该首先创建和填
充 `custnew` 表。 `custnew` 表的结构与附录B中描述的 `customers`
表的相同。在填充 `custnew` 时,不应该使用已经在 `customers`
中使用过的 `cust_id` 值(如果主键值重复,后续的 `INSERT` 操作
将会失败)或仅省略这列值让MySQL在导入数据的过程中产
生新值。

```shell
MariaDB [test]> create table custnew as select * from customers;
Query OK, 8 rows affected (0.33 sec)
Records: 8  Duplicates: 0  Warnings: 0

MariaDB [test]> select * from custnew;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
8 rows in set (0.00 sec)

MariaDB [test]> desc custnew;
+--------------+-----------+------+-----+---------+-------+
| Field        | Type      | Null | Key | Default | Extra |
+--------------+-----------+------+-----+---------+-------+
| cust_id      | int(11)   | NO   |     | 0       |       |
| cust_name    | char(50)  | NO   |     | NULL    |       |
| cust_address | char(50)  | YES  |     | NULL    |       |
| cust_city    | char(50)  | YES  |     | NULL    |       |
| cust_state   | char(5)   | YES  |     | NULL    |       |
| cust_zip     | char(10)  | YES  |     | NULL    |       |
| cust_country | char(50)  | YES  |     | NULL    |       |
| cust_contact | char(50)  | YES  |     | NULL    |       |
| cust_email   | char(255) | YES  |     | NULL    |       |
+--------------+-----------+------+-----+---------+-------+
9 rows in set (0.00 sec)

MariaDB [test]> desc customers;
+--------------+-----------+------+-----+---------+----------------+
| Field        | Type      | Null | Key | Default | Extra          |
+--------------+-----------+------+-----+---------+----------------+
| cust_id      | int(11)   | NO   | PRI | NULL    | auto_increment |
| cust_name    | char(50)  | NO   |     | NULL    |                |
| cust_address | char(50)  | YES  |     | NULL    |                |
| cust_city    | char(50)  | YES  |     | NULL    |                |
| cust_state   | char(5)   | YES  |     | NULL    |                |
| cust_zip     | char(10)  | YES  |     | NULL    |                |
| cust_country | char(50)  | YES  |     | NULL    |                |
| cust_contact | char(50)  | YES  |     | NULL    |                |
| cust_email   | char(255) | YES  |     | NULL    |                |
+--------------+-----------+------+-----+---------+----------------+
9 rows in set (0.00 sec)

MariaDB [test]> select * from custnew;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
8 rows in set (0.00 sec)

MariaDB [test]> update custnew set cust_id=20001 where cust_id=10001;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20002 where cust_id=10002;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20003 where cust_id=10003;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20004 where cust_id=10004;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20005 where cust_id=10005;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20006 where cust_id=10006;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20007 where cust_id=10007;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> update custnew set cust_id=20008 where cust_id=10008;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> select * from custnew;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   20001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   20002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   20003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   20004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   20005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   20006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
8 rows in set (0.00 sec)

# 将custnew表中的数据导入customers表

MariaDB [test]> insert into customers select * from custnew;
Query OK, 8 rows affected (0.06 sec)
Records: 8  Duplicates: 0  Warnings: 0

MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
|   20001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   20002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   20003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   20004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   20005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   20006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
16 rows in set (0.00 sec)
```

这个例子使用 `INSERT SELECT` 从 `custnew` 中将所有数据导入
`customers` 。 `SELECT` 语句从 `custnew` 检索出要插入的值,而不
是列出它们。 `SELECT` 中列出的每个列对应于 `customers` 表名后所跟的列
表中的每个列。这条语句将插入多少行有赖于 `custnew` 表中有多少行。
如果这个表为空,则没有行被插入(也不产生错误,因为操作仍然是合
法的)。如果这个表确实含有数据,则所有数据将被插入到 `customers` 。

这个例子导入了 cust_id (假设你能够确保 cust_id 的值不重复)。
你也可以简单地省略这列(从 INSERT 和 SELECT 中),这样MySQL就会生成
新值。

**INSERT SELECT 中的列名** 为简单起见,这个例子在 INSERT 和
SELECT 语句中使用了相同的列名。但是,不一定要求列名匹配。
事实上,MySQL甚至不关心 SELECT 返回的列名。它使用的是
列的位置,因此 SELECT 中的第一列(不管其列名)将用来填充
表列中指定的第一个列,第二列将用来填充表列中指定的第二
个列,如此等等。这对于从使用不同列名的表中导入数据是非
常有用的。

INSERT SELECT 中 SELECT 语句可包含 WHERE 子句以过滤插入的数据。


> 小结
>> 本章介绍如何将行插入到数据库表。我们学习了使用 INSERT 的几种
方法,以及为什么要明确使用列名,学习了如何用 INSERT SELECT 从其他
表中导入行。下一章讲述如何使用 UPDATE 和 DELETE 进一步操纵表数据。

---

#### 更新和删除数据

本章介绍如何利用 UPDATE 和 DELETE 语句进一步操纵表数据。

> 更新数据

为了更新(修改)表中的数据,可使用 UPDATE 语句。可采用两种方
式使用 UPDATE :

* 更新表中特定行;
* 更新表中所有行。

下面分别对它们进行介绍。

**不要省略 WHERE 子句** 在使用 UPDATE 时一定要注意细心。因为
稍不注意,就会更新表中所有行。在使用这条语句前,请完整地阅读本节。

**UPDATE 与安全** 可以限制和控制 UPDATE 语句的使用

UPDATE 语句非常容易使用,甚至可以说是太容易使用了。基本的
UPDATE 语句由3部分组成,分别是:

* 要更新的表;
* 列名和它们的新值;
* 确定要更新行的过滤条件。

举一个简单例子。

>> 客户`cust_id=10005` 现在有了电子邮件地址`elmer@fudd.com`,因此他的记录需要更新,语句如下:

```shell
MariaDB [test]> update customers set cust_email = 'elmer@fudd.com' where cust_id = 10005;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> select cust_email from customers where cust_id =10005;
+----------------+
| cust_email     |
+----------------+
| elmer@fudd.com |
+----------------+
1 row in set (0.00 sec)
```

`UPDATE` 语句总是以要更新的表的名字开始。在此例子中,要更新的
表的名字为 `customers` 。 `SET` 命令用来将新值赋给被更新的列。如这里所
示, `SET` 子句设置 `cust_email` 列为指定的值:`set cust_email = 'elmer@fudd.com'`

`UPDATE `语句以 `WHERE` 子句结束,它告诉MySQL更新哪一行。没有
`WHERE` 子句,MySQL将会用这个电子邮件地址更新 `customers` 表中所有
行,这不是我们所希望的。

更新多个列的语法稍有不同:

```shell
MariaDB [test]> update customers set cust_name = 'The Fudds',cust_email = 'elmer@fudd.com' where cust_id = 10005;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> select * from customers where cust_id =10005;
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+----------------+
| cust_id | cust_name | cust_address     | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email     |
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+----------------+
|   10005 | The Fudds | 4545 53rd Street | Chicago   | IL         | 54545    | USA          | E Fudd       | elmer@fudd.com |
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+----------------+
1 row in set (0.00 sec)
```

在更新多个列时,只需要使用单个` SET` 命令,每个“列=值”对之间
用逗号分隔(最后一列之后不用逗号)。在此例子中,更新客户 `10005` 的
`cust_name` 和 `cust_email` 列。

**在 UPDATE 语句中使用子查询** UPDATE 语句中可以使用子查
询,使得能用 SELECT 语句检索出的数据更新列数据。

**IGNORE 关键字** 如果用 UPDATE 语句更新多行,并且在更新这些
行中的一行或多行时出一个现错误,则整个 UPDATE 操作被取消
(错误发生前更新的所有行被恢复到它们原来的值)。为即使是发
生错误,也继续进行更新,可使用 IGNORE 关键字,如下所示:
`UPDATE IGNORE customers...`


>> 为了删除某个列的值,可设置它为 NULL (假如表定义允许 NULL 值)。

```shell
MariaDB [test]> update customers set cust_email = null where cust_id = 10005;
Query OK, 1 row affected (0.05 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [test]> select * from customers where cust_id =10005;
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+------------+
| cust_id | cust_name | cust_address     | cust_city | cust_state | cust_zip | cust_country | cust_contact | cust_email |
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+------------+
|   10005 | The Fudds | 4545 53rd Street | Chicago   | IL         | 54545    | USA          | E Fudd       | NULL       |
+---------+-----------+------------------+-----------+------------+----------+--------------+--------------+------------+
1 row in set (0.00 sec)
```

其中 `NULL` 用来去除 `cust_email` 列中的值。

> 删除数据

为了从一个表中删除(去掉)数据,使用 DELETE 语句。可以两种方
式使用 DELETE :

* 从表中删除特定的行;
* 从表中删除所有行。

下面分别对它们进行介绍。

**不要省略 WHERE 子句** 在使用 DELETE 时一定要注意细心。因为
稍不注意,就会错误地删除表中所有行。在使用这条语句前,
请完整地阅读本节。

**DELETE 与安全** 可以限制和控制 DELETE 语句的使用

前面说过, UPDATE 非常容易使用,而 DELETE 更容易使用。
下面的语句从 customers 表中删除一行:

>> 只删除客户 10006

```shell
MariaDB [test]> delete from customers where cust_id = 10006;
Query OK, 1 row affected (0.04 sec)

MariaDB [test]> select * from customers;
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
| cust_id | cust_name      | cust_address        | cust_city   | cust_state | cust_zip | cust_country | cust_contact | cust_email          |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
|   10001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   10002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   10003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   10004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   10005 | The Fudds      | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   10007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   10008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
|   20001 | Coyote Inc.    | 200 Maple Lane      | Detroit     | MI         | 44444    | USA          | Y Lee        | ylee@coyote.com     |
|   20002 | Mouse House    | 333 Fromage Lane    | Columbus    | OH         | 43333    | USA          | Jerry Mouse  | NULL                |
|   20003 | Wascals        | 1 Sunny Place       | Muncie      | IN         | 42222    | USA          | Jim Jones    | rabbit@wascally.com |
|   20004 | Yosemite Place | 829 Riverside Drive | Phoenix     | AZ         | 88888    | USA          | Y Sam        | sam@yosemite.com    |
|   20005 | E Fudd         | 4545 53rd Street    | Chicago     | IL         | 54545    | USA          | E Fudd       | NULL                |
|   20006 | Pep E. Lapew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20007 | Pep E. LaPew   | 100 Main Street     | Los Angeles | CA         | 90046    | USA          | NULL         | NULL                |
|   20008 | M. Martian     | 42 Galaxy Way       | New York    | NY         | 11213    | USA          | NULL         | NULL                |
+---------+----------------+---------------------+-------------+------------+----------+--------------+--------------+---------------------+
15 rows in set (0.00 sec)
```

这条语句很容易理解。 `DELETE FROM` 要求指定从中删除数据的表名。
`WHERE` 子句过滤要删除的行。在这个例子中,只删除客户 `10006 `。
如果省略 `WHERE` 子句,它将删除表中每个客户。

`DELETE` 不需要列名或通配符。 `DELETE` 删除整行而不是删除列。为了
删除指定的列,请使用 `UPDATE` 语句。

**删除表的内容而不是表** `DELETE` 语句从表中删除行,甚至是
删除表中所有行。但是, `DELETE `不删除表本身。

**更快的删除** 如果想从表中删除所有行,不要使用 `DELETE` 。
可使用 `TRUNCATE TABLE` 语句,它完成相同的工作,但速度更
快( TRUNCATE 实际是删除原来的表并重新创建一个表,而不
是逐行删除表中的数据)。

> 更新和删除的指导原则

前一节中使用的 UPDATE 和 DELETE 语句全都具有 WHERE 子句,这样做的
理由很充分。如果省略了 WHERE 子句,则 UPDATE 或 DELETE 将被应用到表中
所有的行。换句话说,如果执行 UPDATE 而不带 WHERE 子句,则表中每个行
都将用新值更新。类似地,如果执行 DELETE 语句而不带 WHERE 子句,表的
所有数据都将被删除。

下面是许多SQL程序员使用 UPDATE 或 DELETE 时所遵循的习惯。
* 除非确实打算更新和删除每一行,否则绝对不要使用不带 WHERE
子句的 UPDATE 或 DELETE 语句。
* 保证每个表都有主键,尽可能像 WHERE 子句那样使用它(可以指定各主键、多个值或值的范围)。
* 在对 UPDATE 或 DELETE 语句使用 WHERE 子句前,应该先用 SELECT 进
行测试,保证它过滤的是正确的记录,以防编写的 WHERE 子句不
正确。
* 使用强制实施引用完整性的数据库,这样MySQL将不允许删除具有与其他表相关联的数据的行。

**小心使用** MySQL没有撤销(undo)按钮。应该非常小心地
使用 UPDATE 和 DELETE ,否则你会发现自己更新或删除了错误
的数据。

> 小结
>> 我们在本章中学习了如何使用 UPDATE 和 DELETE 语句处理表中的数
据。我们学习了这些语句的语法,知道了它们固有的危险性。本章中还
讲解了为什么 WHERE 子句对 UPDATE 和 DELETE 语句很重要,并且给出了应该
遵循的一些指导原则,以保证数据的安全。

---

### 使用视图

本章将介绍视图究竟是什么,它们怎样工作,何时使用它们。我们
还将看到如何利用视图简化前面章节中执行的某些SQL操作。

> 视图

**需要MySQL 5** MySQL 5添加了对视图的支持。因此,本章
内容适用于MySQL 5及以后的版本。

视图是虚拟的表。与包含数据的表不一样,视图只包含使用时动态
检索数据的查询。

理解视图的最好方法是看一个例子。用下面的 SELECT 语句从3个表中检索数据:

```shell
MariaDB [test]> select cust_name,cust_contact from customers,orders,orderitems
  -> where customers.cust_id=orders.cust_id and orderitems.order_num = orders.order_num
  -> and prod_id='TNT2';
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
2 rows in set (0.00 sec)
```

此查询用来检索订购了某个特定产品的客户。任何需要这个数据的
人都必须理解相关表的结构,并且知道如何创建查询和对表进行联结。
为了检索其他产品(或多个产品)的相同数据,必须修改最后的 WHERE 子
句。

现在,假如可以把整个查询包装成一个名为 productcustomers 的虚
拟表,则可以如下轻松地检索出相同的数据:`select cust_name,cust_contact from productcustomers where prod_id = 'TNT2';`

这就是视图的作用。 productcustomers 是一个视图,作为视图,它
不包含表中应该有的任何列或数据,它包含的是一个SQL查询(与上面用
以正确联结表的相同的查询)。

1.为什么使用视图

我们已经看到了视图应用的一个例子。下面是视图的一些常见应用。

* 重用SQL语句。
* 简化复杂的SQL操作。在编写查询后,可以方便地重用它而不必
知道它的基本查询细节。
* 使用表的组成部分而不是整个表。
* 保护数据。可以给用户授予表的特定部分的访问权限而不是整个
表的访问权限。
* 更改数据格式和表示。视图可返回与底层表的表示和格式不同的
数据。

在视图创建之后,可以用与表基本相同的方式利用它们。可以对视
图执行 SELECT 操作,过滤和排序数据,将视图联结到其他视图或表,甚
至能添加和更新数据(添加和更新数据存在某些限制。关于这个内容稍
后还要做进一步的介绍)。

重要的是知道视图仅仅是用来查看存储在别处的数据的一种设施。
视图本身不包含数据,因此它们返回的数据是从其他表中检索出来的。
在添加或更改这些表中的数据时,视图将返回改变过的数据。

**性能问题** 因为视图不包含数据,所以每次使用视图时,都
必须处理查询执行时所需的任一个检索。如果你用多个联结
和过滤创建了复杂的视图或者嵌套了视图,可能会发现性能
下降得很厉害。因此,在部署使用了大量视图的应用前,应
该进行测试。

2.视图的规则和限制

下面是关于视图创建和使用的一些最常见的规则和限制。

* 与表一样,视图必须唯一命名(不能给视图取与别的视图或表相
同的名字)。
* 对于可以创建的视图数目没有限制。
* 为了创建视图,必须具有足够的访问权限。这些限制通常由数据
库管理人员授予。
* 视图可以嵌套,即可以利用从其他视图中检索数据的查询来构造
一个视图。
* ORDER BY 可以用在视图中,但如果从该视图检索数据 SELECT 中也
含有 ORDER BY ,那么该视图中的 ORDER BY 将被覆盖。
* 视图不能索引,也不能有关联的触发器或默认值。
* 视图可以和表一起使用。例如,编写一条联结表和视图的 SELECT
语句。

> 使用视图

在理解什么是视图(以及管理它们的规则及约束)后,我们来看一
下视图的创建。

* 视图用 CREATE VIEW 语句来创建。
* 使用 SHOW CREATE VIEW viewname ;来查看创建视图的语句。
* 用 DROP 删除视图,其语法为 DROP VIEW viewname;。
* 更新视图时,可以先用DROP再用CREATE,也可以直接用CREATE OR
REPLACE VIEW。如果要更新的视图不存在,则第 2 条更新语句会创
建一个视图;如果要更新的视图存在,则第 2 条更新语句会替换原
有视图。

1.利用视图简化复杂的联结

视图的最常见的应用之一是隐藏复杂的SQL,这通常都会涉及联结。
请看下面的例子:

>> 创建一个名为 productcustomers 的视图,它联结三个
表,以返回已订购了任意产品的所有客户的列表。如果执行
SELECT * FROM productcustomers ,将列出订购了任意产品的客户。

```shell
MariaDB [test]> create view productcustomers as select cust_name,cust_contact,prod_id from customers,orders,orderitems where customers.cust_id=orders.cust_id and orderitems.order_num = orders.order_num ;
Query OK, 0 rows affected (0.06 sec)

MariaDB [test]> select * from productcustomers;
+----------------+--------------+---------+
| cust_name      | cust_contact | prod_id |
+----------------+--------------+---------+
| Coyote Inc.    | Y Lee        | ANV01   |
| Coyote Inc.    | Y Lee        | ANV02   |
| Coyote Inc.    | Y Lee        | TNT2    |
| Coyote Inc.    | Y Lee        | FB      |
| Coyote Inc.    | Y Lee        | FB      |
| Coyote Inc.    | Y Lee        | OL1     |
| Coyote Inc.    | Y Lee        | SLING   |
| Coyote Inc.    | Y Lee        | ANV03   |
| Wascals        | Jim Jones    | JP2000  |
| Yosemite Place | Y Sam        | TNT2    |
| The Fudds      | E Fudd       | FC      |
+----------------+--------------+---------+
11 rows in set (0.00 sec)
```

>> 检索订购了产品 TNT2 的客户

```shell
MariaDB [test]> select cust_name,cust_contact from productcustomers where prod_id='TNT2';
+----------------+--------------+
| cust_name      | cust_contact |
+----------------+--------------+
| Coyote Inc.    | Y Lee        |
| Yosemite Place | Y Sam        |
+----------------+--------------+
2 rows in set (0.00 sec)
```

这条语句通过 WHERE 子句从视图中检索特定数据。在MySQL处
理此查询时,它将指定的 WHERE 子句添加到视图查询中的已有
WHERE 子句中,以便正确过滤数据。

可以看出,视图极大地简化了复杂SQL语句的使用。利用视图,可一
次性编写基础的SQL,然后根据需要多次使用。

**创建可重用的视图** 创建不受特定数据限制的视图是一种
好办法。例如,上面创建的视图返回生产所有产品的客户而
不仅仅是 生产TNT2 的客户。扩展视图的范围不仅使得它能被
重用,而且甚至更有用。这样做不需要创建和维护多个类似
视图。

2.用视图重新格式化检索出的数据

如上所述,视图的另一常见用途是重新格式化检索出的数据。下面
的 SELECT 语句在单个组合计算列中返回供应商名和位置:

```shell
MariaDB [test]> select concat(rtrim(vend_name),' (',rtrim(vend_country),')')
    -> as vend_title
    -> from vendors
    -> order by vend_name;
+-------------------------+
| vend_title              |
+-------------------------+
| ACME (USA)              |
| Anvils R Us (USA)       |
| Furball Inc. (USA)      |
| Jet Set (England)       |
| Jouets Et Ours (France) |
| LT Supplies (USA)       |
+-------------------------+
6 rows in set (0.00 sec)
```

现在,假如经常需要这个格式的结果。不必在每次需要时执行联结,
创建一个视图,每次需要时使用它即可。为把此语句转换为视图,可按
如下进行:

```shell
MariaDB [test]> create view vendorlocation as
    -> select concat(rtrim(vend_name),' (',rtrim(vend_country),')')
    -> as vend_title
    -> from vendors
    -> order by vend_name;
Query OK, 0 rows affected (0.32 sec)
```

这条语句使用与以前的 SELECT 语句相同的查询创建视图。为了
检索出以创建所有邮件标签的数据,可如下进行:

```shell
MariaDB [test]> select * from vendorlocation;
+-------------------------+
| vend_title              |
+-------------------------+
| ACME (USA)              |
| Anvils R Us (USA)       |
| Furball Inc. (USA)      |
| Jet Set (England)       |
| Jouets Et Ours (France) |
| LT Supplies (USA)       |
+-------------------------+
6 rows in set (0.00 sec)
```

3.用视图过滤不想要的数据

视图对于应用普通的WHERE子句也很有用。

>> 定义customeremaillist 视图,它过滤没有电子邮件地址的客户。为此目的,
可使用下面的语句:

```shell
MariaDB [test]> create view customeremaillist as
    -> select cust_id,cust_name,cust_email
    -> from customers
    -> where cust_email is not null;
Query OK, 0 rows affected (0.05 sec)

MariaDB [test]> select * from customeremaillist;
+---------+----------------+---------------------+
| cust_id | cust_name      | cust_email          |
+---------+----------------+---------------------+
|   10001 | Coyote Inc.    | ylee@coyote.com     |
|   10003 | Wascals        | rabbit@wascally.com |
|   10004 | Yosemite Place | sam@yosemite.com    |
|   20001 | Coyote Inc.    | ylee@coyote.com     |
|   20003 | Wascals        | rabbit@wascally.com |
|   20004 | Yosemite Place | sam@yosemite.com    |
+---------+----------------+---------------------+
6 rows in set (0.00 sec)
```

在发送电子邮件到邮件列表时,需要排除没有电子邮件
地址的用户。这里的 WHERE 子句过滤了 cust_email 列中具有
NULL 值的那些行,使他们不被检索出来。

**WHERE 子句与 WHERE 子句** 如果从视图检索数据时使用了一条
WHERE 子句,则两组子句(一组在视图中,另一组是传递给视
图的)将自动组合。

4.使用视图与计算字段

视图对于简化计算字段的使用特别有用。

>> 检索某个特定订单中的物品,计算每种物品的总价格

```shell
MariaDB [test]> select prod_id,quantity,item_price,quantity*item_price as expanded_price
    -> from orderitems
    -> where order_num = 20005;
+---------+----------+------------+----------------+
| prod_id | quantity | item_price | expanded_price |
+---------+----------+------------+----------------+
| ANV01   |       10 |       5.99 |          59.90 |
| ANV02   |        3 |       9.99 |          29.97 |
| TNT2    |        5 |      10.00 |          50.00 |
| FB      |        1 |      10.00 |          10.00 |
+---------+----------+------------+----------------+
4 rows in set (0.00 sec)
```

为将其转换为一个视图,如下进行:

```shell
MariaDB [test]> create view orderitemsexpanded as
    -> select prod_id,quantity,item_price,quantity*item_price as expanded_price
    -> from orderitems;
Query OK, 0 rows affected (0.06 sec)
```

为检索订单 20005 的详细内容(上面的输出),如下进行:

```shell
MariaDB [test]> create view orderitemsexpanded as
    -> select order_num,prod_id,quantity,item_price,quantity*item_price as expanded_price
    -> from orderitems;
Query OK, 0 rows affected (0.04 sec)

MariaDB [test]> select * from orderitemsexpanded where order_num = 20005;
+-----------+---------+----------+------------+----------------+
| order_num | prod_id | quantity | item_price | expanded_price |
+-----------+---------+----------+------------+----------------+
|     20005 | ANV01   |       10 |       5.99 |          59.90 |
|     20005 | ANV02   |        3 |       9.99 |          29.97 |
|     20005 | TNT2    |        5 |      10.00 |          50.00 |
|     20005 | FB      |        1 |      10.00 |          10.00 |
+-----------+---------+----------+------------+----------------+
4 rows in set (0.00 sec)
```

可以看到,视图非常容易创建,而且很好使用。正确使用,视图可
极大地简化复杂的数据处理。

5.更新视图

迄今为止的所有视图都是和 SELECT 语句使用的。然而,视图的数据
能否更新?答案视情况而定。

通常,视图是可更新的(即,可以对它们使用 INSERT 、 UPDATE 和
DELETE )。更新一个视图将更新其基表(可以回忆一下,视图本身没有数
据)。如果你对视图增加或删除行,实际上是对其基表增加或删除行。

但是,并非所有视图都是可更新的。基本上可以说,如果MySQL不
能正确地确定被更新的基数据,则不允许更新(包括插入和删除)。
这实际上意味着,如果视图定义中有以下操作,则不能进行视图的更新:

* 分组(使用 GROUP BY 和 HAVING );
* 联结;
* 子查询;
* 并;
* 聚集函数( Min() 、 Count() 、 Sum() 等);
* DISTINCT;
* 导出(计算)列。

换句话说,本章许多例子中的视图都是不可更新的。这听上去好像
是一个严重的限制,但实际上不是,因为视图主要用于数据检索。

**可能的变动** 上面列出的限制自MySQL 5以来是正确的。不
过,未来的MySQL很可能会取消某些限制。

**将视图用于检索** 一般,应该将视图用于检索( SELECT 语句)
而不用于更新( INSERT 、 UPDATE 和 DELETE )。

> 小结
>> 视图为虚拟的表。它们包含的不是数据而是根据需要检索数据的查
询。视图提供了一种MySQL的 SELECT 语句层次的封装,可用来简化数据
处理以及重新格式化基础数据或保护基础数据。

---

### 使用存储过程

本章介绍什么是存储过程,为什么要使用存储过程以及如何使用存
储过程,并且介绍创建和使用存储过程的基本语法。

> 存储过程

**需要MySQL 5** MySQL 5添加了对存储过程的支持,因此,
本章内容适用于MySQL 5及以后的版本。

迄今为止,使用的大多数SQL语句都是针对一个或多个表的单条语
句。并非所有操作都这么简单,经常会有一个完整的操作需要多条语句
才能完成。例如,考虑以下的情形。

* 为了处理订单,需要核对以保证库存中有相应的物品。
* 如果库存有物品,这些物品需要预定以便不将它们再卖给别的人,
并且要减少可用的物品数量以反映正确的库存量。
* 库存中没有的物品需要订购,这需要与供应商进行某种交互。
* 关于哪些物品入库(并且可以立即发货)和哪些物品退订,需要
通知相应的客户。

这显然不是一个完整的例子,它甚至超出了本书中所用样例表的范
围,但足以帮助表达我们的意思了。执行这个处理需要针对许多表的多
条MySQL语句。此外,需要执行的具体语句及其次序也不是固定的,它
们可能会(和将)根据哪些物品在库存中哪些不在而变化。

那么,怎样编写此代码?可以单独编写每条语句,并根据结果有条
件地执行另外的语句。在每次需要这个处理时(以及每个需要它的应用
中)都必须做这些工作。

可以创建存储过程。存储过程简单来说,就是为以后的使用而保存
的一条或多条MySQL语句的集合。可将其视为批文件,虽然它们的作用
不仅限于批处理。


> 为什么要使用存储过程

既然我们知道了什么是存储过程,那么为什么要使用它们呢?有许
多理由,下面列出一些主要的理由。

* 通过把处理封装在容易使用的单元中,简化复杂的操作(正如前
面例子所述)。
* 由于不要求反复建立一系列处理步骤,这保证了数据的完整性。
如果所有开发人员和应用程序都使用同一(试验和测试)存储过
程,则所使用的代码都是相同的。

这一点的延伸就是防止错误。需要执行的步骤越多,出错的可能
性就越大。防止错误保证了数据的一致性。

* 简化对变动的管理。如果表名、列名或业务逻辑(或别的内容)
有变化,只需要更改存储过程的代码。使用它的人员甚至不需要
知道这些变化。

这一点的延伸就是安全性。通过存储过程限制对基础数据的访问减
少了数据讹误(无意识的或别的原因所导致的数据讹误)的机会。

* 提高性能。因为使用存储过程比使用单独的SQL语句要快。
* 存在一些只能用在单个请求中的MySQL元素和特性,存储过程可
以使用它们来编写功能更强更灵活的代码(在下一章的例子中可
以看到。)

换句话说,使用存储过程有3个主要的好处,即简单、安全、高性能。
显然,它们都很重要。不过,在将SQL代码转换为存储过程前,也必须知
道它的一些缺陷。

* 一般来说,存储过程的编写比基本SQL语句复杂,编写存储过程
需要更高的技能,更丰富的经验。
* 你可能没有创建存储过程的安全访问权限。许多数据库管理员限
制存储过程的创建权限,允许用户使用存储过程,但不允许他们
创建存储过程。

尽管有这些缺陷,存储过程还是非常有用的,并且应该尽可能地使用。

**不能编写存储过程?你依然可以使用** MySQL将编写存储过
程的安全和访问与执行存储过程的安全和访问区分开来。这
是好事情。即使你不能(或不想)编写自己的存储过程,也
仍然可以在适当的时候执行别的存储过程。

> 使用存储过程

使用存储过程需要知道如何执行(运行)它们。存储过程的执行远
比其定义更经常遇到,因此,我们将从执行存储过程开始介绍。然后再
介绍创建和使用存储过程。

1.执行存储过程

MySQL称存储过程的执行为调用,因此MySQL执行存储过程的语句
为 CALL 。 CALL 接受存储过程的名字以及需要传递给它的任意参数。请看
以下例子:

>> 执行名为 productpricing 的存储过程,它计算并返回产
品的最低、最高和平均价格。

```shell
MariaDB [test]> call productpricing (@pricelow,@pricehigh,@priceaverage);
```

2.创建存储过程

正如所述,编写存储过程并不是微不足道的事情。为让你了解这个
过程,请看一个例子。

>> 返回产品平均价格的存储过程

```shell
MariaDB [test]> create procedure productpricing() begin select avg(prod_price) as priceaverage from products; end;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'end' at line 1

MariaDB [test]> delimiter //
MariaDB [test]> create procedure productpricing() begin select avg(prod_price) as priceaverage from products; end //
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

我们稍后介绍第一条和最后一条语句。此存储过程名为
`productpricing` ,用 `CREATE PROCEDURE productpricing()` 语
句定义。如果存储过程接受参数,它们将在 `()` 中列举出来。此存储过程没
有参数,但后跟的 `()` 仍然需要。 `BEGIN` 和 `END` 语句用来限定存储过程体,过
程体本身仅是一个简单的 `SELECT` 语句。

在MySQL处理这段代码时,它创建一个新的存储过程 `product-pricing` 。
没有返回数据,因为这段代码并未调用存储过程,这里只是为
以后使用而创建它。

**mysql 命令行客户机的分隔符** 如果你使用的是 mysql 命令行
实用程序,应该仔细阅读此说明。

默认的MySQL语句分隔符为 ; (正如你已经在迄今为止所使用
的MySQL语句中所看到的那样)。 mysql 命令行实用程序也使
用 ; 作为语句分隔符。如果命令行实用程序要解释存储过程自
身内的 ; 字符,则它们最终不会成为存储过程的成分,这会使
存储过程中的SQL出现句法错误。

解决办法是临时更改命令行实用程序的语句分隔符,如下所示

```shell
delimiter//
create procedure productpricing()
begin
  select avg(prod_price) as priceaverage
  from products;
end //
delimiter;
```
其中, `DELIMITER //` 告诉命令行实用程序使用` //` 作为新的语
句结束分隔符,可以看到标志存储过程结束的 `END` 定义为 `END
//` 而不是 `END;` 。这样,存储过程体内的 `;` 仍然保持不动,并且
正确地传递给数据库引擎。最后,为恢复为原来的语句分隔符,
可使用 `DELIMITER ;` 。
除 `\ `符号外,任何字符都可以用作语句分隔符。
如果你使用的是 mysql 命令行实用程序,在阅读本章时请记住
这里的内容。

那么,如何使用这个存储过程?如下所示:

```shell
MariaDB [test]> call productpricing();
+--------------+
| priceaverage |
+--------------+
|    16.133571 |
+--------------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```

`CALL productpricing();` 执行刚创建的存储过程并显示返回
的结果。因为存储过程实际上是一种函数,所以存储过程名后
需要有 () 符号(即使不传递参数也需要)。

3.删除存储过程

存储过程在创建之后,被保存在服务器上以供使用,直至被删除。
删除命令从服务器中删除存储过程。

>> 为删除刚创建的存储过程,可使用以下语句:

```shell
MariaDB [test]> drop procedure productpricing;
Query OK, 0 rows affected (0.00 sec)
```

这条语句删除刚创建的存储过程。请注意没有使用后面的 `()` ,
只给出存储过程名。

**仅当存在时删除** 如果指定的过程不存在,则 `DROP PROCEDURE`
将产生一个错误。当过程存在想删除它时(如果过程不存在也
不产生错误)可使用 `DROP PROCEDURE IF EXISTS`。

```shell
MariaDB [test]> DROP PROCEDURE IF EXISTS productpricing;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

4.使用参数

productpricing 只是一个简单的存储过程,它简单地显示 SELECT 语
句的结果。一般,存储过程并不显示结果,而是把结果返回给你指定的变量。

**变量(variable)** 内存中一个特定的位置,用来临时存储数据。

>> 以下是 productpricing 的修改版本(如果不先删除此存储过程,则
不能再次创建它):

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure productpricing(out pl decimal (8,2),out ph decimal(8,2),out pa decimal(8,2)) begin select min(prod_price) into pl from products;select max(prod_price) into ph from products;select avg(prod_price) into pa from products;  end//
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

此存储过程接受3个参数: pl 存储产品最低价格, ph 存储产品
最高价格, pa 存储产品平均价格。每个参数必须具有指定的类
型,这里使用十进制值。关键字 OUT 指出相应的参数用来从存储过程传出
一个值(返回给调用者)。MySQL支持 IN (传递给存储过程)
、 OUT (从存储过程传出,如这里所用)和 INOUT (对存储过程传入和传出)类型的参
数。存储过程的代码位于 BEGIN 和 END 语句内,如前所见,它们是一系列
SELECT 语句,用来检索值,然后保存到相应的变量(通过指定 INTO 关键字)。

**参数的数据类型** 存储过程的参数允许的数据类型与表中使用
的数据类型相同。附录D列出了这些类型。
注意,记录集不是允许的类型,因此,不能通过一个参数返回
多个行和列。这就是前面的例子为什么要使用3个参数(和3条 SELECT 语句)的原因。

>> 为调用此修改过的存储过程,必须指定3个变量名,如下所示:

```shell
MariaDB [test]> call productpricing(@pricelow,@pricehigh,@priceaverage);
Query OK, 1 row affected, 1 warning (0.00 sec)

MariaDB [test]> call productpricing(@pl,@ph,@pa);
Query OK, 1 row affected, 1 warning (0.00 sec)
```

由于此存储过程要求3个参数,因此必须正好传递3个参数,不
多也不少。所以,这条 CALL 语句给出3个参数。它们是存储过
程将保存结果的3个变量的名字。

**变量名** 所有MySQL变量都必须以 `@` 开始。

在调用时,这条语句并不显示任何数据。它返回以后可以显示(或
在其他处理中使用)的变量。

>> 为了显示检索出的产品平均价格,可如下进行:

```shell
MariaDB [test]> select @pl;
+------+
| @pl  |
+------+
| 2.50 |
+------+
1 row in set (0.00 sec)

MariaDB [test]> select @pricelow;
+-----------+
| @pricelow |
+-----------+
|      2.50 |
+-----------+
1 row in set (0.00 sec)
```

>> 为了获得3个值,可使用以下语句:

```shell
MariaDB [test]> select @pl,@ph,@pa;
+------+-------+-------+
| @pl  | @ph   | @pa   |
+------+-------+-------+
| 2.50 | 55.00 | 16.13 |
+------+-------+-------+
1 row in set (0.00 sec)
```

>> 下面是另外一个例子,这次使用 IN 和 OUT 参数。 ordertotal 接受订单
号并返回该订单的合计:

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure ordertotal(in onumber int,out ototal decimal(8,2)) begin select sum(item_price*quantity) from orderitems where order_num=onumber into ototal; end//
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

`onumber` 定义为 `IN` ,因为订单号被传入存储过程。 `ototal` 定义
为 `OUT` ,因为要从存储过程返回合计。 `SELECT` 语句使用这两个
参数, `WHERE` 子句使用 `onumber` 选择正确的行, `INTO` 使用 `ototal` 存储计算
出来的合计。

>> 为调用这个新存储过程,可使用以下语句:

```shell
MariaDB [test]> call ordertotal(20005,@total);
Query OK, 1 row affected (0.00 sec)
```

必须给 ordertotal 传递两个参数;第一个参数为订单号,第二
个参数为包含计算出来的合计的变量名。

>> 为了显示此合计,可如下进行:

```shell
MariaDB [test]> select @total;
+--------+
| @total |
+--------+
| 149.87 |
+--------+
1 row in set (0.00 sec)
```

@total 已由 ordertotal 的 CALL 语句填写, SELECT 显示它包含
的值。

>> 为了得到另一个订单的合计显示,需要再次调用存储过程,然后重
新显示变量:

```shell
MariaDB [test]> call ordertotal(20009,@total);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> select @total;
+--------+
| @total |
+--------+
|  38.47 |
+--------+
1 row in set (0.00 sec)
```

`into 变量`的位置可以在`from`前面也可以在最后
```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure ordertotal0(in onumber int,out xtotal decimal(8,2)) begin select sum(item_price*quantity) into xtotal from orderitems where order_num=onumber ; end//
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
MariaDB [test]> call ordertotal0;
ERROR 1318 (42000): Incorrect number of arguments for PROCEDURE test.ordertotal0; expected 2, got 0
MariaDB [test]> call ordertotal0(20005,@total);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> select @total;
+--------+
| @total |
+--------+
| 149.87 |
+--------+
1 row in set (0.00 sec)
```

5.建立智能存储过程

迄今为止使用的所有存储过程基本上都是封装MySQL简单的 SELECT
语句。虽然它们全都是有效的存储过程例子,但它们所能完成的工作你
直接用这些被封装的语句就能完成(如果说它们还能带来更多的东西,
那就是使事情更复杂)。只有在存储过程内包含业务规则和智能处理时,
它们的威力才真正显现出来。

考虑这个场景。你需要获得与以前一样的订单合计,但需要对合计
增加营业税,不过只针对某些顾客(或许是你所在州中那些顾客)
。那么,你需要做下面几件事情:

* 获得合计(与以前一样);
* 把营业税有条件地添加到合计;
* 返回合计(带或不带税)。

存储过程的完整工作如下:

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure ordertotal(
    -> in onumber int,
    -> in taxable boolean,
    -> out ototal decimal(8,2)) comment 'Obtain order total, optionally adding tax'
    -> begin
    -> declare total decimal(8,2);
    -> declare taxrate int default 6;
    -> select sum(item_price*quantity)
    -> from orderitems
    -> where order_num = onumber
    -> into total;
    -> if taxable then
    -> select total+(total/100*taxrate) into total;
    -> end if;
    -> select total into ototal;
    -> end //
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;


[root@mastera0 ~]# vi procedure.mysql
[root@mastera0 ~]# mysql -uroot -puplooking < procedure.mysql
[root@mastera0 ~]# cat procedure.mysql
use test;
delimiter //
create procedure ordertotal(
in onumber int,
in taxable boolean,
out ototal decimal(8,2)
)

begin
declare total decimal(8,2);
declare taxrate int default 6;

select sum(item_price*quantity)
from orderitems
where order_num = onumber
into total;

if taxable then
select total+(total/100*taxrate) into total;
end if;

select total into ototal;
end //
delimiter ;

```

此存储过程有很大的变动。首先,增加了注释(前面放置 `-- `)。
在存储过程复杂性增加时,这样做特别重要。添加了另外一个
参数 `taxable` ,它是一个布尔值(如果要增加税则为真,否则为假)。在
存储过程体中,用 `DECLARE `语句定义了两个局部变量。 `DECLARE` 要求指定
变量名和数据类型,它也支持可选的默认值(这个例子中的 `taxrate` 的默
认被设置为 6% )。 `SELECT` 语句已经改变,因此其结果存储到 `total` (局部
变量)而不是 `ototal` 。 `IF` 语句检查 `taxable` 是否为真,如果为真,则用另
一 `SELECT `语句增加营业税到局部变量 `total` 。最后,用另一 `SELECT` 语句将
`total` (它增加或许不增加营业税)保存到 `ototal `。

**COMMENT 关键字** 本例子中的存储过程在 `CREATE PROCEDURE `语
句中包含了一个 `COMMENT` 值。它不是必需的,但如果给出,将
在 `SHOW PROCEDURE STATUS` 的结果中显示。

这显然是一个更高级,功能更强的存储过程。为试验它,请用以下
两条语句:

```shell
MariaDB [test]> call ordertotal(20005,0,@total);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> select @total;
+--------+
| @total |
+--------+
| 149.87 |
+--------+
1 row in set (0.00 sec)
```

BOOLEAN 值指定为 1 表示真,指定为 0 表示假(实际上,非零值
都考虑为真,只有 0 被视为假)。通过给中间的参数指定 0 或 1 ,
可以有条件地将营业税加到订单合计上。

**IF 语句** 这个例子给出了MySQL的 IF 语句的基本用法。 IF 语
句还支持 ELSEIF 和 ELSE 子句(前者还使用 THEN 子句,后者不
使用)。在以后章节中我们将会看到 IF 的其他用法(以及其他
流控制语句)。

6.检查存储过程

>> 为显示用来创建一个存储过程的 CREATE 语句,使用 SHOW CREATE
PROCEDURE 语句:

```shell
MariaDB [test]> show create procedure ordertotal\G;
*************************** 1. row ***************************
           Procedure: ordertotal
            sql_mode:
    Create Procedure: CREATE DEFINER=`root`@`localhost` PROCEDURE `ordertotal`(
in onumber int,
in taxable boolean,
out ototal decimal(8,2))
    COMMENT 'Obtain order total, optionally adding tax'
begin
declare total decimal(8,2);
declare taxrate int default 6;
select sum(item_price*quantity)
from orderitems
where order_num = onumber
into total;
if taxable then
select total+(total/100*taxrate) into total;
end if;
select total into ototal;
end
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
1 row in set (0.00 sec)
```

为了获得包括何时、由谁创建等详细信息的存储过程列表,使用 `SHOW
PROCEDURE STATUS` 。

**限制过程状态结果** `SHOW PROCEDURE STATUS `列出所有存储过
程。为限制其输出,可使用 `LIKE` 指定一个过滤模式,
例如:`show procedure status like 'ordertotal'; `

> 小结
>> 本章介绍了什么是存储过程以及为什么要使用存储过程。我们介绍
了存储过程的执行和创建的语法以及使用存储过程的一些方法。下一章
我们将继续这个话题。

---

### 使用游标

本章将讲授什么是游标以及如何使用游标。

> 游标

**需要MySQL 5** MySQL 5添加了对游标的支持,因此,本章内容
适用于MySQL 5及以后的版本。

由前几章可知,MySQL检索操作返回一组称为结果集的行。这组返
回的行都是与SQL语句相匹配的行(零行或多行)。使用简单的 SELECT 语句,
例如,没有办法得到第一行、下一行或前10行,也不存在每次一行
地处理所有行的简单方法(相对于成批地处理它们)。

有时,需要在检索出来的行中前进或后退一行或多行。这就是使用
游标的原因。游标(cursor)是一个存储在MySQL服务器上的数据库查询,
它不是一条 SELECT 语句,而是被该语句检索出来的结果集。在存储了游
标之后,应用程序可以根据需要滚动或浏览其中的数据。

游标主要用于交互式应用,其中用户需要滚动屏幕上的数据,并对
数据进行浏览或做出更改。

**只能用于存储过程** 不像多数DBMS,MySQL游标只能用于
存储过程(和函数)。

> 使用游标

使用游标涉及几个明确的步骤。

* 在能够使用游标前,必须声明(定义)它。这个过程实际上没有
检索数据,它只是定义要使用的 SELECT 语句。
* 一旦声明后,必须打开游标以供使用。这个过程用前面定义的
SELECT 语句把数据实际检索出来。
* 对于填有数据的游标,根据需要取出(检索)各行。
* 在结束游标使用时,必须关闭游标。

在声明游标后,可根据需要频繁地打开和关闭游标。在游标打开后,
可根据需要频繁地执行取操作。

1.创建游标

游标用 DECLARE 语句创建。 DECLARE 命名游标,并定义
相应的 SELECT 语句,根据需要带 WHERE 和其他子句。

>> 定义名为 ordernumbers 的游标,使用了可以检索所有订单的 SELECT 语句。

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure processorders()
    -> begin
    -> declare ordernumbers cursor
    -> for
    -> select order_num from orders;
    -> end //
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

这个存储过程并没有做很多事情, `DECLARE` 语句用来定义和命
名游标,这里为 `ordernumbers` 。 存储过程处理完成后,游标就
消失(因为它局限于存储过程)。

在定义游标之后,可以打开它。

2.打开和关闭游标

>> 游标用 OPEN CURSOR 语句来打开:`open ordernumbers;`

在处理 OPEN 语句时执行查询,存储检索出的数据以供浏览和滚
动。

>> 游标处理完成后,应当使用如下语句关闭游标:`close ordernumbers;`

CLOSE 释放游标使用的所有内部内存和资源,因此在每个游标
不再需要时都应该关闭。

在一个游标关闭后,如果没有重新打开,则不能使用它。但是,使
用声明过的游标不需要再次声明,用 OPEN 语句打开它就可以了。

**隐含关闭** 如果你不明确关闭游标, MySQL将会在到达 END 语
句时自动关闭它。

下面是前面例子的修改版本:

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure processorders()
    -> begin
    -> declare ordernumbers cursor
    -> for
    -> select order_num from orders;
    -> open ordernumbers;
    -> close ordernumbers;
    -> end //
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

这个存储过程声明、打开和关闭一个游标。但对检索出的数据
什么也没做。

3.使用游标数据

在一个游标被打开后,可以使用 FETCH 语句分别访问它的每一行。
FETCH 指定检索什么数据(所需的列),检索出来的数据存储在什么地方。
它还向前移动游标中的内部行指针,使下一条 FETCH 语句检索下一行(不
重复读取同一行)。

>> 第一个例子从游标中检索单个行(第一行):

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure processorders() begin declare o int;declare ordernumbers cursor for select order_num from orders;
    -> open ordernumbers;
    -> fetch ordernumbers into o;
    -> close ordernumbers;
    -> end //
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;

```

其中 `FETCH `用来检索当前行的 `order_num` 列(将自动从第一行开
始)到一个名为 `o` 的局部声明的变量中。对检索出的数据不做
任何处理。

>> 在下一个例子中,循环检索数据,从第一行到最后一行:

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure processorders() begin declare done boolean default 0;declare o int;declare ordernumbers cursor for select order_num from orders;declare continue handler for sqlstate '02000' set done=1; open ordernumbers;repeat fetch ordernumbers into o;until done end repeat; close ordernumbers; end//
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;
```

与前一个例子一样,这个例子使用 `FETCH `检索当前 `order_num`
到声明的名为 `o` 的变量中。但与前一个例子不一样的是,这个
例子中的 `FETCH` 是在 `REPEAT` 内,因此它反复执行直到 `done` 为真
(由 `UNTIL done END REPEAT;` 规定)。

为使它起作用,用一个 `DEFAULT 0 `(假,不结束)定义变量 `done` 。
那么, `done` 怎样才能在结束时被设置为真呢?
答案是用以下语句:`declare conitinue handler for sqlstate '02000' set done=1`
这条语句定义了一个 C`ONTINUE HANDLER` ,它是在条件出现时被执行
的代码。这里,它指出当 `SQLSTATE '02000'` 出现时, `SET done=1`。
`SQLSTATE '02000'` 是一个未找到条件,当 `REPEAT` 由于没有更多的行供循环而不能继
续时,出现这个条件。

**MySQL的错误代码** 关于MySQL 5使用的MySQL错误代码列
表,请参阅http://dev.mysql.com/doc/mysql/en/error-handling.html。

**DECLARE 语句的次序** DECLARE 语句的发布存在特定的次序。
用 DECLARE 语句定义的局部变量必须在定义任意游标或句柄
之前定义,而句柄必须在游标之后定义。不遵守此顺序将产
生错误消息。

如果调用这个存储过程,它将定义几个变量和一个 `CONTINUE HANDLER` ,
定义并打开一个游标,重复读取所有行,然后关闭游标。
如果一切正常,你可以在循环内放入任意需要的处理(在 FETCH 语句之后,循环结束之前)。

**重复或循环?** 除这里使用的` REPEAT` 语句外,MySQL还支持
循环语句,它可用来重复执行代码,直到使用 `LEAVE` 语句手动
退出为止。通常 `REPEAT` 语句的语法使它更适合于对游标进行循
环。

为了把这些内容组织起来,下面给出我们的游标存储过程样例的更
进一步修改的版本,这次对取出的数据进行某种实际的处理:

```shell
MariaDB [test]> delimiter //
MariaDB [test]> create procedure processorders() begin declare done boolean default 0;declare o int;declare t decimal(8,2);declare ordernumbers cursor for select order_num from orders;declare continue handler for sqlstate '02000' set done=1; create table if not exists ordertotals (order_num int,total decimal(8,2)); open ordernumbers;repeat fetch ordernumbers into o;call ordertotal(o,1,t);insert into ordertotals(order_num,total) values (o,t);until done end repeat; close ordernumbers; end//
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delimiter ;

```

在这个例子中,我们增加了另一个名为 `t` 的变量(存储每个订单的合计)。
此存储过程还在运行中创建了一个新表(如果它不存在的话),名为 `ordertotals` 。
这个表将保存存储过程生成的结果。 `FETCH`像以前一样取每个 `order_num` ,
然后用 `CALL `执行另一个存储过程(我们在前一章中创建)来计算每个订单的带税的合计(结果存储到 `t` )。
最后,用 `INSERT` 保存每个订单的订单号和合计。

此存储过程不返回数据,但它能够创建和填充另一个表,可以用一
条简单的 SELECT 语句查看该表:

```shell
MariaDB [test]> call processorders();
Query OK, 1 row affected (0.48 sec)

MariaDB [test]> select * from ordertotals;
+-----------+---------+
| order_num | total   |
+-----------+---------+
|     20005 |  158.86 |
|     20009 |   40.78 |
|     20006 |   58.30 |
|     20007 | 1060.00 |
|     20008 |  132.50 |
|     20008 |  132.50 |
+-----------+---------+
6 rows in set (0.00 sec)
```

这样,我们就得到了存储过程、游标、逐行处理以及存储过程调用
其他存储过程的一个完整的工作样例。

> 小结
>> 本章介绍了什么是游标以及为什么要使用游标,举了演示基本游标
使用的例子,并且讲解了对游标结果进行循环以及逐行处理的技术。

---

### 使用触发器

本章学习什么是触发器,为什么要使用触发器以及如何使用触发器。
本章还介绍创建和使用触发器的语法。

> 触发器

**需要MySQL 5** 对触发器的支持是在MySQL 5中增加的。因
此,本章内容适用于MySQL 5或之后的版本。

MySQL语句在需要时被执行,存储过程也是如此。但是,如果你
想要某条语句(或某些语句)在事件发生时自动执行,怎么办呢?例
如:

* 每当增加一个顾客到某个数据库表时,都检查其电话号码格式是
否正确,州的缩写是否为大写;
* 每当订购一个产品时,都从库存数量中减去订购的数量;
* 无论何时删除一行,都在某个存档表中保留一个副本。

所有这些例子的共同之处是它们都需要在某个表发生更改时自动
处理。这确切地说就是触发器。触发器是MySQL响应以下任意语句而
自动执行的一条MySQL语句(或位于 BEGIN 和 END 语句之间的一组语
句):

* DELETE ;
* INSERT ;
* UPDATE 。

其他MySQL语句不支持触发器。


> 创建触发器

在创建触发器时,需要给出4条信息:

* 唯一的触发器名;
* 触发器关联的表;
* 触发器应该响应的活动( DELETE 、 INSERT 或 UPDATE );
* 触发器何时执行(处理之前或之后)。

**保持每个数据库的触发器名唯一** 在MySQL 5中,触发器名必
须在每个表中唯一,但不是在每个数据库中唯一。这表示同一
数据库中的两个表可具有相同名字的触发器。这在其他每个数
据库触发器名必须唯一的DBMS中是不允许的,而且以后的
MySQL版本很可能会使命名规则更为严格。因此,现在最好
是在数据库范围内使用唯一的触发器名。

触发器用 CREATE TRIGGER 语句创建。下面是一个简单的例子:

>>

```shell
MariaDB [test]> create trigger triggervendors after insert on vendors for each row select 'hello' into @args;
Query OK, 0 rows affected (0.35 sec)

MariaDB [test]> insert into vendors values (2007,'xx',null,null,null,null,null);
Query OK, 1 row affected (0.05 sec)

MariaDB [test]> select @args;
+-------+
| @args |
+-------+
| hello |
+-------+
1 row in set (0.00 sec)
```

`CREATE TRIGGER` 用来创建名为 `triggervendors `的新触发器。触发器
可在一个操作发生之前或之后执行,这里给出了 `AFTER INSERT` ,
所以此触发器将在 `INSERT` 语句成功执行后执行。这个触发器还指定
`FOR EACH ROW` ,因此代码对每个插入行执行。
在这个例子中,文本 `hello 将对每个插入的行显示一次。
为了测试这个触发器,使用 `INSERT` 语句添加一行或多行到 `vendors`
中,你将看到对每个成功的插入,显示 `hello` 消息。

**仅支持表** 只有表才支持触发器,视图不支持(临时表也不支持)。

触发器按每个表每个事件每次地定义,每个表每个事件每次只允许
一个触发器。因此,每个表最多支持6个触发器(每条 INSERT 、 UPDATE
和 DELETE 的之前和之后)
。单一触发器不能与多个事件或多个表关联,所
以,如果你需要一个对 INSERT 和 UPDATE 操作执行的触发器,则应该定义
两个触发器。

触发器失败 如果 BEFORE 触发器失败,则MySQL将不执行请
求的操作。此外,如果 BEFORE 触发器或语句本身失败, MySQL
将不执行 AFTER 触发器(如果有的话)
。
> 删除触发器
现在,删除触发器的语法应该很明显了。为了删除一个触发器,可
使用 DROP TRIGGER 语句,如下所示:

```shell
MariaDB [test]> drop trigger triggervendors;
Query OK, 0 rows affected (0.00 sec)
```

触发器不能更新或覆盖。为了修改一个触发器,必须先删除它,
然后再重新创建。

> 使用触发器

在有了前面的基础知识后,我们现在来看所支持的每种触发器类型
以及它们的差别。

1.INSERT 触发器

INSERT 触发器在 INSERT 语句执行之前或之后执行。需要知道以下几点:

* 在 INSERT 触发器代码内,可引用一个名为 NEW 的虚拟表,访问被
插入的行;
* 在 BEFORE INSERT 触发器中, NEW 中的值也可以被更新(允许更改
被插入的值);
* 对于 AUTO_INCREMENT 列, NEW 在 INSERT 执行之前包含 0 ,在 INSERT
执行之后包含新的自动生成值。

>> 下面举一个例子(一个实际有用的例子)。 AUTO_INCREMENT 列具有
MySQL自动赋予的值。之前建议了几种确定新生成值的方法,但下面
是一种更好的方法:

```shell
MariaDB [test]> create trigger neworder after insert on orders for each row select new.order_num into @hh;
Query OK, 0 rows affected (0.37 sec)
```

此代码创建一个名为 `neworder` 的触发器,它按照 `AFTER INSERT
ON orders` 执行。在插入一个新订单到` orders `表时,MySQL生
成一个新订单号并保存到 `order_num` 中。触发器从 `NEW. order_num` 取得
这个值并返回它。此触发器必须按照 `AFTER INSERT` 执行,因为在 `BEFORE
INSERT` 语句执行之前,新 `order_num` 还没有生成。对于 `orders` 的每次插
入使用这个触发器将总是返回新的订单号。

>> 为测试这个触发器,试着插入一下新行,如下所示:

```shell
MariaDB [test]> insert into orders (order_date,cust_id) values (now(),10001);
Query OK, 1 row affected (0.60 sec)

MariaDB [test]> select @hh;
+-------+
| @hh   |
+-------+
| 20010 |
+-------+
1 row in set (0.00 sec)

MariaDB [test]> select * from orders;
+-----------+---------------------+---------+
| order_num | order_date          | cust_id |
+-----------+---------------------+---------+
|     20005 | 2005-09-01 00:00:00 |   10001 |
|     20006 | 2005-09-12 00:00:00 |   10003 |
|     20007 | 2005-09-30 00:00:00 |   10004 |
|     20008 | 2005-10-03 00:00:00 |   10005 |
|     20009 | 2005-10-08 00:00:00 |   10001 |
|     20010 | 2016-09-21 11:08:47 |   10001 |
+-----------+---------------------+---------+
6 rows in set (0.00 sec)
```
orders 包 含 3 个 列 。 order_date 和 cust_id 必 须 给 出 ,
order_num 由MySQL自动生成,而现在 order_num 还自动被返
回。

**BEFORE 或 AFTER ?** 通常,将 BEFORE 用于数据验证和净化(目
的是保证插入表中的数据确实是需要的数据)。本提示也适用于 UPDATE 触发器。

2.DELETE 触发器

DELETE 触发器在 DELETE 语句执行之前或之后执行。需要知道以下两点:

* 在 DELETE 触发器代码内,你可以引用一个名为 OLD 的虚拟表,访
问被删除的行;
* OLD 中的值全都是只读的,不能更新。

>> 下面的例子演示使用 OLD 保存将要被删除的行到一个存档表中:

```shell
MariaDB [test]> delimiter //

MariaDB [test]> create trigger deleteorder before delete on orders for each row  begin insert into archive_orders(order_num,order_date,cust_id) values (OLD.order_num,OLD.order_date,OLD.cust_id); end//
Query OK, 0 rows affected (0.07 sec)

MariaDB [test]> delimiter ;

```
在任意订单被删除前将执行此触发器。它使用一条 `INSERT `语句
将 `OLD` 中的值(要被删除的订单)保存到一个名为 `archive_orders` 的
存档表中(为实际使用这个例子,你需要用与 `orders` 相同的列
创建一个名为 `archive_orders` 的表)。

使用 `BEFORE DELETE` 触发器的优点(相对于 `AFTER DELETE` 触发器
来说)为,如果由于某种原因,订单不能存档, `DELETE` 本身将被放弃。

**多语句触发器** 正如所见,触发器 `deleteorder` 使用 `BEGIN` 和
`END` 语句标记触发器体。这在此例子中并不是必需的,不过也
没有害处。使用 `BEGIN END` 块的好处是触发器能容纳多条SQL
语句(在 `BEGIN END`块 中一条挨着一条)。


3.UPDATE 触发器

UPDATE 触发器在 UPDATE 语句执行之前或之后执行。需要知道以下几点:

* 在 UPDATE 触发器代码中,你可以引用一个名为 OLD 的虚拟表访问
以前( UPDATE 语句前)的值,引用一个名为 NEW 的虚拟表访问新
更新的值;
* 在 BEFORE UPDATE 触发器中, NEW 中的值可能也被更新(允许更改
将要用于 UPDATE 语句中的值);
* OLD 中的值全都是只读的,不能更新。

下面的例子保证州名缩写总是大写(不管 UPDATE 语句中给出的是大写还是小写):

显然,任何数据净化都需要在 UPDATE 语句之前进行,就像这
个例子中一样。

>> 每次更新一个行时, NEW.vend_state 中的
值(将用来更新表行的值)都用 Upper(NEW.vend_state) 替换。

```shell
MariaDB [test]> create trigger updatevendor before update on vendors for each row set New.vend_state = Upper（new.vend_state）;
Query OK, 0 rows affected (0.35 sec)
```

4.关于触发器的进一步介绍

在结束本章之前,我们再介绍一些使用触发器时需要记住的重点。
* 与其他DBMS相比,MySQL 5中支持的触发器相当初级。未来的
MySQL版本中有一些改进和增强触发器支持的计划。
* 创建触发器可能需要特殊的安全访问权限,但是,触发器的执行
是自动的。如果 INSERT 、 UPDATE 或 DELETE 语句能够执行,则相关
的触发器也能执行。
* 应该用触发器来保证数据的一致性(大小写、格式等)。在触发器
中执行这种类型的处理的优点是它总是进行这种处理,而且是透
明地进行,与客户机应用无关。
* 触发器的一种非常有意义的使用是创建审计跟踪。使用触发器,
把更改(如果需要,甚至还有之前和之后的状态)记录到另一个
表非常容易。
* 遗憾的是,MySQL触发器中不支持 CALL 语句。这表示不能从触发
器内调用存储过程。所需的存储过程代码需要复制到触发器内。

> 小结
>> 本章介绍了什么是触发器以及为什么要使用触发器,学习了触发器
的类型和何时执行它们,列举了几个用于 INSERT 、 DELETE 和 UPDATE 操作
的触发器例子。

---

### 管理事务处理

本章介绍什么是事务处理以及如何利用 COMMIT 和 ROLLBACK 语句来管
理事务处理。

> 事务处理

**并非所有引擎都支持事务处理** MySQL支
持几种基本的数据库引擎，并非所有引擎都
支持明确的事务处理管理。 MyISAM 和 InnoDB 是两种最常使用
的引擎。前者不支持明确的事务处理管理,而后者支持。这
就是为什么本书中使用的样例表被创建来使用 InnoDB 而不是
更经常使用的 MyISAM 的原因。如果你的应用中需要事务处理
功能,则一定要使用正确的引擎类型。

事务处理(transaction processing)可以用来维护数据库的完整性,它
保证成批的MySQL操作要么完全执行,要么完全不执行。

关系数据库设计把数据存储在多个表中,使数据
更容易操纵、维护和重用。不用深究如何以及为什么进行关系数据库设
计,在某种程度上说,设计良好的数据库模式都是关联的。

前面章中使用的 `orders` 表就是一个很好的例子。订单存储在 `orders`
和 `orderitems `两个表中: `orders` 存储实际的订单,而 `orderitems` 存储订
购的各项物品。这两个表使用称为主键的唯一ID互相关联。
这两个表又与包含客户和产品信息的其他表相关联。

给系统添加订单的过程如下。

(1) 检查数据库中是否存在相应的客户(从 customers 表查询),如果
不存在,添加他/她。
(2) 检索客户的ID。
(3) 添加一行到 orders 表,把它与客户ID关联。
(4) 检索 orders 表中赋予的新订单ID。
(5) 对于订购的每个物品在 orderitems 表中添加一行,通过检索
出来的ID把它与 orders 表关联(以及通过产品ID与 products 表关联)。

现在,假如由于某种数据库故障(如超出磁盘空间、安全限制、表
锁等)阻止了这个过程的完成。数据库中的数据会出现什么情况?

如果故障发生在添加了客户之后, orders 表添加之前,不会有什么
问题。某些客户没有订单是完全合法的。在重新执行此过程时,所插入
的客户记录将被检索和使用。可以有效地从出故障的地方开始执行此过
程。

但是,如果故障发生在 orders 行添加之后, orderitems 行添加之前,
怎么办呢?现在,数据库中有一个空订单。

更糟的是,如果系统在添加 orderitems 行之中出现故障。结果是数
据库中存在不完整的订单,而且你还不知道。

如何解决这种问题?这里就需要使用事务处理了。事务处理是一种
机制,用来管理必须成批执行的MySQL操作,以保证数据库不包含不完
整的操作结果。利用事务处理,可以保证一组操作不会中途停止,它们
或者作为整体执行,或者完全不执行(除非明确指示)
。如果没有错误发生,整组语句提交给(写到)数据库表。如果发生错误,则进行回退(撤
销)以恢复数据库到某个已知且安全的状态。

因此,请看相同的例子,这次我们说明过程如何工作。
- 检查数据库中是否存在相应的客户,如果不存在,添加他/她。
- 提交客户信息。
- 检索客户的ID。
- 添加一行到 orders 表。
- 如果在添加行到 orders 表时出现故障,回退。
- 检索 orders 表中赋予的新订单ID。
- 对于订购的每项物品,添加新行到 orderitems 表。
- 如果在添加新行到 orderitems 时出现故障,回退所有添加的
orderitems 行和 orders 行。
- 提交订单信息。

在使用事务和事务处理时,有几个关键词汇反复出现。下面是关于
事务处理需要知道的几个术语:

* 事务( transaction )指一组SQL语句;
* 回退( rollback )指撤销指定SQL语句的过程;
* 提交( commit )指将未存储的SQL语句结果写入数据库表;
* 保留点( savepoint )指事务处理中设置的临时占位符(place-
holder),你可以对它发布回退(与回退整个事务处理不同)。


> 控制事务处理

既然我们已经知道了什么是事务处理,下面讨论事务处理的管理中
所涉及的问题。

管理事务处理的关键在于将SQL语句组分解为逻辑块,并明确规定数
据何时应该回退,何时不应该回退。

MySQL使用下面的语句来标识事务的开始:`start transaction;`


1.使用 ROLLBACK

MySQL的 ROLLBACK 命令用来回退(撤销)MySQL语句,请看下面的
语句:

```shell
MariaDB [test]> select * from ordertotals;
+-----------+---------+
| order_num | total   |
+-----------+---------+
|     20005 |  158.86 |
|     20009 |   40.78 |
|     20006 |   58.30 |
|     20007 | 1060.00 |
|     20008 |  132.50 |
|     20008 |  132.50 |
+-----------+---------+
6 rows in set (0.00 sec)

MariaDB [test]> start transaction;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delete from ordertotals;
Query OK, 6 rows affected (0.00 sec)

MariaDB [test]> select * from ordertotals;
Empty set (0.00 sec)

MariaDB [test]> rollback;
Query OK, 0 rows affected (0.55 sec)

MariaDB [test]> select * from ordertotals;
+-----------+---------+
| order_num | total   |
+-----------+---------+
|     20005 |  158.86 |
|     20009 |   40.78 |
|     20006 |   58.30 |
|     20007 | 1060.00 |
|     20008 |  132.50 |
|     20008 |  132.50 |
+-----------+---------+
6 rows in set (0.00 sec)
```

这个例子从显示 ordertotals 表的内容开始。首先执行一条 SELECT 以显示该表不为空。
然后开始一个事务处理,用一条 DELETE 语句删除 ordertotals 中的所有行。另一条
SELECT 语句验证 ordertotals 确实为空。这时用一条 ROLLBACK 语句回退
START TRANSACTION 之后的所有语句,最后一条 SELECT 语句显示该表不为空。

显然, ROLLBACK 只能在一个事务处理内使用(在执行一条 START
TRANSACTION 命令之后)。

**哪些语句可以回退?** 事务处理用来管理 INSERT 、 UPDATE 和
DELETE 语句。你不能回退 SELECT 语句。
(这样做也没有什么意义。)你不能回退 CREATE 或 DROP 操作。事务处理块中可以使用
这两条语句,但如果你执行回退,它们不会被撤销。


2.使用 COMMIT

一般的MySQL语句都是直接针对数据库表执行和编写的。这就是
所谓的隐含提交(implicit commit),即提交(写或保存)操作是自动
进行的。

但是,在事务处理块中,提交不会隐含地进行。为进行明确的提交,
使用 COMMIT 语句,如下所示:

```shell
MariaDB [test]> start transaction;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delete from orderitems where order_num=20010;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delete from orders where order_num=20010;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> commit;
Query OK, 0 rows affected (0.00 sec)
```
在这个例子中,从系统中完全删除订单 `20010` 。因为涉及更新
两个数据库表 `orders` 和 `orderItems` ,所以使用事务处理块来
保证订单不被部分删除。最后的 COMMIT 语句仅在不出错时写出更改。如
果第一条 DELETE 起作用,但第二条失败,则 DELETE 不会提交(实际上,
它是被自动撤销的)。

**隐含事务关闭** 当 `COMMIT` 或 `ROLLBACK` 语句执行后,事务会自
隐含事务关闭动关闭(将来的更改会隐含提交)。

3.使用保留点

简单的 `ROLLBACK` 和 `COMMIT` 语句就可以写入或撤销整个事务处理。但
是,只是对简单的事务处理才能这样做,更复杂的事务处理可能需要部
分提交或回退。

例如,前面描述的添加订单的过程为一个事务处理。如果发生错误,
只需要返回到添加 orders 行之前即可,不需要回退到 customers 表(如果存在的话)。

为了支持回退部分事务处理,必须能在事务处理块中合适的位置放
置占位符。这样,如果需要回退,可以回退到某个占位符。

这些占位符称为保留点。为了创建占位符,可如下使用 SAVEPOINT
语句:`savepoint delete1;`

每个保留点都取标识它的唯一名字,以便在回退时,MySQL知道要
回退到何处。为了回退到本例给出的保留点,可如下进行:
`rollback to delete1;`

**保留点越多越好** 可以在MySQL代码中设置任意多的保留
点,越多越好。为什么呢?因为保留点越多,你就越能按自己
的意愿灵活地进行回退。

**释放保留点** 保留点在事务处理完成(执行一条 ROLLBACK 或
COMMIT )后自动释放。自MySQL 5以来,也可以用 RELEASE
SAVEPOINT 明确地释放保留点。

4.更改默认的提交行为

正如所述,默认的MySQL行为是自动提交所有更改。换句话说,任何
时候你执行一条MySQL语句,该语句实际上都是针对表执行的,而且所做
的更改立即生效。为指示MySQL不自动提交更改,需要使用以下语句:
`set autocommit=0;`

`autocommit` 标志决定是否自动提交更改,不管有没有 `COMMIT`
语句。设置 `autocommit` 为` 0` (假)指示MySQL不自动提交更改
(直到 `autocommit` 被设置为 `1` 真为止)。

**标志为连接专用** `autocommit` 标志是针对每个连接而不是服
务器的。

>> 查看autocommit的值

```shell
MariaDB [test]> show variables like '%autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    |
+---------------+-------+
1 row in set (0.00 sec)

MariaDB [test]> select @@autocommit;
+--------------+
| @@autocommit |
+--------------+
|            1 |
+--------------+
1 row in set (0.00 sec)
```


>> 课堂练习

```shell
MariaDB [test]> create database db1;
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> create table db1.t1 (id int primary key );
Query OK, 0 rows affected (0.06 sec)

MariaDB [test]> insert into db1.t1 values (1);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
+----+
| id |
+----+
|  1 |
+----+
1 row in set (0.00 sec)

MariaDB [test]> start transaction;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> delete from db1.t1;
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> savepoint delete1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> insert into db1.t1 values (10);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> savepoint insert1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
+----+
| id |
+----+
| 10 |
+----+
1 row in set (0.00 sec)

MariaDB [test]> rollback to delete1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
Empty set (0.00 sec)

MariaDB [test]> rollback to insert1;
ERROR 1305 (42000): SAVEPOINT insert1 does not exist
MariaDB [test]> rollback to insert1;
ERROR 1305 (42000): SAVEPOINT insert1 does not exist
MariaDB [test]> insert into db1.t1 values (20);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
+----+
| id |
+----+
| 20 |
+----+
1 row in set (0.00 sec)

MariaDB [test]> savepoint in1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> insert into db1.t1 values (30);
Query OK, 1 row affected (0.00 sec)

MariaDB [test]> savepoint in2;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
+----+
| id |
+----+
| 20 |
| 30 |
+----+
2 rows in set (0.00 sec)

MariaDB [test]> rollback to in1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
+----+
| id |
+----+
| 20 |
+----+
1 row in set (0.00 sec)

MariaDB [test]> rollback to in2;
ERROR 1305 (42000): SAVEPOINT in2 does not exist
MariaDB [test]> rollback to delete1;
Query OK, 0 rows affected (0.00 sec)

MariaDB [test]> select * from db1.t1;
Empty set (0.00 sec)
```

注意，当退回到某个保留点时，该保留点之后的保留点就会消失。

> 小结
>> 本章介绍了事务处理是必须完整执行的SQL语句块。我们学习了如何
使用 COMMIT 和 ROLLBACK 语句对何时写数据,何时撤销进行明确的管理。
还学习了如何使用保留点对回退操作提供更强大的控制。

---

### 全球化和本地化

本章介绍MySQL处理不同字符集和语言的基础知识。

> 字符集和校对顺序

数据库表被用来存储和检索数据。不同的语言和字符集需要以不同
的方式存储和检索。因此,MySQL需要适应不同的字符集(不同的字母
和字符),适应不同的排序和检索数据的方法。

在讨论多种语言和字符集时,将会遇到以下重要术语:

* 字符集为字母和符号的集合;
* 编码为某个字符集成员的内部表示;
* 校对为规定字符如何比较的指令。

**校对为什么重要** 排序英文正文很容易,对吗?或许不。考
虑词APE、apex和Apple。它们处于正确的排序顺序吗?这有
赖于你是否想区分大小写。使用区分大小写的校对顺序,这
些词有一种排序方式,使用不区分大小写的校对顺序有另外
一种排序方式。这不仅影响排序(如用 ORDER BY 排序数据),
还影响搜索(例如,寻找apple的WHERE子句是否能找到APPLE)。
在使用诸如法文à或德文ö这样的字符时,情况更复
杂,在使用不基于拉丁文的字符集(日文、希伯来文、俄文等)时,情况更为复杂。

在MySQL的正常数据库活动( SELECT 、 INSERT 等)中,不需要操心太
多的东西。使用何种字符集和校对的决定在服务器、数据库和表级进行。

> 使用字符集和校对顺序

>> MySQL支持众多的字符集。为查看所支持的字符集完整列表,使用
以下语句:

```shell
MariaDB [test]> show character set;
+----------+-----------------------------+---------------------+--------+
| Charset  | Description                 | Default collation   | Maxlen |
+----------+-----------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese    | big5_chinese_ci     |      2 |
| dec8     | DEC West European           | dec8_swedish_ci     |      1 |
| cp850    | DOS West European           | cp850_general_ci    |      1 |
| hp8      | HP West European            | hp8_english_ci      |      1 |
| koi8r    | KOI8-R Relcom Russian       | koi8r_general_ci    |      1 |
| latin1   | cp1252 West European        | latin1_swedish_ci   |      1 |
| latin2   | ISO 8859-2 Central European | latin2_general_ci   |      1 |
| swe7     | 7bit Swedish                | swe7_swedish_ci     |      1 |
| ascii    | US ASCII                    | ascii_general_ci    |      1 |
| ujis     | EUC-JP Japanese             | ujis_japanese_ci    |      3 |
| sjis     | Shift-JIS Japanese          | sjis_japanese_ci    |      2 |
| hebrew   | ISO 8859-8 Hebrew           | hebrew_general_ci   |      1 |
| tis620   | TIS620 Thai                 | tis620_thai_ci      |      1 |
| euckr    | EUC-KR Korean               | euckr_korean_ci     |      2 |
| koi8u    | KOI8-U Ukrainian            | koi8u_general_ci    |      1 |
| gb2312   | GB2312 Simplified Chinese   | gb2312_chinese_ci   |      2 |
| greek    | ISO 8859-7 Greek            | greek_general_ci    |      1 |
| cp1250   | Windows Central European    | cp1250_general_ci   |      1 |
| gbk      | GBK Simplified Chinese      | gbk_chinese_ci      |      2 |
| latin5   | ISO 8859-9 Turkish          | latin5_turkish_ci   |      1 |
| armscii8 | ARMSCII-8 Armenian          | armscii8_general_ci |      1 |
| utf8     | UTF-8 Unicode               | utf8_general_ci     |      3 |
| ucs2     | UCS-2 Unicode               | ucs2_general_ci     |      2 |
| cp866    | DOS Russian                 | cp866_general_ci    |      1 |
| keybcs2  | DOS Kamenicky Czech-Slovak  | keybcs2_general_ci  |      1 |
| macce    | Mac Central European        | macce_general_ci    |      1 |
| macroman | Mac West European           | macroman_general_ci |      1 |
| cp852    | DOS Central European        | cp852_general_ci    |      1 |
| latin7   | ISO 8859-13 Baltic          | latin7_general_ci   |      1 |
| utf8mb4  | UTF-8 Unicode               | utf8mb4_general_ci  |      4 |
| cp1251   | Windows Cyrillic            | cp1251_general_ci   |      1 |
| utf16    | UTF-16 Unicode              | utf16_general_ci    |      4 |
| cp1256   | Windows Arabic              | cp1256_general_ci   |      1 |
| cp1257   | Windows Baltic              | cp1257_general_ci   |      1 |
| utf32    | UTF-32 Unicode              | utf32_general_ci    |      4 |
| binary   | Binary pseudo charset       | binary              |      1 |
| geostd8  | GEOSTD8 Georgian            | geostd8_general_ci  |      1 |
| cp932    | SJIS for Windows Japanese   | cp932_japanese_ci   |      2 |
| eucjpms  | UJIS for Windows Japanese   | eucjpms_japanese_ci |      3 |
+----------+-----------------------------+---------------------+--------+
39 rows in set (0.00 sec)
```
这条语句显示所有可用的字符集以及每个字符集的描述和默认
校对。

>> 为了查看所支持校对的完整列表,使用以下语句:

```shell
MariaDB [test]> show collation;
+--------------------------+----------+-----+---------+----------+---------+
| Collation                | Charset  | Id  | Default | Compiled | Sortlen |
+--------------------------+----------+-----+---------+----------+---------+
| big5_chinese_ci          | big5     |   1 | Yes     | Yes      |       1 |
| big5_bin                 | big5     |  84 |         | Yes      |       1 |
| dec8_swedish_ci          | dec8     |   3 | Yes     | Yes      |       1 |
| dec8_bin                 | dec8     |  69 |         | Yes      |       1 |
| cp850_general_ci         | cp850    |   4 | Yes     | Yes      |       1 |
| cp850_bin                | cp850    |  80 |         | Yes      |       1 |
| hp8_english_ci           | hp8      |   6 | Yes     | Yes      |       1 |
| hp8_bin                  | hp8      |  72 |         | Yes      |       1 |
| koi8r_general_ci         | koi8r    |   7 | Yes     | Yes      |       1 |
| koi8r_bin                | koi8r    |  74 |         | Yes      |       1 |
| latin1_german1_ci        | latin1   |   5 |         | Yes      |       1 |
| latin1_swedish_ci        | latin1   |   8 | Yes     | Yes      |       1 |
| latin1_danish_ci         | latin1   |  15 |         | Yes      |       1 |
| latin1_german2_ci        | latin1   |  31 |         | Yes      |       2 |
| latin1_bin               | latin1   |  47 |         | Yes      |       1 |
| latin1_general_ci        | latin1   |  48 |         | Yes      |       1 |
| latin1_general_cs        | latin1   |  49 |         | Yes      |       1 |
| latin1_spanish_ci        | latin1   |  94 |         | Yes      |       1 |
| latin2_czech_cs          | latin2   |   2 |         | Yes      |       4 |
| latin2_general_ci        | latin2   |   9 | Yes     | Yes      |       1 |
| latin2_hungarian_ci      | latin2   |  21 |         | Yes      |       1 |
| latin2_croatian_ci       | latin2   |  27 |         | Yes      |       1 |
| latin2_bin               | latin2   |  77 |         | Yes      |       1 |
| swe7_swedish_ci          | swe7     |  10 | Yes     | Yes      |       1 |
| swe7_bin                 | swe7     |  82 |         | Yes      |       1 |
| ascii_general_ci         | ascii    |  11 | Yes     | Yes      |       1 |
| ascii_bin                | ascii    |  65 |         | Yes      |       1 |
| ujis_japanese_ci         | ujis     |  12 | Yes     | Yes      |       1 |
| ujis_bin                 | ujis     |  91 |         | Yes      |       1 |
| sjis_japanese_ci         | sjis     |  13 | Yes     | Yes      |       1 |
| sjis_bin                 | sjis     |  88 |         | Yes      |       1 |
| hebrew_general_ci        | hebrew   |  16 | Yes     | Yes      |       1 |
| hebrew_bin               | hebrew   |  71 |         | Yes      |       1 |
| tis620_thai_ci           | tis620   |  18 | Yes     | Yes      |       4 |
| tis620_bin               | tis620   |  89 |         | Yes      |       1 |
| euckr_korean_ci          | euckr    |  19 | Yes     | Yes      |       1 |
| euckr_bin                | euckr    |  85 |         | Yes      |       1 |
| koi8u_general_ci         | koi8u    |  22 | Yes     | Yes      |       1 |
| koi8u_bin                | koi8u    |  75 |         | Yes      |       1 |
| gb2312_chinese_ci        | gb2312   |  24 | Yes     | Yes      |       1 |
| gb2312_bin               | gb2312   |  86 |         | Yes      |       1 |
| greek_general_ci         | greek    |  25 | Yes     | Yes      |       1 |
| greek_bin                | greek    |  70 |         | Yes      |       1 |
| cp1250_general_ci        | cp1250   |  26 | Yes     | Yes      |       1 |
| cp1250_czech_cs          | cp1250   |  34 |         | Yes      |       2 |
| cp1250_croatian_ci       | cp1250   |  44 |         | Yes      |       1 |
| cp1250_bin               | cp1250   |  66 |         | Yes      |       1 |
| cp1250_polish_ci         | cp1250   |  99 |         | Yes      |       1 |
| gbk_chinese_ci           | gbk      |  28 | Yes     | Yes      |       1 |
| gbk_bin                  | gbk      |  87 |         | Yes      |       1 |
| latin5_turkish_ci        | latin5   |  30 | Yes     | Yes      |       1 |
| latin5_bin               | latin5   |  78 |         | Yes      |       1 |
| armscii8_general_ci      | armscii8 |  32 | Yes     | Yes      |       1 |
| armscii8_bin             | armscii8 |  64 |         | Yes      |       1 |
| utf8_general_ci          | utf8     |  33 | Yes     | Yes      |       1 |
| utf8_bin                 | utf8     |  83 |         | Yes      |       1 |
| utf8_unicode_ci          | utf8     | 192 |         | Yes      |       8 |
| utf8_icelandic_ci        | utf8     | 193 |         | Yes      |       8 |
| utf8_latvian_ci          | utf8     | 194 |         | Yes      |       8 |
| utf8_romanian_ci         | utf8     | 195 |         | Yes      |       8 |
| utf8_slovenian_ci        | utf8     | 196 |         | Yes      |       8 |
| utf8_polish_ci           | utf8     | 197 |         | Yes      |       8 |
| utf8_estonian_ci         | utf8     | 198 |         | Yes      |       8 |
| utf8_spanish_ci          | utf8     | 199 |         | Yes      |       8 |
| utf8_swedish_ci          | utf8     | 200 |         | Yes      |       8 |
| utf8_turkish_ci          | utf8     | 201 |         | Yes      |       8 |
| utf8_czech_ci            | utf8     | 202 |         | Yes      |       8 |
| utf8_danish_ci           | utf8     | 203 |         | Yes      |       8 |
| utf8_lithuanian_ci       | utf8     | 204 |         | Yes      |       8 |
| utf8_slovak_ci           | utf8     | 205 |         | Yes      |       8 |
| utf8_spanish2_ci         | utf8     | 206 |         | Yes      |       8 |
| utf8_roman_ci            | utf8     | 207 |         | Yes      |       8 |
| utf8_persian_ci          | utf8     | 208 |         | Yes      |       8 |
| utf8_esperanto_ci        | utf8     | 209 |         | Yes      |       8 |
| utf8_hungarian_ci        | utf8     | 210 |         | Yes      |       8 |
| utf8_sinhala_ci          | utf8     | 211 |         | Yes      |       8 |
| utf8_croatian_ci         | utf8     | 213 |         | Yes      |       8 |
| utf8_general_mysql500_ci | utf8     | 223 |         | Yes      |       1 |
| ucs2_general_ci          | ucs2     |  35 | Yes     | Yes      |       1 |
| ucs2_bin                 | ucs2     |  90 |         | Yes      |       1 |
| ucs2_unicode_ci          | ucs2     | 128 |         | Yes      |       8 |
| ucs2_icelandic_ci        | ucs2     | 129 |         | Yes      |       8 |
| ucs2_latvian_ci          | ucs2     | 130 |         | Yes      |       8 |
| ucs2_romanian_ci         | ucs2     | 131 |         | Yes      |       8 |
| ucs2_slovenian_ci        | ucs2     | 132 |         | Yes      |       8 |
| ucs2_polish_ci           | ucs2     | 133 |         | Yes      |       8 |
| ucs2_estonian_ci         | ucs2     | 134 |         | Yes      |       8 |
| ucs2_spanish_ci          | ucs2     | 135 |         | Yes      |       8 |
| ucs2_swedish_ci          | ucs2     | 136 |         | Yes      |       8 |
| ucs2_turkish_ci          | ucs2     | 137 |         | Yes      |       8 |
| ucs2_czech_ci            | ucs2     | 138 |         | Yes      |       8 |
| ucs2_danish_ci           | ucs2     | 139 |         | Yes      |       8 |
| ucs2_lithuanian_ci       | ucs2     | 140 |         | Yes      |       8 |
| ucs2_slovak_ci           | ucs2     | 141 |         | Yes      |       8 |
| ucs2_spanish2_ci         | ucs2     | 142 |         | Yes      |       8 |
| ucs2_roman_ci            | ucs2     | 143 |         | Yes      |       8 |
| ucs2_persian_ci          | ucs2     | 144 |         | Yes      |       8 |
| ucs2_esperanto_ci        | ucs2     | 145 |         | Yes      |       8 |
| ucs2_hungarian_ci        | ucs2     | 146 |         | Yes      |       8 |
| ucs2_sinhala_ci          | ucs2     | 147 |         | Yes      |       8 |
| ucs2_croatian_ci         | ucs2     | 149 |         | Yes      |       8 |
| ucs2_general_mysql500_ci | ucs2     | 159 |         | Yes      |       1 |
| cp866_general_ci         | cp866    |  36 | Yes     | Yes      |       1 |
| cp866_bin                | cp866    |  68 |         | Yes      |       1 |
| keybcs2_general_ci       | keybcs2  |  37 | Yes     | Yes      |       1 |
| keybcs2_bin              | keybcs2  |  73 |         | Yes      |       1 |
| macce_general_ci         | macce    |  38 | Yes     | Yes      |       1 |
| macce_bin                | macce    |  43 |         | Yes      |       1 |
| macroman_general_ci      | macroman |  39 | Yes     | Yes      |       1 |
| macroman_bin             | macroman |  53 |         | Yes      |       1 |
| cp852_general_ci         | cp852    |  40 | Yes     | Yes      |       1 |
| cp852_bin                | cp852    |  81 |         | Yes      |       1 |
| latin7_estonian_cs       | latin7   |  20 |         | Yes      |       1 |
| latin7_general_ci        | latin7   |  41 | Yes     | Yes      |       1 |
| latin7_general_cs        | latin7   |  42 |         | Yes      |       1 |
| latin7_bin               | latin7   |  79 |         | Yes      |       1 |
| utf8mb4_general_ci       | utf8mb4  |  45 | Yes     | Yes      |       1 |
| utf8mb4_bin              | utf8mb4  |  46 |         | Yes      |       1 |
| utf8mb4_unicode_ci       | utf8mb4  | 224 |         | Yes      |       8 |
| utf8mb4_icelandic_ci     | utf8mb4  | 225 |         | Yes      |       8 |
| utf8mb4_latvian_ci       | utf8mb4  | 226 |         | Yes      |       8 |
| utf8mb4_romanian_ci      | utf8mb4  | 227 |         | Yes      |       8 |
| utf8mb4_slovenian_ci     | utf8mb4  | 228 |         | Yes      |       8 |
| utf8mb4_polish_ci        | utf8mb4  | 229 |         | Yes      |       8 |
| utf8mb4_estonian_ci      | utf8mb4  | 230 |         | Yes      |       8 |
| utf8mb4_spanish_ci       | utf8mb4  | 231 |         | Yes      |       8 |
| utf8mb4_swedish_ci       | utf8mb4  | 232 |         | Yes      |       8 |
| utf8mb4_turkish_ci       | utf8mb4  | 233 |         | Yes      |       8 |
| utf8mb4_czech_ci         | utf8mb4  | 234 |         | Yes      |       8 |
| utf8mb4_danish_ci        | utf8mb4  | 235 |         | Yes      |       8 |
| utf8mb4_lithuanian_ci    | utf8mb4  | 236 |         | Yes      |       8 |
| utf8mb4_slovak_ci        | utf8mb4  | 237 |         | Yes      |       8 |
| utf8mb4_spanish2_ci      | utf8mb4  | 238 |         | Yes      |       8 |
| utf8mb4_roman_ci         | utf8mb4  | 239 |         | Yes      |       8 |
| utf8mb4_persian_ci       | utf8mb4  | 240 |         | Yes      |       8 |
| utf8mb4_esperanto_ci     | utf8mb4  | 241 |         | Yes      |       8 |
| utf8mb4_hungarian_ci     | utf8mb4  | 242 |         | Yes      |       8 |
| utf8mb4_sinhala_ci       | utf8mb4  | 243 |         | Yes      |       8 |
| utf8mb4_croatian_ci      | utf8mb4  | 245 |         | Yes      |       8 |
| cp1251_bulgarian_ci      | cp1251   |  14 |         | Yes      |       1 |
| cp1251_ukrainian_ci      | cp1251   |  23 |         | Yes      |       1 |
| cp1251_bin               | cp1251   |  50 |         | Yes      |       1 |
| cp1251_general_ci        | cp1251   |  51 | Yes     | Yes      |       1 |
| cp1251_general_cs        | cp1251   |  52 |         | Yes      |       1 |
| utf16_general_ci         | utf16    |  54 | Yes     | Yes      |       1 |
| utf16_bin                | utf16    |  55 |         | Yes      |       1 |
| utf16_unicode_ci         | utf16    | 101 |         | Yes      |       8 |
| utf16_icelandic_ci       | utf16    | 102 |         | Yes      |       8 |
| utf16_latvian_ci         | utf16    | 103 |         | Yes      |       8 |
| utf16_romanian_ci        | utf16    | 104 |         | Yes      |       8 |
| utf16_slovenian_ci       | utf16    | 105 |         | Yes      |       8 |
| utf16_polish_ci          | utf16    | 106 |         | Yes      |       8 |
| utf16_estonian_ci        | utf16    | 107 |         | Yes      |       8 |
| utf16_spanish_ci         | utf16    | 108 |         | Yes      |       8 |
| utf16_swedish_ci         | utf16    | 109 |         | Yes      |       8 |
| utf16_turkish_ci         | utf16    | 110 |         | Yes      |       8 |
| utf16_czech_ci           | utf16    | 111 |         | Yes      |       8 |
| utf16_danish_ci          | utf16    | 112 |         | Yes      |       8 |
| utf16_lithuanian_ci      | utf16    | 113 |         | Yes      |       8 |
| utf16_slovak_ci          | utf16    | 114 |         | Yes      |       8 |
| utf16_spanish2_ci        | utf16    | 115 |         | Yes      |       8 |
| utf16_roman_ci           | utf16    | 116 |         | Yes      |       8 |
| utf16_persian_ci         | utf16    | 117 |         | Yes      |       8 |
| utf16_esperanto_ci       | utf16    | 118 |         | Yes      |       8 |
| utf16_hungarian_ci       | utf16    | 119 |         | Yes      |       8 |
| utf16_sinhala_ci         | utf16    | 120 |         | Yes      |       8 |
| utf16_croatian_ci        | utf16    | 215 |         | Yes      |       8 |
| cp1256_general_ci        | cp1256   |  57 | Yes     | Yes      |       1 |
| cp1256_bin               | cp1256   |  67 |         | Yes      |       1 |
| cp1257_lithuanian_ci     | cp1257   |  29 |         | Yes      |       1 |
| cp1257_bin               | cp1257   |  58 |         | Yes      |       1 |
| cp1257_general_ci        | cp1257   |  59 | Yes     | Yes      |       1 |
| utf32_general_ci         | utf32    |  60 | Yes     | Yes      |       1 |
| utf32_bin                | utf32    |  61 |         | Yes      |       1 |
| utf32_unicode_ci         | utf32    | 160 |         | Yes      |       8 |
| utf32_icelandic_ci       | utf32    | 161 |         | Yes      |       8 |
| utf32_latvian_ci         | utf32    | 162 |         | Yes      |       8 |
| utf32_romanian_ci        | utf32    | 163 |         | Yes      |       8 |
| utf32_slovenian_ci       | utf32    | 164 |         | Yes      |       8 |
| utf32_polish_ci          | utf32    | 165 |         | Yes      |       8 |
| utf32_estonian_ci        | utf32    | 166 |         | Yes      |       8 |
| utf32_spanish_ci         | utf32    | 167 |         | Yes      |       8 |
| utf32_swedish_ci         | utf32    | 168 |         | Yes      |       8 |
| utf32_turkish_ci         | utf32    | 169 |         | Yes      |       8 |
| utf32_czech_ci           | utf32    | 170 |         | Yes      |       8 |
| utf32_danish_ci          | utf32    | 171 |         | Yes      |       8 |
| utf32_lithuanian_ci      | utf32    | 172 |         | Yes      |       8 |
| utf32_slovak_ci          | utf32    | 173 |         | Yes      |       8 |
| utf32_spanish2_ci        | utf32    | 174 |         | Yes      |       8 |
| utf32_roman_ci           | utf32    | 175 |         | Yes      |       8 |
| utf32_persian_ci         | utf32    | 176 |         | Yes      |       8 |
| utf32_esperanto_ci       | utf32    | 177 |         | Yes      |       8 |
| utf32_hungarian_ci       | utf32    | 178 |         | Yes      |       8 |
| utf32_sinhala_ci         | utf32    | 179 |         | Yes      |       8 |
| utf32_croatian_ci        | utf32    | 214 |         | Yes      |       8 |
| binary                   | binary   |  63 | Yes     | Yes      |       1 |
| geostd8_general_ci       | geostd8  |  92 | Yes     | Yes      |       1 |
| geostd8_bin              | geostd8  |  93 |         | Yes      |       1 |
| cp932_japanese_ci        | cp932    |  95 | Yes     | Yes      |       1 |
| cp932_bin                | cp932    |  96 |         | Yes      |       1 |
| eucjpms_japanese_ci      | eucjpms  |  97 | Yes     | Yes      |       1 |
| eucjpms_bin              | eucjpms  |  98 |         | Yes      |       1 |
+--------------------------+----------+-----+---------+----------+---------+
202 rows in set (0.00 sec)

MariaDB [test]> show collation like '%latin1%';
+-------------------+---------+----+---------+----------+---------+
| Collation         | Charset | Id | Default | Compiled | Sortlen |
+-------------------+---------+----+---------+----------+---------+
| latin1_german1_ci | latin1  |  5 |         | Yes      |       1 |
| latin1_swedish_ci | latin1  |  8 | Yes     | Yes      |       1 |
| latin1_danish_ci  | latin1  | 15 |         | Yes      |       1 |
| latin1_german2_ci | latin1  | 31 |         | Yes      |       2 |
| latin1_bin        | latin1  | 47 |         | Yes      |       1 |
| latin1_general_ci | latin1  | 48 |         | Yes      |       1 |
| latin1_general_cs | latin1  | 49 |         | Yes      |       1 |
| latin1_spanish_ci | latin1  | 94 |         | Yes      |       1 |
+-------------------+---------+----+---------+----------+---------+
8 rows in set (0.00 sec)
```

此语句显示所有可用的校对,以及它们适用的字符集。可以看
到有的字符集具有不止一种校对。例如, `latin1` 对不同的欧洲
语言有几种校对,而且许多校对出现两次,一次区分大小写(由 `_cs` 表示),
一次不区分大小写(由 `_ci` 表示)。

通常系统管理在安装时定义一个默认的字符集和校对。此外,也可
以在创建数据库时,指定默认的字符集和校对。

>> 为了确定所用的字符集和校对,可以使用以下语句:

```shel
MariaDB [test]> show variables like 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

MariaDB [test]> show variables like 'collation%';
+----------------------+-------------------+
| Variable_name        | Value             |
+----------------------+-------------------+
| collation_connection | utf8_general_ci   |
| collation_database   | latin1_swedish_ci |
| collation_server     | latin1_swedish_ci |
+----------------------+-------------------+
3 rows in set (0.00 sec)
```

实际上,字符集很少是服务器范围(甚至数据库范围)的设置。
不同的表,甚至不同的列都可能需要不同的字符集,而且两者都
可以在创建表时指定。

>> 为了给表指定字符集和校对,可使用带子句的 CREATE TABLE :

```shell
MariaDB [test]> create table mytable (columnn1 int,colunmn2 varchar(10)) default character set hebrew collate hebrew_general_ci;
Query OK, 0 rows affected (0.49 sec)
```

此语句创建一个包含两列的表,并且指定一个字符集和一个校对顺序。

这个例子中指定了 CHARACTER SET 和 COLLATE 两者。一般,MySQL如
下确定使用什么样的字符集和校对。

* 如果指定 `CHARACTER SET` 和 `COLLATE` 两者,则使用这些值。
* 如果只指定 `CHARACTER SET` ,则使用此字符集及其默认的校对(如
`SHOW CHARACTER SET` 的结果中所示)。
* 如果既不指定 `CHARACTER SET` ,也不指定 `COLLATE` ,则使用数据库默认。

>> 除了能指定字符集和校对的表范围外,MySQL还允许对每个列设置它们,如下所示:

```shell
MariaDB [test]> create table mytable1 (coln1 int,coln2 varchar(10),coln3 varchar(10) character set latin1 collate latin1_general_ci) default character set hebrew collate hebrew_general_ci;
Query OK, 0 rows affected (0.36 sec)

```
这里对整个表以及一个特定的列指定了 `CHARACTER SET` 和 `COLLATE` 。

如前所述,校对在对用 `ORDER BY`子句检索出来的数据排序时起重要
的作用。如果你需要用与创建表时不同的校对顺序排序特定的 `SELECT` 语
句,可以在 `SELECT `语句自身中进行:

```shell
MariaDB [test]> create table db1.t2 (name varchar(10) character set latin1 collate latin1_general_ci);
Query OK, 0 rows affected (0.06 sec)

MariaDB [test]> insert into db1.t2 values ('a'),('b'),('A'),('B');
Query OK, 4 rows affected (0.07 sec)
Records: 4  Duplicates: 0  Warnings: 0

MariaDB [test]> select * from db1.t2;
+------+
| name |
+------+
| a    |
| b    |
| A    |
| B    |
+------+
4 rows in set (0.00 sec)

MariaDB [test]> select * from db1.t2 order by name collate latin1_general_cs;
+------+
| name |
+------+
| A    |
| a    |
| B    |
| b    |
+------+
4 rows in set (0.00 sec)

MariaDB [test]> select * from db1.t2 order by name;
+------+
| name |
+------+
| a    |
| A    |
| b    |
| B    |
+------+
4 rows in set (0.00 sec)

```

此 SELECT 使用 COLLATE 指定一个备用的校对顺序(在这个例子
中,为区分大小写的校对)。这显然将会影响到结果排序的次序。

**临时区分大小写** 上面的 SELECT 语句演示了在通常不区分大
小写的表上进行区分大小写搜索的一种技术。当然,反过来
也是可以的。

**ELECT 的其他 COLLATE 子句** 除了这里看到的在 `ORDER BY`子
句 中使用以外, `COLLATE` 还可以用于 `GROUP BY `、 `HAVING` 、聚集
函数、别名等。

最后,值得注意的是,如果绝对需要,串可以在字符集之间进行转
换。为此,使用 Cast() 或 Convert ()函数。

> 小结
>> 本章中,我们学习了字符集和校对的基础知识,还学习了如何对特
定的表和列定义字符集和校对,如何在需要时使用备用的校对。


---

### DCL语言

- DCL(Data Control Language)语句:数据控制语句,用于控制不同数据段直接的许可和
访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要的
语句关键字包括 grant、revoke 等。

#### 安全管理

数据库服务器通常包含关键的数据,确保这些数据的安全和完整需
要利用访问控制。本章将学习MySQL的访问控制和用户管理。

> 访问控制

MySQL服务器的安全基础是:用户应该对他们需要的数据具有适当
的访问权,既不能多也不能少。换句话说,用户不能对过多的数据具有
过多的访问权。

考虑以下内容:

* 多数用户只需要对表进行读和写,但少数用户甚至需要能创建和
删除表;
* 某些用户需要读表,但可能不需要更新表;
* 你可能想允许用户添加数据,但不允许他们删除数据;
* 某些用户(管理员)可能需要处理用户账号的权限,但多数用户
不需要;
* 你可能想让用户通过存储过程访问数据,但不允许他们直接访问
数据;
* 你可能想根据用户登录的地点限制对某些功能的访问。

这些都只是例子,但有助于说明一个重要的事实,即你需要给用户
提供他们所需的访问权,且仅提供他们所需的访问权。这就是所谓的访
问控制,管理访问控制需要创建和管理用户账号。

**使用MySQL Administrator** MySQL Administrator
提供了一个图形用户界面,可用来管理用户及账号权限。
MySQL Administrator在内部利用本章介绍的语句,使你能交互
地、方便地管理访问控制。

我们知道,为了执行数据库操作,需要登录
MySQL。MySQL创建一个名为 `root` 的用户账号,它对整个MySQL服务
器具有完全的控制。你可能已经在本书各章的学习中使用 root 进行过登
录,在对非现实的数据库试验MySQL时,这样做很好。不过在现实世界
的日常工作中,决不能使用 `root` 。应该创建一系列的账号,有的用于管
理,有的供用户使用,有的供开发人员使用,等等。

**防止无意的错误**
重要的是注意到,访问控制的目的不仅仅
是防止用户的恶意企图。数据梦魇更为常见的是无意识错误
的结果,如错打MySQL语句,在不合适的数据库中操作或其
他一些用户错误。通过保证用户不能执行他们不应该执行的
语句,访问控制有助于避免这些情况的发生。

**不要使用 root** 应该严肃对待 `root` 登录的使用。仅在绝对需
要时使用它(或许在你不能登录其他管理账号时使用)。不应
该在日常的MySQL操作中使用 `root `。

> 管理用户

MySQL用户账号和信息存储在名为 `mysql` 的MySQL数据库中。一般
不需要直接访问 `mysql` 数据库和表(你稍后会明白这一点),但有时需要
直接访问。需要直接访问它的时机之一是在需要获得所有用户账号列表
时。为此,可使用以下代码:

```shell
MariaDB [test]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> select user from user;
+------+
| user |
+------+
| root |
| root |
| root |
|      |
| root |
|      |
| root |
+------+
7 rows in set (0.00 sec)
```

mysql 数据库有一个名为 `user` 的表,它包含所有用户账号。 user
表有一个名为 `user` 的列,它存储用户登录名。新安装的服务器
可能只有一个用户(如这里所示),过去建立的服务器可能具有很多用户。

**用多个客户机进行试验** 试验对用户账号和权限进行更改的
最好办法是打开多个数据库客户机(如 mysql 命令行实用程序的
多个副本),一个作为管理登录,其他作为被测试的用户登录。

1.创建用户账号

>> 为了创建一个新用户账号,使用 CREATE USER 语句,如下所示:

```shell
MariaDB [mysql]> create user superman identified by 'p@$$w0rd';
Query OK, 0 rows affected (0.00 sec)
```

CREATE USER 创建一个新用户账号。在创建用户账号时不一定需
要口令,不过这个例子用 `IDENTIFIED BY 'p@$$wOrd'` 给出了
一个口令。

如果你再次列出用户账号,将会在输出中看到新账号。
指定散列口令 `IDENTIFIED BY` 指定的口令为纯文本, MySQL
将在保存到 `user` 表之前对其进行加密。为了作为散列值指定口
令,使用 `IDENTIFIED BY PASSWORD` 。

**使用 GRANT 或 INSERT** GRANT 语句(稍后介绍)也可以创建用
户账号,但一般来说 `CREATE USER` 是最清楚和最简单的句子。
此外,也可以通过直接插入行到 `user` 表来增加用户,不过为安
全起见,一般不建议这样做。MySQL用来存储用户账号信息
的表(以及表模式等)极为重要,对它们的任何毁坏都

可能严重地伤害到MySQL服务器。因此,相对于直接处理来
说,最好是用标记和函数来处理这些表。

>> 为重新命名一个用户账号,使用 RENAME USER 语句,如下所示:

```shell
MariaDB [mysql]> rename user superman@'%' to batman;
Query OK, 0 rows affected (0.00 sec)
MariaDB [mysql]> rename user batman@'%' to superman@'%';
Query OK, 0 rows affected (0.00 sec)
```

**MySQL 5之前** 仅MySQL 5或之后的版本支持 `RENAME USER` 。
为了在以前的MySQL中重命名一个用户,可使用 `UPDATE `直接
更新 `user` 表。

2.删除用户账号

>> 为了删除一个用户账号(以及相关的权限),使用 DROP USER 语句,
如下所示:

```shell
MariaDB [mysql]> drop user superman;
Query OK, 0 rows affected (0.00 sec)
```

**MySQL 5之前** 自MySQL 5以来, DROP USER 删除用户账号和
所有相关的账号权限。在MySQL 5以前, DROP USER 只能用来
删除用户账号,不能删除相关的权限。因此,如果使用旧版
本的MySQL,需要先用 REVOKE 删除与账号相关的权限,然后
再用 DROP USER 删除账号。

3.设置访问权限

在创建用户账号后,必须接着分配访问权限。新创建的用户账号没有访
问权限。它们能登录MySQL,但不能看到数据,不能执行任何数据库操作。

>> 为看到赋予用户账号的权限,使用 SHOW GRANTS FOR ,如下所示:

```shell
MariaDB [mysql]> create user 'wonderwoman'@'172.25.0.12';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> show grants for 'wonderwoman'@'172.25.0.12';
+---------------------------------------------------+
| Grants for wonderwoman@172.25.0.12                |
+---------------------------------------------------+
| GRANT USAGE ON *.* TO 'wonderwoman'@'172.25.0.12' |
+---------------------------------------------------+
1 row in set (0.00 sec)
```

输出结果显示用户 `wonderwoman` 有一个权限 `USAGE ON *.*` 。
`USAGE` 表示根本没有权限(我知道,这不很直观),所以,此结果表示在
任意数据库和任意表上对任何东西没有权限。

**用户定义为 `user@host`** MySQL的权限用用户名和主机名结
合定义。如果不指定主机名,则使用默认的主机名 `%` (授予用户访问权限而不管主机名)。

为设置权限,使用 GRANT 语句。 GRANT 要求你至少给出以下信息:

* 要授予的权限;
* 被授予访问权限的数据库或表;
* 用户名。

>> 以下例子给出 GRANT 的用法:

```shell
MariaDB [mysql]> grant select on db1.* to 'wonderwoman'@'172.25.0.12';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> show grants for 'wonderwoman'@'172.25.0.12';
+--------------------------------------------------------+
| Grants for wonderwoman@172.25.0.12                     |
+--------------------------------------------------------+
| GRANT USAGE ON *.* TO 'wonderwoman'@'172.25.0.12'      |
| GRANT SELECT ON `db1`.* TO 'wonderwoman'@'172.25.0.12' |
+--------------------------------------------------------+
2 rows in set (0.00 sec)
```

此 `GRANT` 允许用户在 `db1.*` ( db1 数据库的所
有表)上使用 `SELECT `。通过只授予 `SELECT` 访问权限,用户 `wonderwoman`
对 `db1` 数据库中的所有数据具有只读访问权限。

每个 `GRANT` 添加(或更新)用户的一个权限。MySQL读取所有
授权,并根据它们确定权限。

>> `GRANT` 的反操作为 `REVOKE` ,用它来撤销特定的权限。下面举一个例子:

```shell
MariaDB [mysql]> revoke select on db1.* from 'wonderwoman'@'172.25.0.12';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> show grants for 'wonderwoman'@'172.25.0.12';
+---------------------------------------------------+
| Grants for wonderwoman@172.25.0.12                |
+---------------------------------------------------+
| GRANT USAGE ON *.* TO 'wonderwoman'@'172.25.0.12' |
+---------------------------------------------------+
1 row in set (0.00 sec)
```

这条 `REVOKE` 语句取消刚赋予用户 `wonderwoman` 的 `SELECT` 访问权限。被
撤销的访问权限必须存在,否则会出错。

GRANT 和 REVOKE 可在几个层次上控制访问权限:

* 整个服务器,使用 GRANT ALL 和 REVOKE ALL;
* 整个数据库,使用 ON database.*;
* 特定的表,使用 ON database.table;
* 特定的列;
* 特定的存储过程。

下表列出可以授予或撤销的每个权限。

|权限|说明|
|:--|:--|
|ALL |除GRANT OPTION外的所有权限|
|ALTER| 使用ALTER TABLE|
|ALTER ROUTINE| 使用ALTER PROCEDURE和DROP PROCEDURE|
|CREATE |使用CREATE TABLE|
|CREATE ROUTINE| 使用CREATE PROCEDURE|
|CREATE TEMPORARY TABLES|使用CREATE TEMPORARY TABLE|
|CREATE USER|使用CREATE USER、 DROP USER、 RENAME USER和REVOKE ALL PRIVILEGES|
|CREATE VIEW |使用CREATE VIEW|
|DELETE| 使用DELETE|
|DROP| 使用DROP TABLE|
|EXECUTE| 使用CALL和存储过程|
|FILE |使用SELECT INTO OUTFILE和LOAD DATA INFILE|
|GRANT OPTION| 使用GRANT和REVOKE|
|INDEX |使用CREATE INDEX和DROP INDEX|
|INSERT |使用INSERT|
|LOCK TABLES| 使用LOCK TABLES|
|PROCESS |使用SHOW FULL PROCESSLIST|
|RELOAD |使用FLUSH|
|REPLICATION CLIENT |服务器位置的访问|
|REPLICATION SLAVE |由复制从属使用|
|SELECT|使用SELECT|
|SHOW DATABASES| 使用SHOW DATABASES|
|SHOW VIEW |使用SHOW CREATE VIEW|
|SHUTDOWN| 使用mysqladmin shutdown(用来关闭MySQL)|
|SUPER|使用CHANGE MASTER、KILL、LOGS、PURGE、MASTER和SET GLOBAL。还允许mysqladmin调试登录|
|UPDATE| 使用UPDATE|
|USAGE| 无访问权限|

使用 GRANT 和 REVOKE ,再结合上表中列出的权限,你能对用户可以
就你的宝贵数据做什么事情和不能做什么事情具有完全的控制。

**未来的授权** 在使用 GRANT 和 REVOKE 时,用户账号必须存在,
但对所涉及的对象没有这个要求。这允许管理员在创建数据库
和表之前设计和实现安全措施。

这样做的副作用是,当某个数据库或表被删除时(用 DROP 语句),
相关的访问权限仍然存在。而且,如果将来重新创建该
数据库或表,这些权限仍然起作用。

**简化多次授权** 可通过列出各权限并用逗号分隔,将多条
GRANT 语句串在一起,如下所示:
grant select,insert on db1.* to 'wonderwoman'@'172.25.0.12';

4.更改口令

>> 为了更改用户口令,可使用 SET PASSWORD 语句。新口令必须如下加密:

```shell
MariaDB [mysql]> create user bforta;
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> set password for bforta = password('uplooking');
Query OK, 0 rows affected (0.00 sec)
```
SET PASSWORD 更新用户口令。新口令必须传递到 Password() 函
数进行加密。

SET PASSWORD 还可以用来设置你自己的口令:`set password = password('uplooking');`

在不指定用户名时, SET PASSWORD 更新当前登录用户的口令。

> 小结
>> 本章学习了通过赋予用户特殊的权限进行访问控制和保护MySQL服务器。

### 其他
####  MySQL语句的语法

> 通过帮助可以查看mysql的语法

```shell
MariaDB [mysql]> help;

General information about MariaDB can be found at
http://mariadb.org

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'
```

> 查看`create`和`create database`的帮助

```shell
MariaDB [mysql]> help create;
Many help items for your request exist.
To make a more specific request, please type 'help <item>',
where <item> is one of the following
topics:
   CREATE DATABASE
   CREATE EVENT
   CREATE FUNCTION
   CREATE FUNCTION UDF
   CREATE INDEX
   CREATE PROCEDURE
   CREATE SERVER
   CREATE TABLE
   CREATE TABLESPACE
   CREATE TRIGGER
   CREATE USER
   CREATE VIEW
   SHOW
   SHOW CREATE DATABASE
   SHOW CREATE EVENT
   SHOW CREATE FUNCTION
   SHOW CREATE PROCEDURE
   SHOW CREATE TABLE
   SPATIAL

MariaDB [mysql]> help create database;
Name: 'CREATE DATABASE'
Description:
Syntax:
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name

CREATE DATABASE creates a database with the given name. To use this
statement, you need the CREATE privilege for the database. CREATE
SCHEMA is a synonym for CREATE DATABASE.

URL: http://dev.mysql.com/doc/refman/5.5/en/create-database.html
```

下表罗列出常见的DDL、DML、DCL、DQL的用法：

~~~
DDL	数据库定义语言	create\drop\alter

      create database [dbname];
      drop database [dbname];
      create table [tbname] (col1 type,col2 type,col3....);
      drop table [tbname];

DML	数据库操作语言	insert into\delete from\update
      insert into [tbname] set col1=value,col2=value,col3=value;
      insert into [tbname] values (1,'booboo'),(2,'batman'),(3,'superman');
      insert into [tbname] (name,id) values ();
      delete from [tbname] where id=1 and name='booboo';
      update [tbname] set col='superman' where id=2;

DCL	数据库控制语言	grant revoke
      认证 + 授权
      grant all on *.* to booboo@'172.25.0.11' identified by 'uplooking';
        all 权限
        *.* 库.表
      flush privileges; 刷新授权表
      revoke [权限] on [库].[表] from booboo@'172.25.0.11';


DQL	数据库查询语言	show databases;
      use mysql;
      show tables;
      desc mysql.user;
      select * from db1.t1;
~~~


####  MySQL数据类型

数据类型是定义列中可以存储什么数据以及该数据
实际怎样存储的基本规则。

数据类型用于以下目的。

* 数据类型允许限制可存储在列中的数据。例如,数值数据类型列
只能接受数值。
* 数据类型允许在内部更有效地存储数据。可以用一种比文本串更
简洁的格式存储数值和日期时间值。
* 数据类型允许变换排序顺序。如果所有数据都作为串处理,则1
位于10之前,而10又位于2之前(串以字典顺序排序,从左边开始
比较,一次一个字符)。作为数值数据类型,数值才能正确排序。

在设计表时,应该特别重视所用的数据类型。使用错误的数据类型
可能会严重地影响应用程序的功能和性能。更改包含数据的列不是一件
小事(而且这样做可能会导致数据丢失)。

本章虽然不是关于数据类型及其如何使用的一个完整的教材,但
介绍了MySQL主要的数据类型和用途。

>> 串数据类型

最常用的数据类型是串数据类型。它们存储串,如名字、地址、电
话号码、邮政编码等。有两种基本的串类型,分别为定长串和变长串。

* 定长串接受长度固定的字符串,其长度是在创建表时指定的。例如,
名字列可允许30个字符,而社会安全号列允许11个字符(允许的字符数
目中包括两个破折号)。定长列不允许多于指定的字符数目。它们分配的
存储空间与指定的一样多。因此,如果串 Ben 存储到30个字符的名字字段,
则存储的是30个字符, CHAR 属于定长串类型。

* 变长串存储可变长度的文本。有些变长数据类型具有最大的定长,
而有些则是完全变长的。不管是哪种,只有指定的数据得到保存(额外
的数据不保存) TEXT 属于变长串类型。

既然变长数据类型这样灵活,为什么还要使用定长数据类型?回答是
因为性能。MySQL处理定长列远比处理变长列快得多。此外,MySQL不
允许对变长列(或一个列的可变部分)进行索引。这也会极大地影响性能。

|串数据类型|说明|
|:-|:--|
|CHAR|1~255个字符的定长串。它的长度必须在创建时指定,否则MySQL假定为CHAR(1)|
|ENUM| 接受最多64 K个串组成的一个预定义集合的某个串|
|LONGTEXT| 与TEXT相同,但最大长度为4 GB|
|MEDIUMTEXT| 与TEXT相同,但最大长度为16 K|
|SET| 接受最多64个串组成的一个预定义集合的零个或多个串|
|TEXT| 最大长度为64 K的变长文本|
|TINYTEXT| 与TEXT相同,但最大长度为255字节|
|VARCHAR|长度可变,最多不超过255字节。如果在创建时指定为VARCHAR(n),
则可存储0到n个字符的变长串(其中n≤255)|

**使用引号** 不管使用何种形式的串数据类型,串值都必须括在
引号内(通常单引号更好)。

**当数值不是数值时** 你可能会认为电话号码和邮政编码应该
存储在数值字段中(数值字段只存储数值数据),但是,这样
做却是不可取的。如果在数值字段中存储邮政编码01234,则
保存的将是数值1234,实际上丢失了一位数字。

需要遵守的基本规则是:如果数值是计算(求和、平均等)中使
用的数值,则应该存储在数值数据类型列中。如果作为字符串(可
能只包含数字)使用,则应该保存在串数据类型列中。

> 数值数据类型

数值数据类型存储数值。MySQL支持多种数值数据类型,每种存储
的数值具有不同的取值范围。显然,支持的取值范围越大,所需存储空
间越多。此外,有的数值数据类型支持使用十进制小数点(和小数),而
有的则只支持整数。下表列出了常用的MySQL数值数据类型。

**有符号或无符号** 所有数值数据类型(除 `BIT` 和 `BOOLEAN` 外)
都可以有符号或无符号。有符号数值列可以存储正或负的数
值,无符号数值列只能存储正数。默认情况为有符号,但如
果你知道自己不需要存储负值,可以使用 `UNSIGNED` 关键字,
这样做将允许你存储两倍大小的值。

|数值数据类型|说明|
|:--|:--|
|BIT|位字段,1~64位。(在MySQL 5之前,BIT在功能上等价于TINYINT|
|BIGINT|整数值,支持9223372036854775808~9223372036854775807(如果是UNSIGNED,为0~18446744073709551615)的数|
|BOOLEAN(或BOOL)| 布尔标志,或者为0或者为1,主要用于开/关(on/off)标志|
|DECIMAL(或DEC)| 精度可变的浮点值|
|DOUBLE |双精度浮点值|
|FLOAT |单精度浮点值|
|INT(或INTEGER)| 整数值,支持2147483648~2147483647 (如果是UNSIGNED,为0~4294967295)的数|
|MEDIUMINT| 整数值,支持8388608~8388607(如果是UNSIGNED,为0~16777215)的数|
|REAL|4字节的浮点值|
|SMALLINT|整数值,支持32768~32767(如果是UNSIGNED,为0~65535)的数|
|TINYINT|整数值,支持128~127(如果为UNSIGNED,为0~255)的数|

**不使用引号** 与串不一样,数值不应该括在引号内。

**存储货币数据类型** MySQL中没有专门存储货币的数据类型,一般情况下使用 DECIMAL(8, 2)

> 日期和时间数据类型

MySQL使用专门的数据类型来存储日期和时间值。见下表。

|日期和时间数据类型|说明|
|:--|:--|
|DATE|表示1000-01-01~9999-12-31的日期,格式为YYYY-MM-DD|
|DATETIME| DATE和TIME的组合|
|TIMESTAMP| 功能和DATETIME相同(但范围较小)|
|TIME| 格式为HH:MM:SS|
|YEAR|用2位数字表示,范围是70(1970年)~69(2069年),用4位数字表示,范围是1901年~2155年|

> 二进制数据类型

二进制数据类型可存储任何数据(甚至包括二进制信息),如图像、
多媒体、字处理文档等(见下表）。

|二进制数据类型|说明|
|:--|:--|
|BLOB| Blob最大长度为64 KB|
|MEDIUMBLOB| Blob最大长度为16 MB|
|LONGBLOB| Blob最大长度为4 GB|
|TINYBLOB| Blob最大长度为255字节|

---

### 实战项目

> 实战项目1:熟悉SQL语句
根据下面的表格练习sql语句（书店的库存数据库）

![sql](pic/22-sql.png)

在该表格中,编号、书名、作者、进货日期、价格、数量就是不同的列,
而每一行保存的则是具体的数据。

建立一个名字为 bookshop 的库,在这个库中建立一张关于书籍库存名字为 reserve 的表,表的结构如图所示。

```shell
[root@serverg ~]# mysql -uroot -p
Enter password:
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.41-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
#显示库
MariaDB [(none)]> show databases;
+--------------------+
| Database
|
+--------------------+
| information_schema |
| mysql
|
| performance_schema |
| test
|
+--------------------+
4 rows in set (0.00 sec)
#建库
MariaDB [(none)]> create database db1;
Query OK, 1 row affected (0.00 sec)
#使用库
MariaDB [(none)]> use db1;
Database changed
#删除库
MariaDB [db1]> drop database db1;
Query OK, 0 rows affected (0.02 sec)
MariaDB [(none)]> create database booboo;
Query OK, 1 row affected (0.00 sec)
#使用库
MariaDB [(none)]> use booboo;
Database changed
#建表
MariaDB [booboo]> create table bookshop (
-> id int primary key,
列名为 id 整数型 主建
-> bookname varchar(50) not null,
列名为 bookname 可变长度字符类型最长为 50 字节 不为
空
-> writer varchar(50),
列名为 writer 可变长度字符类型最长为 50 字节
-> bookdate date,
列名为 bookdate 日期型
-> price float,
列名为 price 浮点型
-> amount int
列名为 amount 整数型
-> );
Query OK, 0 rows affected (0.05 sec)
#显示表
MariaDB [booboo]> show tables ;
+------------------+
| Tables_in_booboo |
+------------------+
| bookshop
|
+------------------+
1 row in set (0.00 sec)
#插入表数据
MariaDB [booboo]> insert into bookshop values (1,"Live with Linux","Tube","2007-1-25",75.00,50);
Query OK, 1 row affected (0.01 sec)
MariaDB [booboo]> insert into bookshop values (2,"Linux inside","Kevin","2008-2-15",83.00,50)
-> ;
Query OK, 1 row affected (0.06 sec)
MariaDB [booboo]> insert into bookshop values
-> (3,"L.A.M.P","Tom","2008-2-5",82.5,50),
-> (4,"My way","Jam","2007-12-3",45.25,130),
-> (5,"Open your heart","July","2007-3-8",35,20),
-> (6,"Pro C","Todd","2007-8-25",85,25),
-> (7,"Thinking in C","John","2006-6-13",65,30);
Query OK, 5 rows affected (0.03 sec)
Records: 5 Duplicates: 0 Warnings: 0
#显示表数据
MariaDB [booboo]> select * from bookshop;
+----+-----------------+--------+------------+-------+--------+
| id | bookname
| writer | bookdate | price | amount |
+----+-----------------+--------+------------+-------+--------+
| 1 | Live with Linux | Tube | 2007-01-25 | 75 | 50 |
| 2 | Linux inside | Kevin | 2008-02-15 | 83 | 50 |
| 3 | L.A.M.P
| Tom | 2008-02-05 | 82.5 | 50 |
| 4 | My way
| Jam | 2007-12-03 | 45.25 | 130 |
| 5 | Open your heart | July | 2007-03-08 | 35 | 20 |
| 6 | Pro C
| Todd | 2007-08-25 | 85 | 25 |
| 7 | Thinking in C | John | 2006-06-13 | 65 | 30 |
+----+-----------------+--------+------------+-------+--------+
7 rows in set (0.00 sec)
#查询表数据
MariaDB [booboo]> select bookname,writer from bookshop where id=4;
+----------+--------+
| bookname | writer |
+----------+--------+
| My way | Jam |
+----------+--------+
1 row in set (0.00 sec)
MariaDB [booboo]> select bookname,writer from bookshop where id=4 or id=5;
+-----------------+--------+
| bookname
| writer |
+-----------------+--------+
| My way
| Jam |
| Open your heart | July |
+-----------------+--------+
2 rows in set (0.00 sec)
#修改表数据
MariaDB [booboo]> update bookshop set
-> writer="booboo"
-> where id=1;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1 Changed: 1 Warnings: 0
#删除表数据
MariaDB [booboo]> delete from bookshop where id=7;
Query OK, 1 row affected (0.02 sec)
MariaDB [booboo]> select * from bookshop;
+----+-----------------+--------+------------+-------+--------+
| id | bookname
| writer | bookdate | price | amount |
+----+-----------------+--------+------------+-------+--------+
| 1 | Live with Linux | booboo | 2007-01-25 | 75 | 50 |
| 2 | Linux inside | Kevin | 2008-02-15 | 83 | 50 |
| 3 | L.A.M.P
| Tom | 2008-02-05 | 82.5 | 50 |
| 4 | My way
| Jam | 2007-12-03 | 45.25 | 130 |
| 5 | Open your heart | July | 2007-03-08 | 35 | 20 |
| 6 | Pro C
| Todd | 2007-08-25 | 85 | 25 |
+----+-----------------+--------+------------+-------+--------+
6 rows in set (0.00 sec)
```

> 实战项目2:熟悉mysql.user表

1. 查看mysql库中user表中的host,user,password列的值；
2. 删除mysql库中的user表中，user列为空或者password列为空的行；

```shell
MariaDB [mysql]> select host,user,password from mysql.user;
+----------------------+------+-------------------------------------------+
| host                 | user | password                                  |
+----------------------+------+-------------------------------------------+
| localhost            | root | *6FF883623B8639D08083FF411D20E6856EB7D2BF |
| mastera0.example.com | root |                                           |
| 127.0.0.1            | root |                                           |
| ::1                  | root |                                           |
| localhost            |      |                                           |
| mastera0.example.com |      |                                           |
+----------------------+------+-------------------------------------------+
6 rows in set (0.00 sec)

MariaDB [mysql]> delete from mysql.user where user=' ' or password=' ';
Query OK, 5 rows affected (0.00 sec)

MariaDB [mysql]> select host,user,password from mysql.user;
+-----------+------+-------------------------------------------+
| host      | user | password                                  |
+-----------+------+-------------------------------------------+
| localhost | root | *6FF883623B8639D08083FF411D20E6856EB7D2BF |
+-----------+------+-------------------------------------------+
1 row in set (0.00 sec)
```

> 实战项目3：完成数据库用户权限操作项目
>> 要求:添加授权用户测试客户机优先使用的哪个密码(X为学生机号)

1. user1@'172.25.X.%' uplooking
2. user1@'172.25.X.12' uplooking123


```shell
# mastera:

MariaDB [mysql]> grant all on *.* to user1@"172.25.0.%" identified by "uplooking";
Query OK, 0 rows affected (0.00 sec)
MariaDB [mysql]> grant all on *.* to user1@"172.25.0.12" identified by "uplooking123";
Query OK, 0 rows affected (0.00 sec)
MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.01 sec)
MariaDB [mysql]> \q
Bye

# masterb:

# 第一次输入密码为: uplooking
[root@serverb ~]# mysql -uuser1 -h172.25.0.11 -p'uplooking'
ERROR 1045 (28000): Access denied for user 'user1'@'mastera0.example.com' (using password:YES)
#第二次输入密码为:uplooking123
[root@serverb ~]# mysql -uuser1 -h172.25.0.11 -p'uplooking123'
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 7
Server version: 5.5.41-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]>

```
实验结论:uplooking2 密码进去了 授权越精确越优先

> 实战项目4: 破解 MariaDB 5.5 的 root 密码

1. 停止服务 `systemctl stop mariadb`
2. 跳过授权表启动服务 `mysqld_safe --skip-grant-tables &`
3. 修改root密码 `update mysql.user set password=password('uplooking') where user='root'; `
4. 停止跳过授权表启动服务 `kill -9`
5. 启动服务 `systemctl start mariadb`

```shell
# rhel7 mariadb5.5
[root@serverg ~]# systemctl stop mariadb
[root@serverg ~]# mysqld_safe --skip-grant-tables &
[1] 3078
[root@serverg ~]# 160304 18:36:15 mysqld_safe Logging to '/var/log/mariadb/mariadb.log'.
160304 18:36:15 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
[root@serverg ~]# mysql -uxxx
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 1
Server version: 5.5.41-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
MariaDB [mysql]> update user set password=password("redhat") where user="root" and
host="localhost";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1 Changed: 1 Warnings: 0
MariaDB [mysql]> \q
Bye
[root@serverg ~]# ps -ef |grep mysql
mysql
3221
1 0 18:36 ?
00:00:00 /usr/libexec/mysqld --basedir=/usr
--datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --user=mysql --skip-grant-tables --log-
error=/var/log/mariadb/mariadb.log
--pid-file=/var/run/mariadb/mariadb.pid
--socket=/var/lib/mysql/mysql.sock
root
3287 3256 0 18:40 pts/0 00:00:00 grep --color=auto mysql
[root@serverg ~]# kill -9 3221
[root@serverg ~]# systemctl start mariadb
[root@serverg ~]# mysql -uroot -predhat
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 5.5.41-MariaDB MariaDB Server
Copyright (c) 2000, 2014, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]>
```

### 总结

本章要求掌握sql语句的基本用法，包括`create database`,`create table`,
`drop database`,`drop table`,`insert into`,`update`,`delete from`,`grant`,`revoke`;
其他sql语句作为拓展。
