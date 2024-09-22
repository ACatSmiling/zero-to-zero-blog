>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-20`*

## 约束

### 常见的数据库对象

| 对象                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| 表（TABLE）           | 表是存储数据的逻辑单元，以行和列的形式存在，列就是字段，行就是记录 |
| 数据字典              | 即系统表，存放数据库相关信息的表。系统表的数据通常由数据库系统维护，程序员通常不应该修改，只可查看 |
| 约束（CONSTRAINT）    | 执行数据校验的规则，用于保证数据完整性的规则                 |
| 视图（VIEW）          | 一个或者多个数据表里的数据的逻辑显示，视图并不存储数据       |
| 索引（INDEX）         | 用于提高查询性能，相当于书的目录                             |
| 存储过程（PROCEDURE） | 用于完成一次完整的业务处理，没有返回值，但可通过传出参数将多个值传给调用环境 |
| 存储函数（FUNCTION）  | 用于完成一次特定的计算，具有一个返回值                       |
| 触发器（TRIGGER）     | 相当于一个事件监听器，当数据库发生特定事件后，触发器被触发，完成相应的处理 |

### 概述

#### 为什么需要约束

**数据完整性（Data Integrity）是指数据的精确性（Accuracy）和可靠性（Reliability）。**它是防止数据库中存在不符合语义规定的数据和防止因错误信息的输入输出造成无效操作或错误信息而提出的。

为了保证数据的完整性，SQL 规范以约束的方式对表数据进行额外的条件限制。从以下四个方面考虑：

- `实体完整性（Entity Integrity）`：例如，同一个表中，不能存在两条完全相同无法区分的记录。
- `域完整性（Domain Integrity）` ：例如：年龄范围 0~120，性别范围 "男/女"。
- `引用完整性（Referential Integrity）`：例如：员工所在部门，在部门表中要能找到这个部门。
- `用户自定义完整性（User-defined Integrity）` ：例如：用户名唯一、密码不能为空等，本部门经理的工资不得高于本部门职工的平均工资的 5 倍。

#### 什么是约束

`约束，constraint，是表级的强制规定。`可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后通过 ALTER TABLE 语句规定
约束。

#### 约束的分类

根据约束`数据列的限制`，约束可分为：

- `单列约束`：每个约束只约束一列。
- `多列约束`：每个约束可约束多列数据。

根据约束的`作用范围`，约束可分为：

![image-20230507123142168](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001600477-2045669104.png)

- `列级约束`：只能作用在一个列上，跟在列的定义后面。
- `表级约束`：可以作用在多个列上，不与列一起，而是单独定义。

根据约束`起的作用`，约束可分为：

- `NOT NULL`：非空约束，规定某个字段不能为空。
- `UNIQUE`：唯一约束，规定某个字段在整个表中是唯一的。
- `PRIMARY KEY`：主键（非空且唯一）约束。
- `FOREIGN KEY`：外键约束。
- `CHECK`：检查约束。**MySQL 不支持 CHECK 约束**，但可以使用 CHECK 约束，只是没有任何效果。
- `DEFAULT`：默认值约束。

查看某个表已有的约束：

```mysql
# information_schema 数据库名（系统库）
# table_constraints 表名称（专门存储各个表的约束）
SELECT * FROM information_schema.table_constraints WHERE table_name = '表名称';
```

```mysql
mysql> SELECT * FROM information_schema.table_constraints WHERE table_name = 'employees';
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME | TABLE_SCHEMA | TABLE_NAME | CONSTRAINT_TYPE | ENFORCED |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
| def                | atguigudb         | emp_email_uk    | atguigudb    | employees  | UNIQUE          | YES      |
| def                | atguigudb         | emp_emp_id_pk   | atguigudb    | employees  | UNIQUE          | YES      |
| def                | atguigudb         | PRIMARY         | atguigudb    | employees  | PRIMARY KEY     | YES      |
| def                | atguigudb         | emp_dept_fk     | atguigudb    | employees  | FOREIGN KEY     | YES      |
| def                | atguigudb         | emp_job_fk      | atguigudb    | employees  | FOREIGN KEY     | YES      |
| def                | atguigudb         | emp_manager_fk  | atguigudb    | employees  | FOREIGN KEY     | YES      |
+--------------------+-------------------+-----------------+--------------+------------+-----------------+----------+
6 rows in set (0.00 sec)
```

### 非空约束

作用：限定某个字段/某列的值，不允许为空。

关键字：`NOT NULL`。

特点：

- 默认，所有的类型的值都可以是 NULL，包括 INT、FLOAT 等数据类型。
- 非空约束只能出现在表对象的列上，只能某个列单独限定非空，不能组合非空。
- 一个表可以有很多列都分别限定了非空。
- 空字符串 '' 不等于 NULL，0 也不等于 NULL。

添加非空约束：

```mysql
# 建表时
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型 NOT NULL,
    字段名 数据类型 NOT NULL
);

# 建表后
ALTER TABLE 表名称 MODIFY 字段名 数据类型 NOT NULL;
```

删除非空约束：

```mysql
# 去掉 NOT NULL，相当于修改某个非注解字段，该字段允许为空
ALTER TABLE 表名称 MODIFY 字段名 数据类型 NULL;

# 去掉 not null，相当于修改某个非注解字段，该字段允许为空
ALTER TABLE 表名称 MODIFY 字段名 数据类型;
```

### 唯一性约束

作用：用来限制某个字段/某列的值不能重复。

关键字：`UNIQUE`。

特点：

- 同一个表可以有多个唯一约束。
- 唯一约束可以是某一个列的值唯一，也可以多个列组合的值唯一。
- 唯一性约束允许列值为空，也允许出现多个 NULL 值。
- 在创建唯一约束的时候，如果不给唯一约束命名，就默认和列名相同。
- `MySQL 会给唯一约束的列上默认创建一个唯一索引。`

添加唯一约束：

```mysql
# 建表时
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型 UNIQUE,
    字段名 数据类型 UNIQUE KEY,
    字段名 数据类型
);

# 字段列表中如果是一个字段，表示该列的值唯一，如果是两个或更多个字段，那么复合唯一，即多个字段的组合是唯一的
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型,
    字段名 数据类型,
    [CONSTRAINT 约束名] UNIQUE KEY(字段名)
);

# 建表后
# 方式一
ALTER TABLE 表名称 ADD UNIQUE KEY(字段列表);

# 方式二
ALTER TABLE 表名称 MODIFY 字段名 字段类型 UNIQUE;
```

示例：

```mysql
mysql> CREATE TABLE USER(
    ->     id INT NOT NULL,
    ->     NAME VARCHAR(25),
    ->     PASSWORD VARCHAR(16),
    ->     -- 使用表级约束语法，表示用户名和密码组合不能重复
    ->     CONSTRAINT uk_name_pwd UNIQUE(NAME,PASSWORD)
    -> );
Query OK, 0 rows affected (0.05 sec)
```

删除唯一约束：

- 添加唯一性约束的列上也会自动创建唯一索引。
- `删除唯一约束只能通过删除唯一索引的方式删除。`
- 删除时需要指定唯一索引名，唯一索引名就和唯一约束名一样。
- 如果创建唯一约束时未指定名称，如果是单列，就默认和列名相同；如果是组合列，那么默认和 () 中排在第一个的列名相同。也可以自定义唯一性约束名。

```mysql
# 查看都有哪些约束
SELECT * FROM information_schema.table_constraints WHERE table_name = 表名; 

# 删除约束
ALTER TABLE 表名 DROP INDEX 约束名;
```

**关于复合唯一性约束：**

```mysql
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型,
    字段名 数据类型,
    UNIQUE KEY(字段列表) # 字段列表中写的是多个字段名，多个字段名用逗号分隔，表示复合唯一约束，即多个字段的组合是唯一的
);
```

### 主键约束

作用：用来唯一标识表中的一行记录。

关键字：`PRIMARY KEY`。

特点：

- `主键约束相当于唯一约束 + 非空约束的组合，主键约束列不允许重复，也不允许出现空值。`
- 一个表最多只能有一个主键约束，建立主键约束可以在列级别创建，也可以在表级别上创建。
- 主键约束对应着表中的一列或者多列（复合主键）。
- 如果是多列组合的复合主键约束，那么这些列都不允许为空值，并且组合的值不允许重复。`MySQL 的主键名总是 PRIMARY`，就算自己命名了主键约束名也没用。
- `当创建主键约束时，系统默认会在所在的列或列组合上建立对应的主键索引（能够根据主键查询的，就根据主键查询，效率更高）。如果删除主键约束了，主键约束对应的索引就自动删除了。`
- 需要注意的一点是，不要修改主键字段的值。因为主键是数据记录的唯一标识，如果修改了主键的值，就有可能会破坏数据的完整性。

添加主键约束：

```mysql
# 建表时指定主键约束
CREATE TABLE 表名称(
    字段名 数据类型 PRIMARY KEY, # 列级模式
    字段名 数据类型,
    字段名 数据类型
);
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型,
    字段名 数据类型,
    [CONSTRAINT 约束名] PRIMARY KEY(字段名) # 表级模式
);

# 建表后增加主键约束
ALTER TABLE 表名称 ADD PRIMARY KEY(字段列表); # 字段列表可以是一个字段，也可以是多个字段，如果是多个字段的话，是复合主键
```

删除主键约束：

```mysql
ALTER TABLE 表名称 DROP PRIMARY KEY;
```

>说明：删除主键约束，不需要指定主键名，因为一个表只有一个主键，删除主键约束后，非空还存在。

**关于复合主键约束：**

```mysql
CREATE TABLE 表名称(
    字段名 数据类型,
    字段名 数据类型,
    字段名 数据类型,
	PRIMARY KEY(字段名1,字段名2) # 表示字段 1 和字段 2 的组合是唯一的，也可以有更多个字段
);
```

### 外键约束

作用：限定某个表的某个字段的引用完整性。

关键字：`FOREIGN KEY`。

- 主表（父表）：被引用的表，被参考的表。
- 从表（子表）：引用别人的表，参考别人的表。

特点：

- **从表的外键列，必须引用/参考主表的主键或唯一约束的列，因为被依赖/被参考的值必须是唯一的。**
- 在创建外键约束时，如果不给外键约束命名，默认名不是列名，而是自动产生一个外键名（例如 student_ibfk_1），也可以指定外键约束名。
- 若创建表时就指定外键约束，需先创建主表，再创建从表。
- 删表时，先删从表（或先删除外键约束），再删除主表。
- 当主表的记录被从表参照时，主表的记录将不允许删除，如果要删除数据，需要先删除从表中依赖该记录的数据，然后才可以删除主表的数据。
- 在从表中指定外键约束，一个表可以建立多个外键约束。
- 从表的外键列与主表被参照的列名字可以不相同，但是数据类型必须一样，逻辑意义一致。如果类型不一样，创建子表时，会出现错误。
- **当创建外键约束时，系统默认会在所在的列上建立对应的普通索引，索引名是外键的约束名。（根据外键查询效率很高）**
- **删除外键约束后，必须手动删除对应的索引。**

添加外键约束：

```mysql
# 建表时
CREATE TABLE 主表名称(
    字段1 数据类型 PRIMARY KEY,
    字段2 数据类型
);

CREATE TABLE 从表名称(
字段1 数据类型 PRIMARY KEY,
字段2 数据类型,
[CONSTRAINT <外键约束名称>] FOREIGN KEY (从表的某个字段) REFERENCES 主表名 (被参考字段)
);
# （从表的某个字段）的数据类型必须与主表名（被参考字段）的数据类型一致，逻辑意义也一样
# （从表的某个字段）的字段名可以与主表名（被参考字段）的字段名一样，也可以不一样
-- FOREIGN KEY：在表级指定子表中的列
-- REFERENCES：标示在父表中的列

# 建表后
ALTER TABLE 从表名 ADD [CONSTRAINT 约束名] FOREIGN KEY (从表的字段) REFERENCES 主表名 (被引用字段) [ON UPDATE 约束等级][ON DELETE 约束等级];
```

>一般情况下，表与表的关联都是提前设计好了的，因此，会在创建表的时候就把外键约束定义好。不过，如果需要修改表的设计（比如添加新的字段，增加新的关联关系），但没有预先定义外键约束，那么，就要用修改表的方式来补充定义。

外键约束关系是针对主表和从表双方的：

- 添加了外键约束后，主表的修改和删除数据受约束。
- 添加了外键约束后，从表的添加和修改数据受约束。
- 在从表上建立外键，要求主表必须存在。
- 删除主表时，要求从表从表先删除，或将从表中外键引用该主表的关系先删除。

>在 MySQL 里，外键约束是有成本的，需要消耗系统资源。对于大并发的 SQL 操作，有可能会不适合。比如大型网站的中央数据库，可能会 因为外键约束的系统开销而变得非常慢 。所以，MySQL 允许不使用系统自带的外键约束，应在应用层面完成检查数据一致性的逻辑。也就是说，即使不用外键约束，也要想办法通过应用层面的附加逻辑，来实现外键约束的功能，确保数据的一致性。

`约束等级`：

- `Cascade`方式 ：在父表上 UPDATE/DELETE 记录时，同步 UPDATE/DELETE 子表的匹配记录。
- `Set null`方式 ：在父表上 UPDATE/DELETE 记录时，将子表上匹配记录的列设为 NULL，但是要注意子表的外键列不能为 NULL。
- `No action`方式 ：如果子表中有匹配的记录，则不允许对父表对应候选键进行 UPDATE/DELETE 操作。
- `Restrict`方式 ：同 No action， 都是立即检查外键约束。
- `Set default`方式 （在可视化工具 SQLyog 中可能显示空白）：父表有变更时，子表将外键列设置成一个默认的值，但 Innodb 不能识别。
- 如果没有指定等级，就相当于 Restrict 方式。
- 对于外键约束，最好是采用`ON UPDATE CASCADE ON DELETE RESTRICT`的方式。

删除外键约束：

```mysql
# 第一步，先查看约束名和删除外键约束
SELECT * FROM information_schema.table_constraints WHERE table_name = 表名称;# 查看某个表的约束名

ALTER TABLE 从表名 DROP FOREIGN KEY 外键约束名;

# 第二步，查看索引名和删除索引，注意，只能手动删除
SHOW INDEX FROM 表名称; # 查看某个表的索引名

ALTER TABLE 从表名 DROP INDEX 索引名;
```

> 阿里开发规范：
>
> **【 强制 】不得使用外键与级联，一切外键概念必须在应用层解决。**
>
> 说明：（概念解释）学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新，即为`级联更新`。**外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险；外键影响数据库的插入速度。**

### 检查约束

作用：检查某个字段的值是否符号某种要求，一般指的是值的范围。

关键字：`CHECK`。

>MySQL 5.7 可以使用 CHECK 约束，但 CHECK 约束对数据验证没有任何作用，添加数据时，没有任何错误或警告，MySQL 8.0 中可以使用 CHECK 约束。

### 默认约束

作用：给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默认值。

关键字：`DEFAULT`。

添加默认约束：

```mysql
# 建表时
CREATE TABLE 表名称(
    字段名 数据类型 PRIMARY KEY,
    字段名 数据类型 UNIQUE KEY NOT NULL,
    字段名 数据类型 UNIQUE KEY,
    字段名 数据类型 NOT NULL DEFAULT 默认值
);

CREATE TABLE 表名称(
    字段名 数据类型 DEFAULT 默认值 ,
    字段名 数据类型 NOT NULL DEFAULT 默认值,
    字段名 数据类型 NOT NULL DEFAULT 默认值,
    PRIMARY KEY(字段名),
    UNIQUE KEY(字段名)
);

# 建表后
ALTER TABLE 表名称 MODIFY 字段名 数据类型 DEFAULT 默认值;

# 如果这个字段原来有非空约束，那么在加默认值约束时，还得保留非空约束，否则非空约束就被删除了
# 同理，在给某个字段加非空约束也一样，如果这个字段原来有默认值约束，你想保留，也要在 modify 语句中保留默认值约束，否则就删除了
ALTER TABLE 表名称 MODIFY 字段名 数据类型 DEFAULT 默认值 NOT NULL;
```

>`说明：默认值约束一般不在唯一键和主键列上加。`

删除默认值约束：

```mysql
# 删除默认值约束，也不保留非空约束
ALTER TABLE 表名称 MODIFY 字段名 数据类型;

# 删除默认值约束，保留非空约束
ALTER TABLE 表名称 MODIFY 字段名 数据类型 NOT NULL; 
```

### 自增约束

作用：设置某个字段的值自增。

关键字：`AUTO_INCREMENT`。

特点：

- `一个表最多只能有一个自增长列。`
- 当需要产生唯一标识符或顺序值时，可设置自增长。
- 自增长列约束的列必须是键列（主键列，唯一键列）。
- 自增约束的列的数据类型必须是整数类型。
- 如果自增列指定了 0 和 NULL，会在当前最大值的基础上自增；如果自增列手动指定了具体值，直接赋值为具体值。

添加自增约束：

```mysql
# 建表时
CREATE TABLE 表名称(
    字段名 数据类型 PRIMARY KEY AUTO_INCREMENT,
    字段名 数据类型 UNIQUE KEY NOT NULL,
    字段名 数据类型 UNIQUE KEY,
    字段名 数据类型 NOT NULL DEFAULT 默认值
);

CREATE TABLE 表名称(
    字段名 数据类型 DEFAULT 默认值 ,
    字段名 数据类型 UNIQUE KEY AUTO_INCREMENT,
    字段名 数据类型 NOT NULL DEFAULT 默认值,
    PRIMARY KEY(字段名)
);

# 建表后
ALTER TABLE 表名称 MODIFY 字段名 数据类型 AUTO_INCREMENT;
```

删除自增约束：

```mysql
# ALTER TABLE 表名称 MODIFY 字段名 数据类型 AUTO_INCREMENT; # 给这个字段增加自增约束
ALTER TABLE 表名称 MODIFY 字段名 数据类型; # 去掉 AUTO_INCREMENT，相当于删除
```

>MySQL 8.0 新特性 - 自增变量的持久化：
>
>在 MySQL 8.0 之前，自增主键 AUTO_INCREMENT 的值如果大于 max(primary key) + 1，此时，如果删除最大的数据，然后重庆 MySQL 后，会重置 AUTO_INCREMENT=max(primary key)+1，再次新增数据时，主键会等于重启前被删除的数据的主键值，这种现象在某些情况下会导致业务主键冲突或者其他难以发现的问题。
>
>在 MySQL 5.7 系统中，对于自增主键的分配规则，是由 InnoDB 数据字典内部一个`计数器`来决定的，而该计数器只在`内存中维护`，并不会持久化到磁盘中，当数据库重启时，该计数器会被初始化。
>
>MySQL 8.0 将自增主键的计数器持久化到`重做日志`中。每次计数器发生改变，都会将其写入重做日志中。如果数据库重启，InnoDB 会根据重做日志中的信息来初始化计数器的内存值。

## 视图

### 概述

![image-20230509233813667](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001600011-254153697.png)

视图，VIEW，一方面可以帮我们使用表的一部分而不是所有的表，另一方面也可以针对不同的用户制定不同的查询视图。比如，针对一个公司的销售人员，我们只想给他看部分数据，而某些特殊的数据，比如采购的价格，则不会提供给他。再比如，人员薪酬是个敏感的字段，那么只给某个级别以上的人员开放，其他人的查询视图中则不提供这个字段。

### 视图的理解

![image-20230509234031123](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001559596-1500880909.png)

- 视图是一种`虚拟表`，本身是`不具有数据`的，占用很少的内存空间，它是 SQL 中的一个重要概念。

- 视图建立在已有表的基础上，视图赖以建立的这些表称为`基表`。
- **视图的创建和删除只影响视图本身，不影响对应的基表。但是当对视图中的数据进行增加、删除和修改操作时，数据表中的数据会相应地发生变化，反之亦然。**
  - 在数据库中，视图不会保存数据，数据真正保存在数据表中。当对视图中的数据进行增加、删除和修改操作时，数据表中的数据会相应地发生变化；反之亦然。
- 向视图提供数据内容的语句为 SELECT 语句，可以将视图理解为存储起来的 SELECT 语句。
- 视图，是向用户提供基表数据的另一种表现形式。通常情况下，小型项目的数据库可以不使用视图，但是在大型项目中，以及数据表比较复杂的情况下，视图的价值就凸显出来了，它可以帮助我们把经常查询的结果集放到虚拟表中，提升使用效率。

### 创建视图

在 CREATE VIEW 语句中嵌入子查询：

```mysql
CREATE [OR REPLACE]
[ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW 视图名称 [(字段列表)]
AS 查询语句
[WITH [CASCADED|LOCAL] CHECK OPTION];

# 简化
CREATE VIEW 视图名称
AS 查询语句;
```

#### 创建单表视图

```mysql
mysql> CREATE VIEW empvu80
    -> AS
    -> SELECT employee_id, last_name, salary
    -> FROM employees
    -> WHERE department_id = 80;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM empvu80;
+-------------+------------+----------+
| employee_id | last_name  | salary   |
+-------------+------------+----------+
|         145 | Russell    | 14000.00 |
|         146 | Partners   | 13500.00 |
|         147 | Errazuriz  | 12000.00 |
|         148 | Cambrault  | 11000.00 |
|         149 | Zlotkey    | 10500.00 |
|         150 | Tucker     | 10000.00 |
|         151 | Bernstein  |  9500.00 |
|         152 | Hall       |  9000.00 |
|         176 | Taylor     |  8600.00 |
|         177 | Livingston |  8400.00 |
|         179 | Johnson    |  6200.00 |
+-------------+------------+----------+
34 rows in set (0.00 sec)
```

```mysql
mysql> CREATE VIEW emp_year_salary (ename, year_salary)
    -> AS
    -> SELECT last_name, salary * 12 * (1 + IFNULL(commission_pct, 0))
    -> FROM employees;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM emp_year_salary;
+-------------+-------------+
| ename       | year_salary |
+-------------+-------------+
| King        |   288000.00 |
| Kochhar     |   204000.00 |
| De Haan     |   204000.00 |
| Hunold      |   108000.00 |
| Ernst       |    72000.00 |
| Hartstein   |   156000.00 |
| Fay         |    72000.00 |
| Mavris      |    78000.00 |
| Baer        |   120000.00 |
| Higgins     |   144000.00 |
| Gietz       |    99600.00 |
+-------------+-------------+
107 rows in set (0.00 sec)
    
mysql> CREATE VIEW salvu50
    -> AS
    -> SELECT employee_id ID_NUMBER, last_name NAME, salary * 12 ANN_SALARY
    -> FROM employees
    -> WHERE department_id = 50;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT * FROM salvu50;
+-----------+-------------+------------+
| ID_NUMBER | NAME        | ANN_SALARY |
+-----------+-------------+------------+
|       120 | Weiss       |   96000.00 |
|       121 | Fripp       |   98400.00 |
|       122 | Kaufling    |   94800.00 |
|       123 | Vollman     |   78000.00 |
|       124 | Mourgos     |   69600.00 |
|       125 | Nayer       |   38400.00 |
|       126 | Mikkilineni |   32400.00 |
|       193 | Everett     |   46800.00 |
|       194 | McCain      |   38400.00 |
|       195 | Jones       |   33600.00 |
|       196 | Walsh       |   37200.00 |
|       197 | Feeney      |   36000.00 |
|       198 | OConnell    |   31200.00 |
|       199 | Grant       |   31200.00 |
+-----------+-------------+------------+
45 rows in set (0.00 sec)
```

>说明 1：实际上就是在 SQL 查询语句的基础上封装了视图 VIEW，这样就会基于 SQL 语句的结果集形成一张虚拟表。
>
>说明 2：**在创建视图时，没有在视图名后面指定字段列表，则视图中字段列表默认和 SELECT 语句中的字段列表一致。如果 SELECT 语句中给字段取了别名，那么视图中的字段名和别名相同。**

#### 创建多表联合视图

```mysql
mysql> CREATE VIEW empview
    -> AS
    -> SELECT e.employee_id emp_id, e.last_name, d.department_name
    -> FROM employees e, departments d
    -> WHERE e.department_id = d.department_id;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM empview;
+--------+-------------+------------------+
| emp_id | last_name   | department_name  |
+--------+-------------+------------------+
|    200 | Whalen      | Administration   |
|    201 | Hartstein   | Marketing        |
|    202 | Fay         | Marketing        |
|    114 | Raphaely    | Purchasing       |
|    160 | Doran       | Sales            |
|    161 | Sewall      | Sales            |
|    113 | Popp        | Finance          |
|    205 | Higgins     | Accounting       |
|    206 | Gietz       | Accounting       |
+--------+-------------+------------------+
106 rows in set (0.00 sec)

# 左外连接，比上面的内连接数据多了一条
mysql> CREATE VIEW empview2
    -> AS
    -> SELECT e.employee_id emp_id, e.last_name, d.department_name
    -> FROM employees e LEFT JOIN departments d
    -> ON e.department_id = d.department_id;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM empview2;
+--------+-------------+------------------+
| emp_id | last_name   | department_name  |
+--------+-------------+------------------+
|    100 | King        | Executive        |
|    101 | Kochhar     | Executive        |
|    102 | De Haan     | Executive        |
|    164 | Marvins     | Sales            |
|    172 | Bates       | Sales            |
|    173 | Kumar       | Sales            |
|    174 | Abel        | Sales            |
|    175 | Hutton      | Sales            |
|    176 | Taylor      | Sales            |
|    177 | Livingston  | Sales            |
|    178 | Grant       | NULL             |
|    204 | Baer        | Public Relations |
|    205 | Higgins     | Accounting       |
|    206 | Gietz       | Accounting       |
+--------+-------------+------------------+
107 rows in set (0.00 sec)

mysql> CREATE VIEW emp_dept
	-> AS
	-> SELECT e.last_name, e.department_name
	-> FROM employees e LEFT JOIN departments d
	-> ON e.department_id = d.department_id;
Query OK, 0 rows affected (0.02 sec)

mysql> SELECT * FROM emp_dept;
+-------------+------------------+
| last_name   | department_name  |
+-------------+------------------+
| King        | Executive        |
| Kochhar     | Executive        |
| De Haan     | Executive        |
| Hunold      | IT               |
| Livingston  | Sales            |
| Grant       | NULL             |
| Johnson     | Sales            |
| Taylor      | Shipping         |
| Mavris      | Human Resources  |
| Baer        | Public Relations |
| Higgins     | Accounting       |
| Gietz       | Accounting       |
+-------------+------------------+
107 rows in set (0.00 sec)

mysql> CREATE VIEW dept_sum_vu(name, minsal, maxsal, avgsal)
    -> AS
    -> SELECT d.department_name, MIN(e.salary), MAX(e.salary), AVG(e.salary)
    -> FROM employees e, departments d
    -> WHERE e.department_id = d.department_id
    -> GROUP BY d.department_name;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM dept_sum_vu;
+------------------+----------+----------+--------------+
| name             | minsal   | maxsal   | avgsal       |
+------------------+----------+----------+--------------+
| Executive        | 17000.00 | 24000.00 | 19333.333333 |
| IT               |  4200.00 |  9000.00 |  5760.000000 |
| Finance          |  6900.00 | 12000.00 |  8600.000000 |
| Purchasing       |  2500.00 | 11000.00 |  4150.000000 |
| Shipping         |  2100.00 |  8200.00 |  3475.555556 |
| Sales            |  6100.00 | 14000.00 |  8955.882353 |
| Administration   |  4400.00 |  4400.00 |  4400.000000 |
| Marketing        |  6000.00 | 13000.00 |  9500.000000 |
| Human Resources  |  6500.00 |  6500.00 |  6500.000000 |
| Public Relations | 10000.00 | 10000.00 | 10000.000000 |
| Accounting       |  8300.00 | 12000.00 | 10150.000000 |
+------------------+----------+----------+--------------+
11 rows in set (0.00 sec)
```

#### 基于视图创建视图

当创建好一张视图之后，还可以在它的基础上继续创建视图。

举例：联合 emp_dept 视图和 emp_year_salary 视图查询员工姓名、部门名称、年薪信息，创建 emp_dept_ysalary 视图。

```mysql
mysql> CREATE VIEW emp_dept_ysalary
    -> AS
    -> SELECT emp_dept.last_name, emp_dept.department_name, emp_year_salary.year_salary
    -> FROM emp_dept INNER JOIN emp_year_salary
    -> ON emp_dept.last_name = emp_year_salary.last_name;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM emp_dept_ysalary;
+-------------+------------------+-------------+
| last_name   | department_name  | year_salary |
+-------------+------------------+-------------+
| King        | Sales            |   288000.00 |
| King        | Executive        |   288000.00 |
| Kochhar     | Executive        |   204000.00 |
| De Haan     | Executive        |   204000.00 |
| Hunold      | IT               |   108000.00 |
| Ernst       | IT               |    72000.00 |
| Grant       | NULL             |    31200.00 |
| Whalen      | Administration   |    52800.00 |
| Hartstein   | Marketing        |   156000.00 |
| Fay         | Marketing        |    72000.00 |
| Mavris      | Human Resources  |    78000.00 |
| Baer        | Public Relations |   120000.00 |
| Higgins     | Accounting       |   144000.00 |
| Gietz       | Accounting       |    99600.00 |
+-------------+------------------+-------------+
117 rows in set (0.01 sec)
```

#### 利用视图对数据进行格式化

开发中，经常需要输出某个格式的内容，比如想输出员工姓名和对应的部门名，对应格式为 emp_name(department_name)，就可以使用视图来完成数据格式化的操作：

```mysql
mysql> CREATE VIEW emp_depart
	-> AS
	-> SELECT CONCAT(e.last_name, '(', d.department_name, ')') AS emp_dept
	-> FROM employees e JOIN departments d
	-> WHERE e.department_id = d.department_id;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM emp_depart;
+-------------------------+
| emp_dept                |
+-------------------------+
| Whalen(Administration)  |
| Hartstein(Marketing)    |
| Fay(Marketing)          |
| Raphaely(Purchasing)    |
| Khoo(Purchasing)        |
| Urman(Finance)          |
| Popp(Finance)           |
| Higgins(Accounting)     |
| Gietz(Accounting)       |
+-------------------------+
106 rows in set (0.00 sec)
```

#### 示例

不同库的数据连表视图：

```mysql
-- 作业票视图

-- 对应字段：
    -- 企业标识, 作业票id, 作业任务id, 作业票编号, 作业名称, 作业内容, 作业具体地点, 作业位置名称, 作业单位名称, 属地单位, 作业类型, 作业等级, 作业状态, 实际开始时间, 实际结束时间, 作业票票样地址, 是否关联JSA, 数据来源(0统建, 1自建)

DROP VIEW IF EXISTS enterprise_self.view_enterprise_self_work_ticket;
CREATE VIEW enterprise_self.view_enterprise_self_work_ticket
(company_identity, work_ticket_id, work_task_id, work_ticket_num, work_name, work_content, work_site, work_position_name, work_unit_name, territorial_unit_name, work_type, work_level, work_status, actual_start_time, actual_end_time, ticket_url, if_jsa, source_type)
AS
(
    /*
	 * 统建系统
	 */
    SELECT ticket.tenant, ticket.id, ticket.work_task, ticket.num, ticket.work_name, ticket.work_content, ticket.work_site, ticket.work_position_name, ticket.work_unit_name, ticket.territorial_unit_name, ticket.work_type_code, ticket.work_level, ticket.work_status, ticket.actual_start_time, ticket.actual_end_time, ticket.ticket_url, (CASE WHEN (task.safety_analysis != 0) THEN '关联' ELSE '未关联' END) AS if_jsa, 0 FROM hse_alratk.work_ticket ticket LEFT JOIN hse_alratk.work_task task ON ticket.work_task = task.id WHERE ticket.is_deleted = 0 AND ticket.work_status IN (1, 2, 3, 4)
	/*
	 * 自建系统
	 */
    UNION ALL
    SELECT ticket.credit_code, ticket.work_ticket_id, ticket.work_task_id, ticket.work_ticket_code, ticket.work_name, ticket.work_content, '', ticket.work_position_name, ticket.work_unit_name, ticket.department_name, (CASE ticket.work_type_code WHEN 'blinding_pipeline_operation' THEN 'blinding-pipeline_operation' ELSE work_type_code END) AS work_type_code, (CASE ticket.work_level_code WHEN 'super' THEN '特级' WHEN 'first' THEN '一级' WHEN 'second' THEN '二级' WHEN 'third' THEN '三级' WHEN 'forth' THEN '四级' ELSE '-') AS work_level_code, 3, ticket.plan_start_time, ticket.plan_end_time, ticket.work_ticket_file_url, (CASE WHEN (task.jsa_id != '' AND task.jsa_id IS NOT NULL) THEN '关联' ELSE '未关联' END) AS if_jsa, 1 FROM data_sync.work_ticket ticket LEFT JOIN data_sync.work_task task ON ticket.work_task_id = task.work_task_id WHERE ticket.deleted = 0
);
```

对应 ddl：

```mysql
-- 作业票视图

-- 对应字段：
    -- 企业标识, 作业票id, 作业任务id, 作业票编号, 作业名称, 作业内容, 作业具体地点, 作业位置名称, 作业单位名称, 属地单位, 作业类型, 作业等级, 作业状态, 计划开始时间, 计划结束时间, 实际开始时间, 实际结束时间, 作业票票样地址, 是否关联安全分析, 位置点所属划分区域id, 坐标, 位置名称, 数据上报时间(自建系统), 数据来源(0统建, 1自建)

-- enterprise_data_lake2.view_enterprise_self_work_ticket source
DROP VIEW IF EXISTS enterprise_data_lake2.view_enterprise_self_work_ticket;
CREATE OR REPLACE
ALGORITHM = UNDEFINED VIEW `enterprise_data_lake2`.`view_enterprise_self_work_ticket`
AS
-- 统建系统
SELECT
    `ticket`.`tenant` AS `company_identity`,
    `ticket`.`id` AS `work_ticket_id`,
    `ticket`.`work_task` AS `work_task_id`,
    `ticket`.`num` AS `work_ticket_num`,
    `ticket`.`work_name` AS `work_name`,
    `ticket`.`work_content` AS `work_content`,
    `ticket`.`work_site` AS `work_site`,
    `ticket`.`work_position_name` AS `work_position_name`,
    `ticket`.`work_unit_name` AS `work_unit_name`,
    `ticket`.`territorial_unit_name` AS `territorial_unit_name`,
    `ticket`.`work_type_code` AS `work_type`,
    `ticket`.`work_level` AS `work_level`,
    `ticket`.`work_status` AS `work_status`,
    `ticket`.`plan_start_time` AS `plan_start_time`,
    `ticket`.`plan_end_time` AS `plan_end_time`,
    `ticket`.`actual_start_time` AS `actual_start_time`,
    `ticket`.`actual_end_time` AS `actual_end_time`,
    `ticket`.`ticket_url` AS `ticket_url`,
    (CASE
        WHEN (`task`.`safety_analysis` <> 0) THEN '关联'
        ELSE '未关联'
    END) AS `if_safety_analysis`,
    `position`.`region_id` AS `region_id`,
    `position`.`coordinate` AS `coordinate`,
    `position`.`position_name` AS `position_name`,
    NULL AS `report_create_date`,
    0 AS `source_type`
FROM
(`enterprise_data_lake2`.`work_ticket` `ticket`
    LEFT JOIN `enterprise_data_lake2`.`work_task` `task` ON ((`ticket`.`work_task` = `task`.`id`) AND (`ticket`.`tenant` = `task`.`tenant`))
    INNER JOIN `enterprise_data_lake2`.`mm_position` `position` ON ((`ticket`.`work_position` = `position`.`id`) AND (`ticket`.`tenant` = `position`.`tenant`))
)
WHERE
((`ticket`.`is_deleted` = 0) AND (`ticket`.`work_status` IN (1, 2, 3, 4)))
UNION ALL
-- 自建系统
SELECT
    `ticket`.`credit_code` AS `company_identity`,
    `ticket`.`work_ticket_id` AS `work_ticket_id`,
    `ticket`.`work_task_id` AS `work_task_id`,
    `ticket`.`work_ticket_code` AS `work_ticket_num`,
    `ticket`.`work_name` AS `work_name`,
    `ticket`.`work_content` AS `work_content`,
    '' AS `work_site`,
    `ticket`.`work_position_name` AS `work_position_name`,
    `ticket`.`work_unit_name` AS `work_unit_name`,
    `ticket`.`department_name` AS `territorial_unit_name`,
    (CASE
        `ticket`.`work_type_code` WHEN 'blinding_pipeline_operation' THEN 'blinding-pipeline_operation'
        ELSE `ticket`.`work_type_code`
    END) AS `work_type`,
    (CASE
        `ticket`.`work_level_code` WHEN 'super' THEN '特级'
        WHEN 'first' THEN '一级'
        WHEN 'second' THEN '二级'
        WHEN 'third' THEN '三级'
        WHEN 'forth' THEN '四级'
        ELSE '-'
    END) AS `work_level`,
    3 AS `work_status`,
    `ticket`.`plan_start_time` AS `plan_start_time`,
    `ticket`.`plan_end_time` AS `plan_end_time`,
    `ticket`.`plan_start_time` AS `actual_start_time`,
    `ticket`.`plan_end_time` AS `actual_end_time`,
    `ticket`.`work_ticket_file_url` AS `ticket_url`,
    (CASE
        WHEN ((`task`.`jsa_id` <> '')
            AND (`task`.`jsa_id` IS NOT NULL)) THEN '关联'
        ELSE '未关联'
    END) AS `if_safety_analysis`,
    NULL AS `region_id`,
    NULL AS `coordinate`,
    NULL AS `position_name`,
    `ticket`.`report_create_date` AS `report_create_date`,
    1 AS `source_type`
FROM
    (`data_sync`.`work_ticket` `ticket`
LEFT JOIN `data_sync`.`work_task` `task` ON
    ((`ticket`.`work_task_id` = `task`.`work_task_id`)))
WHERE
    (`ticket`.`deleted` = 0);
```

### 查看视图

```mysql
# 语法一：查看数据库的表对象，视图对象
mysql> SHOW TABLES;
+---------------------+
| Tables_in_atguigudb |
+---------------------+
| countries           |
| departments         |
| dept_sum_vu         |
| emp                 |
| emp_depart          |
| emp_dept            |
| emp_dept_ysalary    |
| emp_details_view    |
| emp_year_salary     |
| employees           |
| empview             |
| empview2            |
| empvu80             |
| job_grades          |
| job_history         |
| jobs                |
| locations           |
| order               |
| regions             |
| salvu50             |
+---------------------+
20 rows in set (0.00 sec)

# 语法二：查看视图的结构
# DESC/DESCRIBE 视图名称;
mysql> DESC emp_depart;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| emp_dept | varchar(57) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
1 row in set (0.00 sec)

# 语法三：查看视图的属性信息
# 查看视图信息（显示数据表的存储引擎、版本、数据行数和数据大小等）
# 执行结果显示，注释 Comment 为 VIEW，说明该表为视图，其他的信息为 NULL，说明这是一个虚表
mysql> SHOW TABLE STATUS LIKE 'emp_depart';
+------------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+-------------+------------+-----------+----------+----------------+---------+
| Name       | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment | Create_time         | Update_time | Check_time | Collation | Checksum | Create_options | Comment |
+------------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+-------------+------------+-----------+----------+----------------+---------+
| emp_depart | NULL   |    NULL | NULL       | NULL |           NULL |        NULL |            NULL |         NULL |      NULL |           NULL | 2023-05-09 16:45:51 | NULL        | NULL       | NULL      |     NULL | NULL           | VIEW    |
+------------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+-------------+------------+-----------+----------+----------------+---------+
1 row in set (0.00 sec)

# 语法四：查看视图的详细定义信息
# SHOW CREATE VIEW 视图名称;
mysql> SHOW CREATE VIEW emp_depart;
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| View       | Create View                                                                                                                                                                                                                                                        | character_set_client | collation_connection |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
| emp_depart | CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`%` SQL SECURITY DEFINER VIEW `emp_depart` AS select concat(`e`.`last_name`,'(',`d`.`department_name`,')') AS `emp_dept` from (`employees` `e` join `departments` `d`) where (`e`.`department_id` = `d`.`department_id`) | utf8mb3              | utf8_general_ci      |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+
1 row in set (0.00 sec)
```

### 更新视图的数据

#### 一般情况

MySQL 支持使用 INSERT、UPDATE 和 DELETE 语句对视图中的数据进行插入、更新和删除操作。当视图中的数据发生变化时，数据表中的数据也会发生变化，反之亦然。

#### 不可更新的视图

要使视图可更新，视图中的行和底层基本表中的行之间必须存在`一对一`的关系。另外当视图定义出现如下情况时，视图不支持更新操作：

- 在定义视图的时候指定了`ALGORITHM = TEMPTABLE`，视图将不支持 INSERT 和 DELETE 操作；
- 视图中不包含基表中所有被定义为非空又未指定默认值的列，视图将不支持 INSERT 操作；
- 在定义视图的 SELECT 语句中使用了 JOIN 联合查询 ，视图将不支持 INSERT 和 DELETE 操作；
- 在定义视图的 SELECT 语句后的字段列表中使用了`数学表达式`或`子查询`，视图将不支持 INSERT，也不支持 UPDATE 使用了数学表达式、子查询的字段值；
- 在定义视图的 SELECT 语句后的字段列表中使用`DISTINCT`、`聚合函数`、`GROUP BY`、`HAVING`、`UNION`等，视图将不支持 INSERT、UPDATE、DELETE；
- 在定义视图的 SELECT 语句中包含了子查询，而子查询中引用了FROM后面的表，视图将不支持 INSERT、UPDATE、DELETE；
- 视图定义基于一个`不可更新视图`；
- 常量视图。

>虽然可以更新视图数据，但总的来说，视图作为`虚拟表`，主要用于`方便查询`，不建议更新视图的数据。对视图数据的更改，都是通过对实际数据表里数据的操作来完成的。

### 修改和删除视图

#### 修改视图

方式 1：使用`CREATE OR REPLACE VIEW`子句修改视图。

```mysql
CREATE OR REPLACE VIEW empvu80(id_number, name, sal, department_id)
AS
SELECT employee_id, first_name || ' ' || last_name, salary, department_id
FROM employees
WHERE department_id = 80;
```

方式 2：使用`ALTER VIEW`子句修改视图。

```mysql
ALTER VIEW 视图名称
AS
查询语句
```

#### 删除视图

删除视图只是删除视图的定义，并不会删除基表的数据。

语法：

```mysql
DROP VIEW IF EXISTS 视图名称;

DROP VIEW IF EXISTS 视图名称1, 视图名称2, 视图名称3, ...;
```

>说明：基于视图 a、b 创建了新的视图 c，如果将视图 a 或者视图 b 删除，会导致视图 c 的查询失败。这样的视图 c 需要手动删除或修改，否则影响使用。

### 总结

#### 视图优点

**操作简单：**

- 将经常使用的查询操作定义为视图，可以使开发人员不需要关心视图对应的数据表的结构、表与表之间的关联关系，也不需要关心数据表之间的业务逻辑和查询条件，而只需要简单地操作视图即可，极大简化了开发人员对数据库的操作。

**减少数据冗余：**

- 视图跟实际数据表不一样，它存储的是查询语句。所以，在使用的时候，需要通过定义视图的查询语句来获取结果集。而视图本身不存储数据，不占用数据存储的资源，减少了数据冗余。

**数据安全：**

- MySQL 将用户对数据的访问限制在某些数据的结果集上，而这些数据的结果集可以使用视图来实现，用户不必直接查询或操作数据表。这也可以理解为`视图具有隔离性`。视图相当于在用户和实际的数据表之间加了一层虚拟表。

- 同时，MySQL 可以根据权限将用户对数据的访问限制在某些视图上，用户不需要查询数据表，可以直接通过视图获取数据表中的信息。这在一定程度上保障了数据表中数据的安全性。

**适应灵活多变的需求：**

- 当业务系统的需求发生变化后，如果需要改动数据表的结构，则工作量相对较大，可以使用视图来减少改动的工作量。这种方式在实际工作中使用得比较多。


**能够分解复杂的查询逻辑：**

- 数据库中如果存在复杂的查询逻辑，则可以将问题进行分解，创建多个视图获取数据，再将创建的多个视图结合起来，完成复杂的查询逻辑。

#### 视图缺点

如果在实际数据表的基础上创建了视图，那么，如果实际数据表的结构变更了，就需要及时对相关的视图进行相应的维护。特别是嵌套的视图（就是在视图的基础上创建视图），会导致维护变得比较复杂， 可读性不好 ，容易变成系统的潜在隐患。因为创建视图的 SQL 查询可能会对字段重命名，也可能包含复杂的逻辑，这些都会增加维护的成本。

实际项目中，如果视图过多，会导致数据库维护成本的问题。所以，在创建视图的时候，需要结合实际项目需求，综合考虑视图的优点和不足，这样才能正确使用视图，使系统整体达到最优。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql.md