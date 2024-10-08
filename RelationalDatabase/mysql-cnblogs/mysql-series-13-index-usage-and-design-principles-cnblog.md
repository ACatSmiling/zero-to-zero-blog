﻿>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-21`*

## 索引的声明与使用

### 索引的分类

MySQL 的索引包括普通索引、唯一性索引、全文索引、单列索引、多列索引和空间索引等。

- 从`功能逻辑`上说，索引主要有 4 种：普通索引、唯一索引、主键索引、全文索引。
- 按照`物理实现方式`，索引可以分为 2 种：聚簇索引和非聚簇索引。
- 按照`作用字段个数`进行划分，分成单列索引和联合索引。

#### 普通索引

在创建普通索引时，不附加任何限制条件，只是用于提高查询效率。这类索引可以创建在`任何数据类型`中，其值是否唯一和非空，要由字段本身的完整性约束性条件决定。建立索引以后，可以通过索引进行查询。例如，在表 student 的字段 name 上建立一个普通索引，查询记录时就可以根据该索引进行查询。

#### 唯一性索引

使用`UNIQUE 参数`可以设置索引为唯一性索引，在创建唯一性索引时，限制该索引的值必须是唯一的，但允许有空值。在一张数据表里，`可以有多个唯一索引`。例如，在表 student 的字段 email 中创建唯一性索引，那么字段 email 的值就必须是唯一的。通过唯一性索引，可以快速确定某条记录。

#### 主键索引

主键索引就是一种特殊的唯一性索引，在唯一索引的基础上增加了不为空的约束，也就是 NOT NULL + UNIQUE，一张表里`最多只有一个主键索引`。

#### 单列索引

在表中的单个字段上创建索引，单列索引只根据该字段进行索引。单列索引可以是普通索引，也可以是唯一性索引，还可以是全文索引，只要保证该索引只对应一个字段即可。一张表中，`可以有多个单列索引`。

#### 多列（组合、联合）索引

在表中的多个字段组合上创建索引。多列索引指向创建时对应的多个字段，可以通过这几个字段进行查询，但是只有查询条件中使用了这些字段中的第一个字段时才会被使用。例如，在表 student 的字段 id、name 和 gender 上建立一个多列索引 idx_id_name_gender，只有在查询条件中使用了字段 id 时，该索引才会被使用。使用多列索引时，遵循`最左前缀集合`。

#### 全文索引

全文索引，也称全文检索，是目前搜索引擎使用的一种关键技术。它能够利用`分词技术`等多种算法智能分析出文本文字中关键词的频率和重要性，然后按照一定的算法规则智能的筛选出想要的搜索结果。全文索引非常适合大型数据集，对于小的数据集，它的用处比较小。

使用参数`FULLTEXT`可以设置索引为全文索引，在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值。全文索引只能创建在`CHAR`、`VARCHAR`或`TEXT`类型及其系列类型的字段上，`查询数据量较大的字符串类型的字段时，使用全文索引可以提高查询速度`。例如，表 student 的字段 information 是 TEXT 类型，该字段包含了很多文字信息，在字段 information 上建立全文索引后，可以提高查询字段 information 的速度。

全文索引典型的有两种类型：自然语言的全文索引和布尔全文索引。

- 自然语言搜索引擎将计算每一个文档对象和查询的相关度。这里，相关度是基于匹配的关键词的个数，以及关键词在文档中出现的次数。`在整个索引中出现次数越少的词语，匹配时的相关度就越高。`相反，非常常见的单词将不会被搜索，如果一个词语在超过 50% 的记录中都出现了，那么自然语言的搜索将不会搜索这类词语。

MySQL 数据库从 3.23.23 版开始支持全文索引，但在 MySQL 5.6.4 之前只有 MyISAM 支持，5.6.4 版本以后 InnoDB 才支持，但是官方版本不支持中文分词，需要第三方分词插件。在 5.7.6 版本，MySQL 内置了`ngram 全文解析器`，用来支持亚洲语种的分词。测试或使用全文检索时，需要先查看一下 MySQL 版本、存储引擎和数据类型，是否支持全文索引。

随着大数据时代的到来，关系型数据库应对全文索引的需求已力不从心，逐渐被 Solr、ElasticSearch 等专门的搜索引擎所替代。

#### 补充：空间索引

使用参数`SPATIAL`可以设置索引为空间索引。空间索引只能建立在空间数据类型上，这样可以提高系统获取空间数据的效率。MySQL 中的空间数据类型包括`GEOMETRY`、`POINT`、`LINESTRING`和`POLYGON`等。目前只有 MyISAM 存储引擎支持空间检索，而且索引的字段不能为空值。

#### 小结

不同的存储引擎支持的索引类型也不一样。

- InnoDB：支持 B+Tree、Full-Text 等索引，不支持 Hash 索引；
- MyISAM：支持 B+Tree、Full-Text 等索引，不支持 Hash 索引；
- Memory：支持 B+Tree、Hash 等索引，不支持 Full-Text 索引； 
- NDB：支持 Hash 索引，不支持 B+Tree、Full-Text 等索引； 
- Archive：不支持 B+Tree、Hash、Full-Text 等索引。

### 创建索引

MySQL 支持多种方法在单个或多个列上创建索引：

- 在创建表的定义语句`CREATE TABLE`中指定索引列。
- 使用`ALTER TABLE`语句在存在的表上创建索引。
- 使用`CREATE INDEX`语句在已存在的表上添加索引。

#### 创建表的时候创建索引

使用 CREATE TABLE 创建表时，除了可以定义列的数据类型外，还可以定义主键约束、外键约束或者唯一性约束，而不论创建哪种约束，在定义约束的同时，相当于在指定列上创建了一个索引。

`隐式的索引创建`：

```mysql
# 1. 隐式的添加索引（在添加有主键约束、唯一性约束或者外键约束的字段会自动的创建索引）
CREATE TABLE dept(
    dept_id INT PRIMARY KEY AUTO_INCREMENT, # 创建主键索引
    dept_name VARCHAR(20)
)

CREATE TABLE emp(
    emp_id INT PRIMARY KEY AUTO_INCREMENT, # 主键索引
    emp_name VARCHAR(20) UNIQUE, # 唯一索引
    dept_id INT,
    CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept_id)
) # 外键索引
```

`显式的索引创建`，基本语法格式如下，共有七种情况：

```mysql
CREATE TABLE table_name [col_name data_type]
[UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC | DESC]
```

- UNIQUE、FULLTEXT 和 SPATIAL 为可选参数，分别表示唯一索引、全文索引和空间索引。
- INDEX 与 KEY 为同义词，两者的作用相同，用来指定创建索引。
- index_name 指定索引的名称，为可选参数，如果不指定，那么 MySQL 默认 col_name 为索引名。
- col_name 为需要创建索引的字段列，该字段列必须从数据表中定义的多个列中选择。
- length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度。
- ASC 或 DESC 指定升序或者降序的索引值存储。
- 特例：主键索引使用主键约束的方式来创建。

**创建普通索引：**

在 book 表中的 year_publication 字段上建立普通索引，SQL 语句如下：

```mysql
CREATE TABLE book(
    book_id INT,
    book_name VARCHAR(100),
    authors VARCHAR(100),
    info VARCHAR(100) ,
    comment VARCHAR(100),
    year_publication YEAR,
    # 声明索引
    INDEX idx_bname(year_publication)
)
```

**创建唯一索引：**

```mysql
CREATE TABLE test(
	id INT NOT NULL,
	name varchar(30) NOT NULL,
	UNIQUE INDEX uk_idx_id(id)
)
```

**主键索引：**

设定为主键后数据库会自动建立索引，InnoDB 为聚簇索引，语法：

```mysql
CREATE TABLE student (
    # 通过定义主键约束的方式定义主键索引
	id INT(10) UNSIGNED AUTO_INCREMENT ,
	student_no VARCHAR(200),
	student_name VARCHAR(200),
	PRIMARY KEY(id)
)
```

删除主键索引：

```mysql
ALTER TABLE student DROP PRIMARY KEY
```

> 修改主键索引：必须先删除掉（DROP）原索引，再新建（ADD）索引。

**创建单列索引：**

```mysql
CREATE TABLE test2(
	id INT NOT NULL,
	name CHAR(50) NULL,
	INDEX single_idx_name(name(20))
)
```

**创建组合索引：**

```mysql
CREATE TABLE test3(
    id INT(11) NOT NULL,
    name CHAR(30) NOT NULL,
    age INT(11) NOT NULL,
    info VARCHAR(255),
    INDEX multi_idx(id, name, age)
)
```

> 组合索引在查询时会遵守`最左索引原则`，先进行 id 条件的比较，然后再进行 name 比较，最后才是 age，因此注意把最常用的查询字段放在索引的最左边。

**创建全文索引：**

FULLTEXT 全文索引可以用于全文搜索，并且只为`CHAR`、`VARCHAR`和`TEXT`列创建索引。索引总是对整个列进行，不支持局部（前缀）索引。

```mysql
CREATE TABLE test4(
    id INT NOT NULL,
    name CHAR(30) NOT NULL,
    age INT NOT NULL,
    info VARCHAR(255),
    FULLTEXT INDEX futxt_idx_info(info)
) ENGINE=MyISAM
```

> 在 MySQL 5.7 及之后版本中可以不指定最后的 ENGINE，因为在此版本中 InnoDB 支持全文索引。

创建一个给 title 和 body 字段添加全文索引的表：

```mysql
CREATE TABLE articles (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR (200),
    body TEXT,
    FULLTEXT index (title, body)
) ENGINE=INNODB
```

```mysql
CREATE TABLE `papers` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `title` varchar(200) DEFAULT NULL,
  `content` text,
  PRIMARY KEY (`id`),
  FULLTEXT KEY `title` (`title`, `content`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8
```

全文索引用 `MATCH + AGAINST` 方式查询：

```mysql
SELECT * FROM papers WHERE MATCH(title, content) AGAINST (‘查询字符串’);
```

>注意：
>
>1. 使用全文索引前，搞清楚版本支持情况；
>2. 全文索引比 LIKE + % 快 N 倍，但是可能存在精度问题；
>3. 如果需要全文索引的是大量数据，建议先添加数据，再创建索引。

**创建空间索引：**

空间索引创建中，要求空间类型的字段必须为`非空` 。

```mysql
CREATE TABLE test5(
	geo GEOMETRY NOT NULL,
	SPATIAL INDEX spa_idx_geo(geo)
) ENGINE=MyISAM
```

#### 在已经存在的表上创建索引

在已经存在的表中创建索引可以使用`ALTER TABLE`语句或者`CREATE INDEX`语句。

**使用 ALTER TABLE 语句创建索引：**

```mysql
ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY]
[index_name] (col_name[length],...) [ASC | DESC]
```

示例：

```mysql
# 唯一索引
ALTER TABLE book ADD UNIQUE INDEX uk_idex_bid(book_id)

# 单列索引
ALTER TABLE book ADD INDEX inx_cmt(comment(50))

# 组合索引
ALTER TABLE book ADD INDEX idx_author_info(authors(30), info(50))
```

声明主键索引：

```mysql
CREATE TABLE customer2(
	id INT(10) UNSIGNED,
    customer_no VARCHAR(200),
    customer_name VARCHAR(200)
)

ALTER TABLE customer2 ADD PRIMARY KEY customer2(id)
```

**使用 CREATE INDEX 创建索引：**

`CREATE INDEX`语句可以在已经存在的表上添加索引，在 MySQL 中，CREATE INDEX 被映射到一个 ALTER TABLE 语句上，基本语法结构为：

```mysql
CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
ON table_name (col_name[length],...) [ASC | DESC]
```

示例：

```mysql
# 普通索引
CREATE INDEX idx_cmt ON book(comment)

# 唯一索引
CREATE UNIQUE INDEX uk_idx_bid ON book(book_id)

# 组合索引
CREATE INDEX mul_bid_bname_info ON book(book_id, book_name, info)
```

### 删除索引

MySQ L中删除索引使用`ALTER TABLE`或`DROP INDEX`语句，两者可实现相同的功能，DROP INDEX 语句在内部被映射到一个 ALTER TABLE 语句中。

#### 使用 ALTER TABLE 删除索引

语法：

```mysql
ALTER TABLE table_name DROP INDEX index_name
```

示例：

```mysql
# 查看索引是否存在
SHOW INDEX FROM book\G

# 删除索引
ALTER TABLE book DROP INDEX idx_bk_id
```

>添加 AUTO_INCREMENT 约束字段的唯一索引不能被删除。

#### 使用 DROP INDEX 语句删除索引

语法：

```mysql
DROP INDEX index_name ON table_name
```

示例：

```mysql
# 删除索引
DROP INDEX idx_aut_info ON book

# 查看索引是否删除
SHOW INDEX FROM book\G
```

> 删除表中的列时，如果要删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除。

## MySQL 8.0 索引新特性

### 支持降序索引

`降序索引以降序存储键值。`虽然在语法上，从 MySQL 4 版本就已经支持降序索引，但实际上该 DESC 定义是被忽略的，直到 MySQL 8 版本才开始真正支持降序索引（仅限于 InnoDB 存储引擎）。

MySQL 在 8.0 版本之前创建的仍然是升序索引，使用时进行反向扫描，这大大降低了数据库的效率。在某些场景下，降序索引意义重大。例如，如果一个查询，需要对多个列进行排序，且顺序要求不一致，那么使用降序索引，将会避免数据库使用额外的文件排序操作，从而提高性能。

示例：

```mysql
mysql> CREATE TABLE ts1(a int, b int, index idx_a_b(a, b desc)); 
Query OK, 0 rows affected (0.04 sec)

mysql> SHOW CREATE TABLE ts1;
+-------+---------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------+
| ts1   | CREATE TABLE `ts1` (
  `a` int DEFAULT NULL,
  `b` int DEFAULT NULL,
  KEY `idx_a_b` (`a`,`b` DESC)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3 |
+-------+---------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

从结果可以看出，索引已经是降序了。

向数据表 ts1 中插入 800 条随机数据，执行语句如下：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE ts_insert()
    -> BEGIN
    -> DECLARE i INT DEFAULT 1;
    -> WHILE i < 800
    -> DO
    -> insert into ts1 select rand()*80000,rand()*80000;
    -> SET i = i + 1;
    -> END WHILE;
    -> commit;
    -> END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;
mysql> CALL ts_insert();
Query OK, 0 rows affected (3.95 sec)
```

在 MySQL 8.0 版本中查看数据表 ts1 的执行计划：

```mysql
mysql> EXPLAIN SELECT * FROM ts1 ORDER BY a, b DESC LIMIT 5;
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | ts1   | NULL       | index | NULL          | idx_a_b | 10      | NULL |    5 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.01 sec)
```

从结果可以看出，执行计划中扫描数为 5，并且没有使用 Using filesort。

> 可以对比 MySQL 5.0 版本中相同查询的执行计划，执行计划中扫描数远大于 MySQL 8.0，并且使用了 Using filesort 排序。
>
> Using filesort 是 MySQL 中一种速度比较慢的外部排序，能避免是最好的。多数情况下，管理员可以通过优化索引来尽量避免出现 Using filesort，从而提高数据库执行速度。

`降序索引只对查询中特定的排序顺序有效，如果使用不当，反而查询效率更低。`例如，将上述查询的排序条件改为`ORDER BY a desc, b desc`，然后在 MySQL 8.0 版本中查看数据表 ts1 的执行计划：

```mysql
mysql> EXPLAIN SELECT * FROM ts1 ORDER BY a DESC,b DESC LIMIT 5;
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-----------------------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | ts1   | NULL       | index | NULL          | idx_a_b | 10      | NULL |  799 |   100.00 | Using index; Using filesort |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-----------------------------+
1 row in set, 1 warning (0.00 sec)
```

> 此时再对比  MySQL 5.0 版本中相同查询的执行计划，会发现 MySQL 5.0 中执行计划会优于 MySQL 8.0。

### 隐藏索引

在 MySQL 5.7 版本及之前，只能通过显式的方式删除索引。此时，如果发现删除索引后出现错误，又只能通过显式创建索引的方式将删除的索引创建回来。如果数据表中的数据量非常大，或者数据表本身比较大，这种操作就会消耗系统过多的资源，操作成本非常高。

从 MySQL 8.x 开始支持`隐藏索引（invisible indexes）`，只需要将待删除的索引设置为隐藏索引，使查询优化器不再使用这个索引（即使使用 force index（强制使用索引），优化器也不会使用该索引），确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。这种通过先将索引设置为隐藏索引，再删除索引的方式就是`软删除`。

同时，如果想验证某个索引删除之后的查询性能影响，也可以暂时先隐藏该索引。

索引默认是可见的，在使用 CREATE TABLE，CREATE INDEX 或者 ALTER TABLE 等语句时，可以通过`VISIBLE`或者`INVISIBLE`关键词设置索引的可见性。

> 主键不能被设置为隐藏索引，当表中没有显式主键时，表中第一个唯一非空索引会成为隐式索引，也不能设置为隐藏索引。
>
> 当索引被隐藏时，它的内容仍然是和正常索引一样实时更新的。如果一个索引需要长期被隐藏，那么可以将其删除，因为索引的存在会影响插入、更新和删除的性能。

#### 创建表时直接创建

在 MySQL 中创建隐藏索引通过 SQL 语句`INVISIBLE`来实现，语法：

```mysql
 CREATE TABLE tablename(
    propname1 type1[CONSTRAINT1],
    propname2 type2[CONSTRAINT2],
    ......
    propnamen typen,
    INDEX [indexname](propname1 [(length)]) INVISIBLE
)
```

上述语句比普通索引多了一个关键字`INVISIBLE`，用来标记索引为不可见索引。

示例：

```mysql
CREATE TABLE book(
    book_id INT,
    book_name VARCHAR(100),
    authors VARCHAR (100),
    info VARCHAR (100),
    comment VARCHAR (100),
    year_publication YEAR,
    # 创建不可见的索引
    INDEX idx_cmt(comment) INVISIBLE
)
```

#### 在已经存在的表上创建

语法：

```mysql
CREATE INDEX indexname
ON tablename(propname[(length)]) INVISIBLE
```

示例：

```mysql
CREATE INDEX idx_year_pub ON book(year_publication) INVISIBLE
```

#### 通过 ALTER TABLE 语句创建

语法：

```mysql
ALTER TABLE tablename
ADD INDEX indexname (propname[(length)]) INVISIBLE
```

示例：

```mysql
ALTER TABLE book
ADD UNIQUE INDEX uk_idx_bname(book_name) INVISIBLE
```

#### 切换索引可见状态

已存在的索引可通过如下语句切换可见状态：

```mysql
# 切换成隐藏索引
ALTER TABLE tablename ALTER INDEX index_name INVISIBLE
# 切换成非隐藏索引
ALTER TABLE tablename ALTER INDEX index_name VISIBLE
```

示例：

```mysql
# 可见 ---> 不可见
ALTER TABLE book ALTER INDEX idx_year_pub INVISIBLE
# 不可见 ---> 可见
ALTER TABLE book ALTER INDEX idx_cmt VISIBLE
```

#### 使隐藏索引对查询优化器可见

在 MySQL 8.x 版本中，为索引提供了一种新的测试方式，可以通过查询优化器的一个开关 （`use_invisible_indexes`）来打开某个设置，使隐藏索引对查询优化器可见。如果 use_invisible_indexes 设置为 off（默认），优化器会忽略隐藏索引。如果设置为 on，即使隐藏索引不可见，优化器在生成执行计划时仍会考虑使用隐藏索引。

查看：

```mysql
mysql> SELECT @@optimizer_switch;
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| @@optimizer_switch                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=off,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

修改：

```mysql
mysql> SELECT @@optimizer_switch;
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| @@optimizer_switch                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on,index_condition_pushdown=on,mrr=on,mrr_cost_based=on,block_nested_loop=on,batched_key_access=off,materialization=on,semijoin=on,loosescan=on,firstmatch=on,duplicateweedout=on,subquery_materialization_cost_based=on,use_index_extensions=on,condition_fanout_filter=on,derived_merge=on,use_invisible_indexes=on,skip_scan=on,hash_join=on,subquery_to_derived=off,prefer_ordering_index=on,hypergraph_optimizer=off,derived_condition_pushdown=on |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

## 索引的设计原则

### 数据准备

创建数据库和表：

```mysql
mysql> CREATE DATABASE atguigudb1;
Query OK, 1 row affected (0.01 sec)

mysql> USE atguigudb1;
Database changed

mysql> CREATE TABLE `student_info` (
    -> `id` INT(11) NOT NULL AUTO_INCREMENT,
    -> `student_id` INT NOT NULL ,
    -> `name` VARCHAR(20) DEFAULT NULL,
    -> `course_id` INT NOT NULL ,
    -> `class_id` INT(11) DEFAULT NULL,
    -> `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    -> PRIMARY KEY (`id`)
    -> ) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected, 3 warnings (0.04 sec)

mysql> CREATE TABLE `course` (
    -> `id` INT(11) NOT NULL AUTO_INCREMENT,
    -> `course_id` INT NOT NULL ,
    -> `course_name` VARCHAR(40) DEFAULT NULL,
    -> PRIMARY KEY (`id`)
    -> ) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected, 2 warnings (0.03 sec)
```

创建模拟数据必需的存储函数：

```mysql
# 函数 1：创建随机产生字符串函数
mysql> DELIMITER //
mysql> CREATE FUNCTION rand_string(n INT)
    -> RETURNS VARCHAR(255) # 函数会返回一个字符串
    -> BEGIN
    -> DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFJHIJKLMNOPQRSTUVWXYZ';
    -> DECLARE return_str VARCHAR(255) DEFAULT '';
    -> DECLARE i INT DEFAULT 0;
    -> WHILE i < n DO
    -> SET return_str = CONCAT(return_str, SUBSTRING(chars_str, FLOOR(1 + RAND() * 52), 1));
    -> SET i = i + 1;
    -> END WHILE;
    -> RETURN return_str;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;

# 函数 2：创建随机数函数
mysql> DELIMITER //
mysql> CREATE FUNCTION rand_num(from_num INT, to_num INT) RETURNS INT(11)
    -> BEGIN
    -> DECLARE i INT DEFAULT 0;
    -> SET i = FLOOR(from_num + RAND() * (to_num - from_num + 1)) ;
    -> RETURN i;
    -> END //
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> DELIMITER ;
```

>主从复制，主机会将写操作记录在 bin-log 日志中。从机读取 bin-log 日志，执行语句来同步数据。如果使用函数来操作数据，会导致从机和主机操作时间不一致。所以，默认情况下，MySQL 不开启创建函数设置。
>
>查看 MySQL 是否允许创建函数：
>
>```mysql
>mysql> SHOW VARIABLES LIKE 'log_bin_trust_function_creators';
>+---------------------------------+-------+
>| Variable_name                   | Value |
>+---------------------------------+-------+
>| log_bin_trust_function_creators | OFF   |
>+---------------------------------+-------+
>1 row in set (0.01 sec)
>```
>
>命令开启，允许创建函数设置：
>
>```mysql
>mysql> SET GLOBAL log_bin_trust_function_creators=1;
>Query OK, 0 rows affected (0.00 sec)
>```
>
>如果不进行如上设置，创建函数时会报错：
>
>```mysql
>ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
>```

创建插入模拟数据的存储过程：

```mysql
# 存储过程 1：创建插入课程表的存储过程
mysql> DELIMITER //
mysql> CREATE PROCEDURE insert_course(max_num INT)
    -> BEGIN
    -> DECLARE i INT DEFAULT 0;
    -> SET autocommit = 0; # 设置手动提交事务
    -> REPEAT # 循环
    -> SET i = i + 1; # 赋值
    -> INSERT INTO course(course_id, course_name) VALUES (rand_num(10000, 10100), rand_string(6));
    -> UNTIL i = max_num
    -> END REPEAT;
    -> COMMIT; # 提交事务
    -> END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;

# 存储过程 2：创建插入学生信息表的存储过程
mysql> DELIMITER //
mysql> CREATE PROCEDURE insert_stu(max_num INT)
    -> BEGIN
    -> DECLARE i INT DEFAULT 0;
    -> SET autocommit = 0; # 设置手动提交事务
    -> REPEAT # 循环
    -> SET i = i + 1; # 赋值
    -> INSERT INTO student_info(course_id, class_id, student_id, name) VALUES (rand_num(10000, 10100), rand_num(10000, 10200), rand_num(1, 200000), rand_string(6));
    -> UNTIL i = max_num
    -> END REPEAT;
    -> COMMIT; # 提交事务
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
```

调用存储过程：

```mysql
mysql> CALL insert_course(100);
Query OK, 0 rows affected (0.02 sec)

mysql> CALL insert_stu(1000000);
Query OK, 0 rows affected (57.34 sec)
```

### 适合创建索引的情景

#### 字段的数值有唯一性的限制

索引本身可以起到约束的作用，比如唯一索引、主键索引都是可以起到唯一性约束的，因此在数据表中，如果`某个字段是唯一性的`，就可以直接创建唯一性索引，或者主键索引。这样，可以更快速的通过该索引来确定某条记录。

>业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。（来源：Alibaba）
>
>说明：不要以为唯一索引影响了 INSERT 速度，实际上这个速度损耗可以忽略，但提高查找速度是明显的。

#### 频繁作为 WHERE 查询条件的字段

某个字段`在 SELECT 语句的 WHERE 条件中经常被使用到`，那么就需要给这个字段创建索引了。尤其是在数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。

#### 经常 GROUP BY 和 ORDER BY 的列

索引其实就是让数据按照某种顺序进行存储或检索。当我们使用 GROUP BY 对数据进行分组查询，或者使用 ORDER BY 对数据进行排序的时候，如果`对分组或者排序的字段建立索引`，本身索引的数据就已经排好序了，进行分组查询和排序操作性能不是很 nice 吗？另外，如果待排序的列有多个，那么可以在这些列上建立`组合索引`。

- 如果仅仅使用 GROUP BY 或者 ORDER BY，且后面只有一个字段，则单独建立索引；如果后面跟多个字段，则建立组合索引。
- 如果既有 GROUP BY 又有 ORDER BY，那就建立联合索引，且 GROUP BY 的字段写在前面，ORDER BY 的字段写在后面。MySQL 8.0 后的版本也可以考虑使用降序索引。

#### UPDATE、DELETE 的 WHERE 条件列

`对数据按照某个条件进行查询后再进行 UPDATE 或 DELETE 的操作，如果对 WHERE 字段创建了索引，就能大幅提升效率。`原理是因为需要先根据 WHERE 条件列检索出来这条记录，然后再对它进行更新或删除。如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更新不需要对索引进行维护。

#### DISTINCT 字段需要创建索引

有时候需要对某个字段进行去重，使用`DISTINCT`，那么对这个字段创建索引，也会提升查询效率。这是因为索引会对数据按照某种顺序进行排序，所以在去重的时候会快很多。

#### 多表 JOIN 连接操作时，创建索引注意事项

首先，`连接表的数量尽量不要超过 3 张`，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增长会非常快（n, n^2, n^3, …），严重影响查询的效率。

其次，`对 WHERE 条件创建索引`，因为 WHERE 才是对数据条件的过滤。如果在数据量非常大的情况下，没有 WHERE 条件过滤是非常可怕的。

最后，`对用于连接的字段创建索引`，并且该字段在多张表中的`类型必须一致`。比如 course_id 在 student_info 表和 course 表中都为 int(11) 类型，而不能一个为 int 另一个为 varchar 类型。否则在查询时，虽然也会进行隐式的类型转换，转换时会使用函数，但会导致`索引失效`。

#### 使用列的类型小的创建索引

此处说的`类型大小`，指的就是该类型表示的数据范围的大小。

在定义表结构的时候，要显式的指定列的类型，以整数类型为例，有 TINYINT、MEDIUMINT、INT、BIGINT 等，它们占用的存储空间依次递增，能表示的整数范围当然也是依次递增。如果想要对某个整数列建立索引的话，在表示的整数范围允许的情况下，尽量让索引列使用较小的类型，比如能使用 INT 就不要使用 BIGINT，能使用 MEDIUMINT 就不要使用 INT。原因如下：

- 数据类型越小，在查询时进行的比较操作就越快；
- 数据类型越小，索引占用的存储空间就越小，在一个数据页内就可以放下更多的记录，从而减少磁盘 I/O 带来的性能损耗，也就意味着可以把更多的数据页缓存在内存中，从而加快读写效率。

这个建议对于表的主键来说更加适应，因为不仅是聚簇索引中会存储主键值，其他所有的二级索引的节点处，都会存储一份记录的主键值，如果主键使用更小的数据类型，也就意味着节省更多的存储空间和更高效的 I/O。

#### 使用字符串前缀创建索引

假设字符串很长，那存储一个字符串就需要占用很大的存储空间。在需要为这个字符串列建立索引时，就意味着在对应的 B+Tree 中有这样的两个问题：

- B+Tree 索引中的记录需要把该列的完整字符串存储起来，更费时，而且字符串越长，在索引中占用的存储空间越大。
- 如果 B+Tree 索引中索引列存储的字符串很长，那在作字符串比较时会占用更多的时间。

此时，可以通过截取字段的前面一部分内容建立索引，即`前缀索引`。这样，在查找记录时，虽然不能精确的定位到记录的位置，但是能定位到相应字符串前缀所在的位置，然后根据前缀相同的记录的主键值，回表查询完整的字符串值。即节约空间，又减少了字符串的比较时间，还大体能解决排序的问题。

例如，TEXT 和 BLOG 类型的字段，进行全文检索会很浪费时间，如果只检索字段前面的若干字符，可以提高检索速度。

示例：

```mysql
# 创建一张商户表
CREATE TABLE shop(address varchar(120) NOT NULL);

# 因为地址字段比较长，在地址字段上建立前缀索引
ALTER TABLE shop ADD INDEX(address(12));
```

问题是，前缀截取多少呢？截取的多了，达不到节省索引存储空间的目的，截取的少了，重复内容太多，字段的`散列度 (选择性)`会降低。 怎么计算不同的长度的选择性呢？

先看一下字段在全部数据中的选择度：

```mysql
SELECT COUNT(DISTINCT address) / COUNT(*) FROM shop
```

然后，通过不同长度去计算，与全表的选择性对比，公式：

```mysql
COUNT(DISTINCT LEFT(列名, 索引长度)) / COUNT(*)
```

例如：

```mysql
SELECT
COUNT(DISTINCT LEFT(address, 10)) / COUNT(*) AS sub10, -- 截取前 10 个字符的选择度 
COUNT(DISTINCT LEFT(address, 15)) / COUNT(*) AS sub11, -- 截取前 15 个字符的选择度 
COUNT(DISTINCT LEFT(address, 20)) / COUNT(*) AS sub12, -- 截取前 20 个字符的选择度 
COUNT(DISTINCT LEFT(address, 25)) / COUNT(*) AS sub13  -- 截取前 25 个字符的选择度
FROM shop
```

>拓展：Alibaba《Java开发手册》
>
>【 强制 】在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度。
>
>说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会高达 90% 以上，可以使用`COUNT(DISTINCT LEFT(列名, 索引长度)) / COUNT(*)`计算的区分度来确定。

**引申：索引列前缀对排序的影响。**

如果使用了索引列前缀，比方说前边只把 address 列的前 12 个字符放到了二级索引中，下边的查询会有问题：

```mysql
SELECT * FROM shop ORDER BY address LIMIT 12
```

因为二级索引中不包含完整的 address 列信息，所以无法对前 12 个字符相同，后边的字符不同的记录进行排序，也就是`使用索引列前缀的方式无法支持使用索引排序`，只能使用文件排序。

#### 区分度高（散列性高）的列适合作为索引

`列的基数`指的是某一个列中不重复数据的个数，比方说某个列包含值 2，5，8，2，5，8，2，5，8，虽然有 9 条记录，但该列的基数是 3。也就是说，在记录行数一定的情况下，列的基数越大，该列中的值越分散；列的基数越小，该列中的值越集中。这个列基数指标非常重要，直接影响是否能有效的利用索引。最好为列的基数大的列建立索引，为基数太小的列建立索引，效果可能不好。

可以使用公式`SELECT COUNT(DISTINCT a) / COUNT(*) FROM t1`计算区分度，越接近 1 越好，一般超过`33%`就算是比较高效的索引。

> 拓展：**联合索引中，应把区分度高（散列性高）的列，放在前面。**

#### 使用最频繁的列放到联合索引的左侧

这样也可以较少的建立一些索引。同时，由于`最左前缀原则`，可以增加联合索引的使用率。

#### 在多个字段都要创建索引的情况下，联合索引优于单值索引

原因：

- 索引建立的多，维护的成本也高。
- 多个字段进行联合查询时，其实只使用到一个索引。
- 在建立联合索引的相关字段做查询时，联合索引都能生效，使用频率比较高，足够优化 SQL 执行的速度了。

### 限制索引的数目

在实际工作中，也需要注意平衡，`索引的数目不是越多越好`。我们需要限制每张表上的索引数量，`建议单张表索引数量不超过 6 个`。原因：

- 每个索引都需要占用磁盘空间，索引越多，需要的磁盘空间就越大。


- 索引会影响 INSERT、DELETE、 UPDATE 等语句的性能，因为表中的数据更改的同时，索引也会进行调整和更新，会造成负担。


- 优化器在选择如何优化查询时，会根据统一信息，对每一个可以用到的索引来进行评估，以生成出一个最好的执行计划，如果同时有很多个索引都可以用于查询，会增加 MySQL 优化器生成执行计划的时间，降低查询性能。这是因为表中创建的索引过多，优化器在 possible_keys 中选择合适的 key 时需要的成本也会更多。比如下面查询中 possible_keys 有两个，实际使用的 key 只有一个，这个过程是优化器判断的。

  ```mysql
  mysql> EXPLAIN SELECT student_id, COUNT(*) AS num FROM student_info
      -> GROUP BY student_id
      -> ORDER BY create_time DESC
      -> LIMIT 100;
  +----+-------------+--------------+------------+-------+--------------------------+---------+---------+------+--------+----------+---------------------------------+
  | id | select_type | table        | partitions | type  | possible_keys            | key     | key_len | ref  | rows   | filtered | Extra                           |
  +----+-------------+--------------+------------+-------+--------------------------+---------+---------+------+--------+----------+---------------------------------+
  |  1 | SIMPLE      | student_info | NULL       | index | idx_sid,idx_cre_time_sid | idx_sid | 4       | NULL | 997449 |   100.00 | Using temporary; Using filesort |
  +----+-------------+--------------+------------+-------+--------------------------+---------+---------+------+--------+----------+---------------------------------+
  1 row in set, 1 warning (0.00 sec)
  ```

### 不适合创建索引的情景

#### 在 WHERE 中使用不到的字段，不要设置索引

WHERE 条件（包括 GROUP BY 和 ORDER BY）里用不到的字段，不需要创建索引，索引的价值是快速定位，如果起不到定位的字段，通常是不需要创建索引的。

#### 数据量小的表最好不要使用索引

如果表记录太少，比如少于 1000 个，那么是不需要创建索引的。表记录太少，是否创建索引对查询效率的影响并不大，甚至说，查询花费的时间，可能比遍历索引的时间还要短，索引可能不会产生优化效果。

#### 有大量重复数据的列上不要建立索引

在条件表达式中经常用到的不同值较多的列上建立索引，但字段中如果有大量重复数据，也不用创建索引。比如性别字段，往往会有大量重复字段，如果建立索引，不但不会提高查询效率，反而会严重降低数据更新速度。

> 索引的价值是快速定位，如果想要定位的数据有很多，那么索引就失去了它的价值，比如通常情况下的性别字段。**当数据重复度大，比如高于 10% 的时候，也就不需要对这个字段使用索引。**

#### 避免对经常更新的表创建过多的索引

第一层含义：频繁更新的字段不一定需要创建索引。因为更新数据的时候，也需要更新索引，如果索引太多，在更新索引的时候也会造成负担，从而影响效率。

第二层含义：避免对经常更新的表创建过多的索引，并且索引中的列尽可能少。此时，虽然提高了查询速度，同时却会降低更新表的速度。

#### 不建议用无序的值作为索引

例如身份证、UUID（在索引比较时需要转为 ASCⅡ，并且插入时可能造成页分裂）、MD5、HASH、无序长字符串等。

#### 删除不再使用或者很少使用的索引

表中的数据被大量更新，或者数据的使用方式被改变后，原有的一些索引可能不再需要。数据库管理员应当定期找出这些索引，将它们删除，从而减少索引对更新操作的影响。

#### 不要定义冗余或重复的索引

有时候无意或者有意的对同一个列创建了多个索引，比如：index(a, b, c) 相当于 index(a)、index(a, b)、index(a, b, c)。

冗余索引：

```mysql
CREATE TABLE person_info(
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL, 
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    PRIMARY KEY (id),
    KEY idx_name_birthday_phone_number (name(10), birthday, phone_number),
    KEY idx_name (name(10))
)
```

- 通过 idx_name_birthday_phone_number 索引就可以对 name 列进行快速搜索，再创建一 个专门针对 name 列的索引就算是一个 冗余索引，维护这个索引只会增加维护的成本，并不会对搜索有 什么好处。

重复索引：

```mysql
CREATE TABLE repeat_index_demo (
    col1 INT PRIMARY KEY,
    col2 INT,
    UNIQUE uk_idx_c1 (col1),
    INDEX idx_c1 (col1)
)
```

- col1 既是主键，又给它定义为一个唯一索引，还给它定义了一个普通索引，可是主键本身就会生成聚簇索引，所以定义的唯一索引和普通索引是重复的，这种情况要避免。

### 小结

索引是一把双刃剑，可以提高查询效率，但也会降低插入和更新的速度，并占用磁盘空间。

使用索引的最终目的是为了使查询的速度变快，上面给出的原则是最基本的准则，但是不能拘泥于上面的准则，在日常的学习和工作中需要进行不断的实践，根据应用的实际情况进行分析和判断，从而选择最合适的索引方式。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql-advanced.md