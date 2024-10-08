>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-21`*

## 为什么需要数据库设计

我们在设计数据表的时候，要考虑很多问题。比如：

- 用户都需要什么数据？需要在数据表中保存哪些数据？
- 如何保证数据表中数据的`正确性`？当插入、删除、更新的时候该进行怎样的`约束检查`？
- 如何降低数据表的`数据冗余度`，保证数据表不会因为用户量的增长而迅速扩张？
- 如何让负责数据库维护的人员`更方便`的使用数据库？
- 使用数据库的应用场景也各不相同，可以说针对不同的情况，设计出来的数据表可能千差万别。

现实情况中，面临的场景：

- 数据冗余、信息重复，存储空间浪费。
- 数据更新、插入、删除的异常。
- 无法正确表示信息。
- 丢失有效信息。
- 程序性能差。

良好的数据库设计，则有以下优点：

- 节省数据的存储空间。
- 能够保证数据的完整性。
- 方便进行数据库应用系统的开发。

总之，开始设置数据库的时候，我们就需要重视数据表的设计。为了建立`冗余较小`、`结构合理`的数据库，设计数据库时`必须遵循一定的规则`。

## 范式

### 范式简介

**在关系型数据库中，关于数据表设计的基本原则、规则就称为`范式`。**可以理解为，一张数据表的设计结构需要满足的某种设计标准的`级别`。要想设计一个结构合理的关系型数据库，必须满足一定的范式。

范式的英文名称是 Normal Form，简称 NF。它是英国人 E.F.Codd 在上个世纪 70 年代提出关系数据库模型后总结出来的。范式是关系数据库理论的基础，也是我们在设计数据库结构过程中所要遵循的`规则`和`指导方法`。

### 范式都包括哪些

目前关系型数据库有六种常见范式，按照范式级别，从低到高分别是：`第一范式 (1NF)、第二范式 (2NF)、第三范式 (3NF)、巴斯-科德范式 (BCNF)、第四范式 (4NF) 和第五范式 (5NF，又称完美范式)。`

数据库的范式设计越高阶，冗余度就越低，同时高阶的范式一定符合低阶范式的要求，满足最低要求的范式是第一范式（1NF）。在第一范式的基础上，进一步满足更多规范要求的称为第二范式（2NF），其余范式以此类推。

![image-20231105101706772](mysql-series-16-database-design-specifications/image-20231105101706772.png)

`一般来说，在关系型数据库设计中，最高也就遵循到 BCNF，普遍的是 3NF。`但范式不是绝对的，有时候为了提高某些查询性能，我们还需要破坏范式规则，也就是`反规范化`。

### 键和相关属性的概念

范式的定义会使用到主键和候选键，数据库中的键（Key）由一个或者多个属性组成。数据表中常用的几种键和属性的定义：

- `超键`：能`唯一标识`元组的属性集叫做超键。（属性集，可以理解为一个或多个字段的组合）
- `候选键`：如果超键不包括多余的属性，那么这个超键就是候选键。用户可以从候选键中选择一个作为主键。
- `外键`∶如果数据表 R1 中的某属性集不是 R1 的主键，而是另一个数据表 R2 的主键，那么这个属性集就是数据表 R1 的外键。
- `主属性`：包含在任一候选键中的属性称为主属性。
- `非主属性`：与主属性相对，指的是不包含在任何一个候选键中的属性。

通常，我们也将候选键称之为`码`，把主键也称为`主码`。因为键可能是由多个属性组成的，针对单个属性，我们还可以用主属性和非主属性来进行区分。

举例来说，假设有两个表：

- 球员表（player）：球员编号 | 姓名 | 身份证号 | 年龄 | 球队编号


- 球队表（team）：球队编号 | 主教练 | 球队所在地

那么：

- 超键：对于球员表来说，超键就是包括球员编号或者身份证号（球员编号或者身份证号是能唯一标识元组的）的任意组合，比如（球员编号）、（球员编号，姓名）、（身份证号，年龄）等。
- 候选键：就是最小的超键，对于球员表来说，候选键就是（球员编号）或者（身份证号）。
- 主键：我们自己选定，也就是从候选键中选择一个，比如（球员编号）。
- 外键：球员表中的球队编号。
- 主属性、非主属性：在球员表中，主属性是（球员编号）（身份证号），其他的属性（姓名）（年龄）（球队编号）都是非主属性。

### 第一范式

**第一范式主要是确保数据表中每个字段的值必须具有`原子性`，也就是说数据表中每个字段的值为`不可再次拆分的最小数据单位`。**

我们在设计某个字段的时候，对于字段 x 来说，不能把字段 x 拆分成字段 x_1 和字段 x_2。事实上，任何的 DBMS 都会满足第一范式的要求，不会将字段进行拆分。

**示例一：**

- 假设一家公司要存储员工的姓名和联系方式。它创建一个如下表：

  ![image-20231105111915247](mysql-series-16-database-design-specifications/image-20231105111915247.png)

- 该表不符合 1NF ，因为规则说 "表的每个属性必须具有原子（单个）值"，lisi 和 zhaoliu 员工的 emp_mobile 值违反了该规则。为了使表符合 1NF ，我们应该有如下表数据：

  ![image-20231105112027619](mysql-series-16-database-design-specifications/image-20231105112027619.png)

**示例二：**

- user 表的设计不符合第一范式：

  ![image-20231105112149639](mysql-series-16-database-design-specifications/image-20231105112149639.png)

- 其中，user_info 字段为用户信息，可以进一步拆分成更小粒度的字段，不符合数据库设计对第一范式的要求。将 user_info 拆分后如下：

  ![image-20231105112426133](mysql-series-16-database-design-specifications/image-20231105112426133.png)

**示例三：**

- 属性的原子性是主观的 。例如，Employees 关系中雇员姓名应当使用 1 个（fullname），2 个（firstname 和 lastname），还是3个（firstname、middlename 和 lastname）属性表示呢？答案取决于应用程序。如果应用程序需要分别处理雇员的姓名部分（如：用于搜索目的），则有必要把它们分开。否则，不需要。

  ![image-20231105112551568](mysql-series-16-database-design-specifications/image-20231105112551568.png)

  ![image-20231105112627064](mysql-series-16-database-design-specifications/image-20231105112627064.png)

### 第二范式

**第二范式要求，在满足第一范式的基础上，还要满足数据表里的每一条数据记录，都是`可唯一标识`的（也就是一定有主键）。而且所有非主键字段，都必须`完全依赖主键`，不能只依赖主键的一部分。**如果知道主键的所有属性的值，就可以检索到任何元组（行）的任何属性的任何值。（要求中的主键，其实可以拓展替换为候选键）

另外第二范式只能完全函数依赖，不能部分函数依赖。

**示例一：**

- 成绩表 （学号，课程号，成绩）关系中，（学号，课程号）可以决定成绩，但是学号不能决定成绩，课程号也不能决定成绩，所以 "（学号，课程号）---> 成绩" 就是`完全依赖关系`。

**示例二：**

- `比赛表 player_game`，里面包含球员编号、姓名、年龄、比赛编号、比赛时间和比赛场地等属性，这里候选键和主键都为（球员编号，比赛编号），我们可以通过候选键（或主键）来决定如下的关系：

  ```tex
  (球员编号, 比赛编号) → (姓名, 年龄, 比赛时间, 比赛场地，得分)
  ```

- 但是这个数据表不满足第二范式，因为数据表中的字段之间还存在着如下的对应关系：

  ```tex
  # 姓名和年龄部分依赖球员编号
  (球员编号) → (姓名，年龄)
  
  # 比赛时间, 比赛场地部分依赖(球员编号, 比赛编号)
  (比赛编号) → (比赛时间, 比赛场地)
  ```

对于非主属性来说，并非完全依赖候选键。这样会产生怎样的问题呢？（为什么要满足 2NF）

- `数据冗余`： 如果一个球员可以参加 m 场比赛，那么球员的姓名和年龄就重复了 m - 1 次。一个比赛也可能会有 n 个球员参加，比赛的时间和地点就重复了 n - 1 次。

- `插入异常`： 如果我们想要添加一场新的比赛，但是这时还没有确定参加的球员都有谁，那么就没法插入。

- `删除异常`： 如果我要删除某个球员编号，如果没有单独保存比赛表的话，就会同时把比赛信息删除掉。

- `更新异常`： 如果我们调整了某场比赛的时间，那么数据表中所有这场比赛的时间都需要进行调整，否则就会出现一场比赛时间不同的情况。

为了避免出现上述的情况，我们可以把球员比赛表设计为下面的三张表：

| 表名                        | 属性（字段）                       |
| --------------------------- | ---------------------------------- |
| 球员 player 表              | 球员编号、姓名和年龄等属性         |
| 比赛 game 表                | 比赛编号、比赛时间和比赛场地等属性 |
| 球员比赛关系 player_game 表 | 球员编号、比赛编号和得分等属性     |

这样的话，每张数据表都符合第二范式，也就避免了异常情况的发生。

> **1NF 告诉我们`字段属性`需要是原子性的，而 2NF 告诉我们`一张表就是一个独立的对象`，一张表只表达一个意思。**

**示例三：**

- 定义一个名为 Orders 的关系，表示订单和订单行的信息：

  ![image-20231105135908487](mysql-series-16-database-design-specifications/image-20231105135908487.png)

  - 违反了第二范式，因为有非主键属性仅依赖于候选键（或主键）的一部分。例如，可以仅通过 orderid 找到订单的 orderdate，以及 customerid 和 companyname，而没有必要再去使用 productid。

- 修改为 Orders 表和 OrderDetails 表如下，此时符合第二范式。

  ![image-20231105142331128](mysql-series-16-database-design-specifications/image-20231105142331128.png)

> 小结：`第二范式要求实体的属性完全依赖主关键字。`如果存在不完全依赖，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与元实体之间是一对多的关系。

### 第三范式

**第三范式是在第二范式的基础上，确保数据表中的`每一个非主键字段都和主键字段直接相关`，也就是说，要求数据表中的所有非主键字段不能依赖于其他非主键字段。**即，不能存在非主属性 A 依赖于非主属性 B，非主属性 B 依赖于主键 C 的情况，即存在 "A ---> B ---> C" 的决定关系。通俗地讲，该规则的意思是所有非主键属性之间不能有传递依赖关系，必须相互独立。（这里的主键可以拓展为候选键）

**示例一：**

- **部门信息表：**每个部门有部门编号（dept_id）、部门名称、部门简介等信息。
- **员工信息表：**每个员工有员工编号、姓名、部门编号。列出部门编号后就不能再将部门名称、部门简介等与部门有关的信息再加入员工信息表中。
- 如果不存在部门信息表，则根据第三范式（3NF）也应该构建它，否则就会有大量的数据冗余。

**示例二：**

- 如下表，商品类别名称依赖于商品类别编号，不符合第三范式：

  ![image-20231105143003915](mysql-series-16-database-design-specifications/image-20231105143003915.png)

- 将其修改为商品表和商品类别表，商品表 goods 通过商品类别 id 字段（category_id）与商品类别表 goods_category 进行关联。

  ![image-20231105143223182](mysql-series-16-database-design-specifications/image-20231105143223182.png)

**示例三：**

- 球员 player 表 ：球员编号、姓名、球队名称和球队主教练。现在，我们把属性之间的依赖关系画出来，如下图所示：

  ![image-20231105192956621](mysql-series-16-database-design-specifications/image-20231105192956621.png)

- 可以看到，球员编号决定了球队名称，同时球队名称决定了球队主教练，非主属性球队主教练就会传递依赖于球员编号，因此不符合 3NF 的要求。

- 如果要达到 3NF 的要求，需要把数据表拆成下面这样：

  | 表名   | 属性（字段）             |
  | ------ | ------------------------ |
  | 球队表 | 球员编号、姓名和球队名称 |
  | 球员表 | 球队名称、球队主教练     |

**示例四：**

- 修改第二范式中的示例三。

- 此时的 Orders 关系包含 orderid、orderdate、customerid 和 companyname 属性，主键定义为 orderid。customerid 和 companyname 均依赖于主键 orderid。例如，你需要通过 orderid 主键来查找代表订单中客户的 customerid，同样，你需要通过 orderid 主键查找订单中客户的公司名称（companyname）。然而， customerid 和 companyname 也是互相依靠的。为满足第三范式，可以改写如下：

  ![image-20231105193501255](mysql-series-16-database-design-specifications/image-20231105193501255.png)

>符合3NF后的数据模型通俗地讲，2NF 和 3NF 通常以这句话概括：`"每个非键属性依赖于键，依赖于整个键，并且除了键别无他物"。`

### 小结

关于数据表的设计，有三个范式要遵循：

- 第一范式（1NF），`确保每列保持原子性`。数据库的每一列都是不可分割的原子数据项，不可再分的最小数据单元，而不能是集合、数组、记录等非原子数据项。
- 第二范式（2NF），`确保每列都和主键完全依赖`。尤其在复合主键的情况向下，非主键部分不应该依赖于部分主键。
- 第三范式（3NF），`确保每列都和主键直接相关`，而不是间接相关。

**范式的优点：**数据的标准化有助于消除数据库中的数据冗余，`第三范式通常被认为在性能、拓展性和数据完整性方面达到了最好的平衡。`

**范式的缺点：**范式的使用，可能降低查询的效率。因为范式等级越高，设计出来的数据表就越多、越精细，数据的冗余度就越低，进行数据查询的时候就可能需要关联多张表，这不但代价昂贵，也可能使一些索引策略无效。

范式只是提出了设计的标准，实际上设计数据表时，未必一定要符合这些标准。开发中，我们会出现为了性能和读取效率违反范式化的原则，通过增加少量的冗余或重复的数据来提高数据库的读性能，减少关联查询，join 表的次数，实现空间换取时间的目的。因此，在实际的设计过程中要理论结合实际，灵活运用。

>范式本身没有优劣之分，只有适用场景不同。没有完美的设计，只有合适的设计，我们在数据表的设计中，还需要根据需求将范式和反范式混合使用。

## 反范式化

### 概述

有的时候，不能简单按照规范要求设计数据表，因为有的数据看似冗余，其实对业务来说十分重要。这个时候，我们就要`遵循业务优先的原则`，首先满足业务需求，再尽量减少冗余。

如果数据库中的数据量比较大，系统的 UV 和 PV 访问频次比较高，则完全按照 MySQL 的三大范式设计数据表，读数据时会产生大量的关联查询，在一定程度上会影响数据库的读性能。如果我们想对查询效率进行优化，反范式化也是一种优化思路。此时，可以通过在数据表中增加冗余字段来提高数据库的读性能。

> **规范化 VS 性能：**
>
> 1. 为满足某种商业目标，数据库性能比规范化数据库更重要。
> 2. 在数据规范化的同时，要综合考虑数据库的性能。
> 3. 通过在给定的表中添加额外的字段，以大量减少需要从中搜索信息所需的时间。
> 4. 通过在给定的表中插入计算列，以方便查询。

### 应用举例

**示例一：**

- 员工的信息存储在 employees 表中，部门信息存储在 departments 表中，通过 employees 表中的 department_id 字段与 departments 表建立关联关系。如果要查询一个员工所在部门的名称，可以使用下面的 SQL。如果经常需要进行这个操作，连接查询就会浪费很多时间。可以在 employees 表中增加一个冗余字段 department_name，这样就不用每次都进行连接操作了。

  ```mysql
  SELECT employee_id, department_name FROM employees e JOIN departments d ON e.department_id = d.department_id;
  ```

**示例二：**

- 反范式化的 goods商品信息表 设计如下：

  ![image-20231105195225812](mysql-series-16-database-design-specifications/image-20231105195225812.png)

**示例三：**

- 假设有 2 个表，分别是商品流水表（trans ）和商品信息表（goodsinfo）。商品流水表里有 400 万条流水记录，商品信息表里有 2000 条商品记录。

  - 商品流水表：

    ![image-20231105195442786](mysql-series-16-database-design-specifications/image-20231105195442786.png)

  - 商品信息表：

    ![image-20231105195506227](mysql-series-16-database-design-specifications/image-20231105195506227.png)

- 上面这两个表，都是符合第三范式要求的。但是，在项目的实施过程中，对流水的查询频率很高，而且为了获取商品名称，基本都会用到与商品信息表的连接查询。为了减少连接，可以直接把商品名称字段加到流水表里面。这样一来，就可以直接从流水表中获取商品名称字段了。虽然增加了冗余字段，但是避免了关联查询，提升了查询的效率。新的商品流水表如下所示：

  ![image-20231105200705246](mysql-series-16-database-design-specifications/image-20231105200705246.png)

**示例四：**

- 课程评论表 class_comment，对应的字段名称及含义如下：

  ![image-20231105200825357](mysql-series-16-database-design-specifications/image-20231105200825357.png)

- 学生表 student，对应的字段名称及含义如下：

  ![image-20231105200859601](mysql-series-16-database-design-specifications/image-20231105200859601.png)

- 在实际应用中，我们在显示课程评论的时候，通常会显示这个学生的昵称，而不是学生 ID，因此，当我们想要查询某个课程的前 1000 条评论时，需要关联 class_comment 和 student这两张表来进行查询。

**实验数据：模拟两张百万量级的数据表**

为了更好地进行 SQL 优化实验，我们需要给学生表和课程评论表随机模拟出百万量级的数据。我们可以通过存储过程来实现模拟数据。

- 创建表：

  ```mysql
  mysql> set names utf8;
  Query OK, 0 rows affected, 1 warning (0.00 sec)
  
  mysql> CREATE DATABASE atguigudb2;
  Query OK, 1 row affected (0.01 sec)
  
  mysql> USE atguigudb2;
  Database changed
  
  # 学生表
  mysql> CREATE TABLE student(
      -> stu_id INT PRIMARY KEY AUTO_INCREMENT,
      -> stu_name VARCHAR(25),
      -> create_time DATETIME
      -> );
  Query OK, 0 rows affected (0.03 sec)
  
  # 课程评论表
  mysql> CREATE TABLE class_comment(
      -> comment_id INT PRIMARY KEY AUTO_INCREMENT,
      -> class_id INT,
      -> comment_text VARCHAR(35),
      -> comment_time DATETIME,
      -> stu_id INT
      -> );
  Query OK, 0 rows affected (0.03 sec)
  ```

- 创建存储过程：

  ```mysql
  # 创建向学生表中添加数据的存储过程
  mysql> DELIMITER //
  mysql> CREATE PROCEDURE batch_insert_student(IN START INT(10),IN max_num INT (10))
      -> BEGIN
      -> DECLARE i INT DEFAULT 0;
      -> DECLARE date_start DATETIME DEFAULT ('2017-01-01 00:00:00');
      -> DECLARE date_temp DATETIME;
      -> SET date_temp = date_start;
      -> SET autocommit=0;
      -> REPEAT
      -> SET i=i+1;
      -> SET date_temp = DATE_ADD(date_temp,INTERVAL RAND( )*60 SECOND);
      -> INSERT INTO student(stu_id, stu_name,create_time)
      -> VALUES ((START+i), CONCAT ('stu_',i), date_temp) ;
      -> UNTIL i = max_num
      -> END REPEAT;
      -> COMMIT;
      -> END //
  Query OK, 0 rows affected, 2 warnings (0.02 sec)
  
  mysql> DELIMITER ;
  
  # 创建向课程评论表中添加数据的存储过程
  mysql> DELIMITER //
  mysql> CREATE PROCEDURE batch_insert_class_comments(IN START INT(10),IN max_num INT (10))
      -> BEGIN
      -> DECLARE i INT DEFAULT 0;
      -> DECLARE date_start DATETIME DEFAULT ('2018-01-01 00:00:00');
      -> DECLARE date_temp DATETIME;
      -> DECLARE comment_text VARCHAR(25);
      -> DECLARE stu_id INT;
      -> SET date_temp = date_start;
      -> SET autocommit=0;
      -> REPEAT
      -> SET i=i+1;
      -> SET date_temp = DATE_ADD(date_temp,INTERVAL RAND( )*60 SECOND);
      -> SET comment_text=SUBSTR(MD5(RAND()),1,20);
      -> SET stu_id=FLOOR(RAND()*1000000);
      -> INSERT INTO `class_comment`(`comment_id`,`class_id`,`comment_text`,`comment_time`,`stu_id`)
      -> VALUES ((START+i),10001,comment_text,date_temp,stu_id);
      -> UNTIL i = max_num
      -> END REPEAT;
      -> COMMIT;
      -> END //
  Query OK, 0 rows affected, 2 warnings (0.01 sec)
  
  mysql> DELIMITER ;
  ```

- 调用存储过程：

  ```mysql
  # 调用存储过程，学生 id 从 10001 开始，添加 1000000 条数据
  mysql> CALL batch_insert_student(10000, 1000000);
  Query OK, 0 rows affected (31.58 sec)
  
  # 添加数据的过程的调用，一个 1000000 条数据
  mysql> CALL batch_insert_class_comments(10000, 1000000);
  Query OK, 0 rows affected (34.81 sec)
  
  mysql> SELECT COUNT(*) FROM student;
  +----------+
  | COUNT(*) |
  +----------+
  |  1000000 |
  +----------+
  1 row in set (0.03 sec)
  
  mysql> SELECT COUNT(*) FROM class_comment;
  +----------+
  | COUNT(*) |
  +----------+
  |  1000000 |
  +----------+
  1 row in set (0.03 sec)
  ```

- 测试：

  ```mysql
  mysql> SELECT p.comment_text, p.comment_time, stu.stu_name FROM class_comment AS p LEFT JOIN student AS stu ON p.stu_id = stu.stu_id WHERE p.class_id = 10001 ORDER BY p.comment_id DESC LIMIT 10000;
  +----------------------+---------------------+------------+
  | comment_text         | comment_time        | stu_name   |
  +----------------------+---------------------+------------+
  | ea4d03873f7f3a26e1ba | 2018-12-14 03:26:26 | stu_209036 |
  | 1fe6697da6c77d637497 | 2018-12-14 03:26:22 | stu_740513 |
  | 5fafc1b2b297f23159cd | 2018-12-14 03:26:21 | stu_168595 |
  | badca0f8d453eb0b67bf | 2018-12-10 15:29:16 | stu_288793 |
  | 219f577d16b8005ca971 | 2018-12-10 15:28:48 | stu_259916 |
  | dcb3e24911b5bcaa47a8 | 2018-12-10 15:28:11 | stu_815722 |
  +----------------------+---------------------+------------+
  10000 rows in set (0.04 sec)
  ```

  - 运行时长为 0.04 秒，对于网站的响应来说，这已经很慢了，用户体验会非常差。如果我们想要提升查询的效率，可以允许适当的数据冗余，也就是在商品评论表中增加用户昵称字段，在 class_comment 数据表的基础上增加 stu_name 字段，就得到了 class_comment2 数据表。

- 反范式优化实验对比：

  ```mysql
  # 进行反范式化设计
  
  # 表的复制
  mysql> CREATE TABLE class_comment1 AS SELECT * FROM class_comment;
  Query OK, 1000000 rows affected (5.70 sec)
  Records: 1000000  Duplicates: 0  Warnings: 0
  
  # 添加主键，保证 class_comment1 与 class_comment 的结构相同
  mysql> ALTER TABLE class_comment1 ADD PRIMARY KEY(comment_id);
  Query OK, 0 rows affected (8.74 sec)
  Records: 0  Duplicates: 0  Warnings: 0
  
  mysql> SHOW INDEX FROM class_comment1;
  +----------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
  | Table          | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
  +----------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
  | class_comment1 |          0 | PRIMARY  |            1 | comment_id  | A         |      996244 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
  +----------------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
  1 row in set (0.00 sec)
  
  # 向课程评论表中增加 stu_name 字段
  mysql> ALTER TABLE class_comment1 ADD stu_name VARCHAR(25);
  Query OK, 0 rows affected (0.02 sec)
  Records: 0  Duplicates: 0  Warnings: 0
  
  # 给新添加的字段赋值
  mysql> UPDATE class_comment1 c SET stu_name = (SELECT stu_name FROM student s WHERE c.stu_id = s.stu_id);
  Query OK, 989941 rows affected (32.28 sec)
  Rows matched: 1000000  Changed: 989941  Warnings: 0
  ```

- 如果我们想要查询课程 ID 为 10001 的前 10000 条评论，需要写成下面这样：

  ```mysql
  mysql> SELECT comment_text, comment_time, stu_name FROM class_comment1 WHERE class_id = 10001 ORDER BY class_id DESC LIMIT 10000;
  +----------------------+---------------------+------------+
  | comment_text         | comment_time        | stu_name   |
  +----------------------+---------------------+------------+
  | 98ac57e47f969ccb3d5e | 2018-01-01 00:00:58 | stu_657565 |
  | 9417961cb63119c96cbc | 2018-01-01 00:01:10 | stu_236175 |
  | cb5b9be56b3ce6de9f0f | 2018-01-01 00:01:30 | stu_659727 |
  | 339b5f86158833d800c0 | 2018-01-04 11:59:26 | stu_411945 |
  | 2e069c3facbd62cf2051 | 2018-01-04 11:59:37 | stu_759542 |
  | 979fd006b1ea975b5c4d | 2018-01-04 12:00:28 | stu_160871 |
  +----------------------+---------------------+------------+
  10000 rows in set (0.00 sec)
  ```

  - 优化之后只需要扫描一次聚集索引即可，运行时间为 0.00 秒，查询时间比之前少很多。 你能看到，在数据量大的情况下，查询效率会有显著的提升。

### 反范式的新问题

- `存储空间变大`了。
- 一个表中字段做了修改，另一个表中冗余的字段也需要做同步修改，否则`数据不一致`。
- 若采用存储过程来支持数据的更新、删除等额外操作，如果更新频繁，会非常`消耗系统资源`。
- 在`数据量小`的情况下，反范式不能体现性能的优势，可能还会让数据库的设计更加`复杂`。

### 反范式的适用场景

当`冗余信息有价值`或者能`大幅度提高查询效率`的时候，我们才会采取反范式的优化。

1. 增加冗余字段的建议。
   - 这个冗余字段不需要经常进行修改。
   - 这个冗余字段查询的时候不可或缺（因为经常要用，所以才增加该冗余字段）。


2. 历史快照、历史数据的需要。
   - 在现实生活中，我们经常需要一些冗余信息，比如订单中的收货人信息，包括姓名、电话和地址等。每次发生的订单收货信息都属于历史快照，需要进行保存，但用户可以随时修改自己的信息，这时保存这些冗余信息是非常有必要的。


反范式优化也常用在数据仓库的设计中，因为数据仓库通常存储历史数据，对增删改的实时性要求不强，对历史数据的分析需求强。这时适当允许数据的冗余度，更方便进行数据分析。

简单总结下数据仓库和数据库在使用上的区别：

- 数据库设计的目的在于捕获数据，而数据仓库设计的目的在于分析数据；
- 数据库对数据的增删改实时性要求强，需要存储在线的用户数据，而数据仓库存储的一般是历史数据；
- 数据库设计需要尽量避免冗余，但为了提高查询效率也允许一定的冗余度，而数据仓库在设计上更偏向采用反范式设计。

> 注意：反范式反的是第二范式或第三范式，第一范式是一定要遵守的。

## 巴斯范式

人们在 3NF 的基础上进行了改进，提出了`巴斯范式（BCNF）`，也叫做`巴斯-科德范式（Boyce-Codd NormalForm）`。BCNF 被认为没有新的设计规范加入，`只是对第三范式中设计规范要求更强`，使得数据库冗余度更小。所以，称为是修正的第三范式，或扩充的第三范式，BCNF 不被称为第四范式。

若一个关系达到了第三范式，并且它只有一个候选键，或者它的每个候选键都是单属性，则该关系自然达到 BCNF。

> 一般来说，一个数据库设计符合 3NF 或 BCNF 就可以了。

### 案例

我们分析如下表的范式情况：

![image-20231105231508896](mysql-series-16-database-design-specifications/image-20231105231508896.png)

在这个表中，一个仓库只有一个管理员，同时一个管理员也只管理一个仓库。先来梳理下这些属性之间的依赖关系。

- 仓库名决定了管理员，管理员也决定了仓库名，同时（仓库名，物品名）的属性集合可以决定数量这个属性。这样，我们就可以找到数据表的候选键。


- 候选键：是（管理员，物品名）和（仓库名，物品名），然后从候选键中选择一个作为主键，比如（仓库名，物品名）。
- 主属性：包含在任一候选键中的属性，也就是仓库名，管理员和物品名。

- 非主属性 ：数量这个属性。

### 是否符合三范式

如何判断一张表的范式呢？需要根据范式的等级，从低到高来进行判断：

- 首先，数据表每个属性都是原子性的，符合 1NF 的要求；


- 其次，数据表中非主属性 "数量" 都与候选键全部依赖，（仓库名，物品名）决定数量，（管理员，物品名）决定数量。因此，数据表符合 2NF 的要求；


- 最后，数据表中的非主属性，不传递依赖于候选键。因此符合 3NF 的要求。

### 存在的问题

既然数据表已经符合了 3NF 的要求，是不是就不存在问题了呢？来看下面的情况：

- 增加一个仓库，但是还没有存放任何物品。根据数据表实体完整性的要求，主键不能有空值，因此会出现插入异常 ；
- 如果仓库更换了管理员，就可能会修改数据表中的多条记录；
- 如果仓库里的商品都卖空了，那么此时仓库名称和相应的管理员名称也会随之被删除。

可以能看到，即便数据表符合 3NF 的要求，同样可能存在插入，更新和删除数据的异常情况。

### 问题解决

首先需要确认造成异常的原因：主属性仓库名对于候选键（管理员，物品名）是部分依赖的关系，这样就有可能导致上面的异常情况。因此引入 BCNF，它在 3NF 的基础上消除了主属性对候选键的部分依赖或者传递依赖关系。

如果在关系 R 中，U 为主键，A 属性是主键的一个属性，若存在 A ---> Y，Y 为主属性（也就是非候选码中的属性），则该关系不属于 BCNF。

根据 BCNF 的要求，需要把仓库管理关系 warehouse_keeper 表拆分成下面这样：

- 仓库表 ：（仓库名，管理员）
- 库存表 ：（仓库名，物品名，数量）

这样就不存在主属性对于候选键的部分依赖或传递依赖，上面数据表的设计就符合 BCNF。

> 再举例如下：
>
> 有一个学生导师表，其中包含字段：学生 ID，专业，导师，专业 GPA，这其中学生 ID 和专业是联合主键。
>
> ![image-20231106082639953](mysql-series-16-database-design-specifications/image-20231106082639953.png)
>
> 这个表的设计满足三范式，但是这里存在另一个依赖关系，"专业" 依赖于 "导师"，也就是说每个导师只做一个专业方面的导师，只要知道了是哪个导师，自然就知道是哪个专业的了。
>
> 所以这个表的部分主键 Major 依赖于非主键属性 Advisor，那么我们可以进行以下的调整，拆分成 2 个表：
>
> 学生导师表：
>
> ![image-20231106090057819](mysql-series-16-database-design-specifications/image-20231106090057819.png)
>
> 导师表：
>
> ![image-20231106090132192](mysql-series-16-database-design-specifications/image-20231106090132192.png)
>
> 

## 第四范式

`多值依赖`的概念：

- 多值依赖即属性之间的一对多关系，记为 K →→ A。
- 函数依赖事实上是单值依赖，所以不能表达属性值之间的一对多关系。
- 平凡的多值依赖∶全集 U = K + A，一个 K 可以对应于多个 A，即 K →→ A。此时整个表就是一组一对多关系。
- 非平凡的多值依赖：全集 U = K + A + B，一个 K 可以对应于多个 A，也可以对应于多个 B，A 与 B 互相独立，即 K →→ A，K →→ B。整个表有多组一对多关系，且有："一" 部分是相同的属性集合，"多" 部分是互相独立的属性集合。

**示例一：**

职工表（职工编号，职工孩子姓名，职工选修课程）。

在这个表中，同一个职工可能会有多个职工孩子姓名。同样，同一个职工也可能会有多个职工选修课程，即这里存在着多值事实，不符合第四范式。

如果要符合第四范式，只需要将上表分为两个表，使它们只有一个多值事实，例如： 职工表一（职工编号，职工孩子姓名），职工表二（职工编号，职工选修课程），两个表都只有一个多值事实，所以符合第四范式。

**示例二：**

比如建立课程、教师、教材的模型。我们规定，每门课程有对应的一组教师，每门课程也有对应的一组教材，一门课程使用的教材和教师没有关系。我们建立的关系表如下：课程 ID，教师 ID，教材 ID，这三列作为联合主键。

为了表述方便，我们用教师 Name 代替 ID，这样更容易看懂：

![image-20231106090818655](mysql-series-16-database-design-specifications/image-20231106090818655.png)

这个表除了主键，就没有其他字段了，所以肯定满足 BCNF，但是却存在多值依赖导致的异常。

假如下学期想采用一本新的英版高数教材，但是还没确定具体哪个老师来教，那么就无法在这个表中维护 Course 高数和 Book 英版高数教材的的关系。

解决办法是把这个多值依赖的表拆解成 2 个表，分别建立关系。这是拆分后的表：

![image-20231106104448526](mysql-series-16-database-design-specifications/image-20231106104448526.png)

![image-20231106104509220](mysql-series-16-database-design-specifications/image-20231106104509220.png)

## 第五范式、域键范式

除了第四范式外，还有更高级的`第五范式（又称完美范式）和域键范式（DKNF）`。

在满足第四范式（4NF）的基础上，消除不是由候选键所蕴含的连接依赖。如果关系模式 R 中的每一个连接依赖均由 R 的候选键所隐含，则称此关系模式符合第五范式。

函数依赖是多值依赖的一种特殊的情况，而多值依赖实际上是连接依赖的一种特殊情况。但连接依赖不像函数依赖和多值依赖可以由语义直接导出 ，而是在关系连接运算时才反映出来。存在连接依赖的关系模式仍可能遇到数据冗余及插入、修改、删除异常等问题。

第五范式处理的是无损连接问题，这个范式基本`没有实际意义`，因为无损连接很少出现，而且难以察觉。而域键范式试图定义一个终极范式 ，该范式考虑所有的依赖和约束类型，但是实用价值也是最小的，只存在理论研究中。

## 实战案例

**需求：商超进货系统中的进货单表进行剖析。**

进货单表：

![image-20231106124225094](mysql-series-16-database-design-specifications/image-20231106124225094.png)

这个表中的字段很多，表里的数据量也很惊人。大量重复导致表变得庞大，效率极低。如何改造？

>在实际工作场景中，这种由于数据表结构设计不合理，而导致的数据重复的现象并不少见。往往是系统虽然能够运行，承载能力却很差，稍微有点流量，就会出现内存不足、CUP 使用率飙升的情况，甚至会导致整个项目失败。

### 迭代 1 次：考虑 1NF

**第一范式要求：所有的字段都是基本数据字段，不可进一步拆分。这里需要确认，所有的列中，每个字段只包含一种数据。**

这张表里把 "property" 这一字段，拆分成 "specification (规格)" 和 "unit (单位)"，这 2 个字段如下：

![image-20231106184832839](mysql-series-16-database-design-specifications/image-20231106184832839.png)

### 迭代 2 次：考虑 2NF

**第二范式要求：在满足第一范式的基础上，还要满足数据表里的每一条数据记录，都是可唯一标识的。而且所有字段，都必须完全依赖主键，不能只依赖主键的一部分。**

第 1 步，就是要确定这个表的主键。通过观察发现，字段 "listnumber (单号)" + "barcode (条码)" 可以唯一标识每一条记录，可以作为主键。

第 2 步，确定好了主键以后，判断哪些字段完全依赖主键，哪些字段只依赖于主键的一部分。把只依赖于主键一部分的字段拆分出去，形成新的数据表。

首先，进货单明细表里面的 "goodsname (名称)"，"specification (规格)"，"unit (单位)" 这些信息是商品的属性，只依赖于 "barcode (条码)"，不完全依赖主键，可以拆分出去。把这 3 个字段加上它们所依赖的字段 "barcode (条码)"，拆分形成一个新的数据表 "商品信息表"。这样一来，原来的数据表就被拆分成了两个表。

- 商品信息表：

  ![image-20231106190656366](mysql-series-16-database-design-specifications/image-20231106190656366.png)

- 进货单表：

  ![image-20231106190744501](mysql-series-16-database-design-specifications/image-20231106190744501.png)

此外，字段 "supplierid (供应商编号)"，"suppliername (供应商名称)"，"stock (仓库)" 只依赖于 "listnumber (单号)"，不完全依赖于主键，所以，可以把 "supplierid"，"suppliername"，"stock" 这 3 个字段拆出去，再加上它们依赖的字段 "listnumber (单号)"，就形成了一个新的表 "进货单头表"。剩下的字段，会组成新的表，我们叫它 "进货单明细表"。原来的数据表就拆分成了 3 个表。

- 进货单头表：

  ![image-20231106191110728](mysql-series-16-database-design-specifications/image-20231106191110728.png)

- 进货单明细表：

  ![image-20231106191217636](mysql-series-16-database-design-specifications/image-20231106191217636.png)

- 商品信息表：

  ![image-20231106190656366](mysql-series-16-database-design-specifications/image-20231106190656366.png)

现在来分析一下拆分后的 3 个表，保证这 3 个表都满足第二范式的要求。

第 3 步，在 "商品信息表" 中，字段 "barcode" 是有可能存在重复的，比如，用户门店可能有散装称重商品和自产商品，会存在条码共用的情况。所以，所有的字段都不能唯一标识表里的记录。这个时候必须给这个表加上一个主键，比如说是自增字段 "itemnumber"。

现在就可以把进货单明细表里面的字段 "barcode" 都替换成字段 "itemnumber"，这就得到了新的表。

- 进货单明细表：

  ![image-20231106191535891](mysql-series-16-database-design-specifications/image-20231106191535891.png)

- 商品信息表：

  ![image-20231106191617188](mysql-series-16-database-design-specifications/image-20231106191617188.png)

拆分后的 3 个数据表，就全部满足了第二范式的要求。

### 迭代 3 次：考虑 3NF

进货单头表还有数据冗余的可能。因为 "supplername" 依赖 "supplierid"，那么，这个时候，就可以按照第三范式的原则进行拆分了。进一步拆分一下进货单头表，把它拆解成供货商表和进货单头表。

- 供货商表：

  ![image-20231106194914167](mysql-series-16-database-design-specifications/image-20231106194914167.png)

- 进货单头表：

  ![image-20231106194949527](mysql-series-16-database-design-specifications/image-20231106194949527.png)

这 2 个表都满足第三范式的要求了。

### 反范式化：业务优先的原则

在进货单明细表中，"quantity * importprice = importvalue"，"importprice"、"quantity" 和 "importvalue"，可以通过任意两个计算出第三个来，这就存在冗余字段。如果严格按照第三范式的要求，应该进行进一步优化。优化的办法是删除其中一个字段，只保留另外 2 个，这样就没有冗余数据了。

可是，真的可以这样做吗？要回答这个问题，就要先了解下实际工作中的业务优先原则。

所谓的业务优先原则，就是指一切以业务需求为主，技术服务于业务。**完全按照理论的设计，不一定就是最优，还要根据实际情况来决定。**这里就来分析一下不同选择的利与弊。

对于 "quantity * importprice =importvalue"，看起来 "importvalue" 似乎是冗余字段，但并不会导致数据不一致，可是，如果把这个字段取消，是会影响业务的。

因为有的时候，供货商会经常进行一些促销活动，按金额促销，那他们拿来的进货单只有金额，没有价格。而 "importprice" 反而是通过 "importvalue / quantity" 计算出来的，经过四舍五入，会产生较大的误差。这样日积月累，最终会导致查询结果出现较大偏差，影响系统的可靠性。

举例：进货金额（importvalue）是 25.5 元，数量（quantity）是 34，那么进货价格（importprice）就等于 25.5 / 34 = 0.74 元，但是如果用这个计算出来的进货价格（importprice）来计算进货金额，那么，进货金额（importvalue）就等于 0.74 x 34 = 25.16元，其中相差了 25.5 - 25.16 = 0.34 元。

所以，本着业务优先的原则，在不影响系统可靠性的前提下，可适当增加数据冗余，保留 "importvalue"，"importprice" 和 "quantity"。

因此，最终我们可以把进货单表拆分成下面的 4 个表：

- 进货单明细表：

  ![image-20231106201017268](mysql-series-16-database-design-specifications/image-20231106201017268.png)

- 商品信息表：

  ![image-20231106201037491](mysql-series-16-database-design-specifications/image-20231106201037491.png)

- 供货商表：

  ![image-20231106201059846](mysql-series-16-database-design-specifications/image-20231106201059846.png)

- 进货单头表：

  ![image-20231106201118266](mysql-series-16-database-design-specifications/image-20231106201118266.png)

这样一来，我们就避免了冗余，而且还能够满足业务的需求，这样的数据表设计，才是合格的设计。

## ER 模型

数据库设计是牵一发而动全身的。那有没有什么办法提前看到数据库的全貌呢?比如需要哪些数据表、数据表中应该有哪些字段，数据表与数据表之间有什么关系、通过什么字段进行连接，等等。这样才能进行整体的梳理和设计。

其实，ER 模型就是一个这样的工具。ER 模型也叫作实体关系模型，是用来描述现实生活中客观存在的事物、事物的属性，以及事物之间关系的一种数据模型。在开发基于数据库的信息系统的设计阶段，通常使用ER模型来描述信息需求和信息特性，帮助我们理清业务逻辑，从而设计出优秀的数据库。

### ER 模型包括哪些要素

ER 模型中有三个要素，分别是`实体`、`属性`和`关系`。

- `实体`，可以看做是数据对象，往往对应于现实生活中的真实存在的个体。在 ER 模型中，用`矩形`来表示。实体分为两类，分别是强实体和弱实体。强实体是指不依赖于其他实体的实体；弱实体是指对另一个实体有很强的依赖关系的实体。
- `属性`， 则是指实体的特性。比如超市的地址、联系电话、员工数等。在 ER 模型中用`椭圆形`来表示。
- `关系`， 则是指实体之间的联系。比如超市把商品卖给顾客，就是一种超市与顾客之间的联系。在 ER 模型中用`菱形`来表示。

注意：实体和属性不容易区分。这里提供一个原则：要从系统整体的角度出发去看，可以独立存在的是实体，不可再分的是属性。也就是说，属性不能包含其他属性。

### 关系类型

在 ER 模型的 3 个要素中，关系又可以分为 3 种类型，分别是一对一、一对多、多对多。

- `一对一`：指实体之间的关系是一一对应的，比如个人与身份证信息之间的关系就是一对一的关系。一个人只能有一个身份证信息，一个身份证信息也只属于一个人。
- `一对多`∶指一边的实体通过关系，可以对应多个另外一边的实体。相反，另外一边的实体通过这个关系，则只能对应唯一的一边的实体。比如说，新建一个班级表，而每个班级都有多个学生，每个学生则对应一个班级，班级对学生就是一对多的关系。
- `多对多`：指关系两边的实体都可以通过关系对应多个对方的实体。比如在进货模块中，供货商与超市之间的关系就是多对多的关系，一个供货商可以给多个超市供货，一个超市也可以从多个供货商那里采购商品。再比如一个选课表，有许多科目，每个科目有很多学生选，而每个学生又可以选择多个科目，这就是多对多的关系。

### 建模分析

ER 模型看起来比较麻烦，但是对我们把控项目整体非常重要。如果你只是开发一个小应用，或许简单设计几个表够用了，一旦要设计有一定规模的应用，在项目的初始阶段，建立完整的 ER 模型就非常关键了。开发应用项目的实质，其实就是`建模`。

此处设计的案例是电商业务，由于电商业务太过庞大且复杂，所以做了业务简化，比如针对 SKU（StockKeepingUnit，库存量单位）和 SPU（Standard Product Unit，标准化产品单元）的含义上，直接使用了 SKU，并没有提及 SPU 的概念。本次电商业务设计总共有 8 个实体，如下所示：

- 地址实体
- 用户实体
- 购物车实体
- 评论实体
- 商品实体
- 商品分类实体
- 订单实体
- 订单详情实体

其中，用户和商品分类是强实体，因为它们不需要依赖其他任何实体。而其他同于弱实体，因为它们虽然都可以独立存在，但是它们都依赖用户这个实体，因此都是弱实体。知道了这些要素就可以给电商业务创建 ER 模型了，如图：

![image-20231106201758591](mysql-series-16-database-design-specifications/image-20231106201758591.png)

在这个图中，地址和用户之间的添加关系，是一对多的关系，而商品和商品详情示一对一的关系，商品和订单是多对多的关系。 这个 ER 模型，包括了 8 个实体之间的 8 种关系。

1. 用户可以在电商平台添加多个地址。
2. 用户只能拥有一个购物车。
3. 用户可以生成多个订单。
4. 用户可以发表多条评论。
5. 一件商品可以有多条评论。
6. 每一个商品分类包含多种商品。
7. 一个订单可以包含多个商品，一个商品可以在多个订单里。
8. 订单中又包含多个订单详情，因为一个订单中可能包含不同种类的商品。

### ER 模型的细化

有了这个 ER 模型就可以从整体上理解电商的业务了。刚刚的 ER 模型展示了电商业务的框架，但是只包括了订单，地址，用户，购物车，评论，商品，商品分类和订单详情这八个实体，以及它们之间的关系，还不能对应到具体的表，以及表与表之间的关联。需要把属性加上，用椭圆来表示，这样得到的 ER 模型就更加完整了。

因此，我们需要进一步去设计一下这个 ER 模型的各个局部，也就是细化下电商的具体业务流程，然后把它们综合到一起，形成一个完整的 ER 模型。这样可以理清数据库的设计思路。

接下来再分析一下各个实体都有哪些属性，如下所示：

1. 地址实体：包括用户编号、省、市、地区、收件人、联系电话、是否是默认地址。
2. 用户实体：包括用户编号、用户名称、昵称、用户密码、手机号、邮箱、头像、用户级别。
3. 购物车实体：包括购物车编号、用户编号、商品编号、商品数量、图片文件 url。
4. 订单实体：包括订单编号、收货人、收件人电话、总金额、用户编号、付款方式、送货地址、下单时间。
5. 订单详情实体：包括订单详情编号、订单编号、商品名称、商品编号、商品数量。
6. 商品实体：包括商品编号、价格、商品名称、分类编号、是否销售，规格、颜色。
7. 评论实体：包括评论 id、评论内容、评论时间、用户编号、商品编号。
8. 商品分类实体：包括类别编号、类别名称、父类别编号。

这样细分之后就可以重新设计电商业务了，ER 模型如图：

![image-20231106202151621](mysql-series-16-database-design-specifications/image-20231106202151621.png)

### ER 模型图转换成数据表

通过绘制 ER 模型已经理清了业务逻辑，现在就要进行非常重要的一步了：把绘制好的 ER 模型，转换成具体的数据表。下面介绍下转换的原则：

1. 一个实体通常转换成一个数据表；
2. 一个多对多的关系，通常也转换成一个 数据表；
3. 一个 1 对 1，或者 1 对多的关系，往往通过表的外键来表达，而不是设计一个新的数据表；
4. 属性转换成表的字段。

下面结合前面的 ER 模型，具体讲解一下怎么运用这些转换的原则，把 ER 模型转换成具体的数据表，从而把抽象出来的数据模型，落实到具体的数据库设计当中。

**1、一个实体通常转换成一个数据表**

先来看一下强实体转换成数据表：

- 用户实体转换成用户表（user_info）的代码如下所示：

  ```mysql
  CREATE TABLE `user_info`(
    `id` bigint(20)NOT NULL AUTO_INCREMENT COMMENT '编号',
    `user_name` varchar(200)DEFAULT NULL COMMENT '用户名称',
    `nick_name` varchar (200)DEFAULT NULL COMMENT '用户昵称',
    `passwd` varchar (200)DEFAULT NULL COMMENT '用户密码',
    `phone_num` varchar (200) DEFAULT NULL COMMENT '手机号',
    `email` varchar(200) DEFAULT NULL COMMENT '邮箱',
    `head_img` varchar ( 200)DEFAULT NULL COMMENT'头像',
    `user_level` varchar(200) DEFAULT NULL COMMENT '用户级别',
    PRIMARY KEY (id)
  ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 COMMENT='用户表';
  ```

- 商品分类实体转换成商品分类表（base_category），由于商品分类可以有一级分类和二级分类，比如一级分类有家居、手机等等分类，二级分类可以根据手机的一级分类分为手机配件，运营商等，这里我们把商品分类实体规划为两张表，分别是一级分类表和二级分类表，之所以这么规划是因为一级分类和二级分类都是有限的，存储为两张表业务结构更加清晰。

  ```mysql
  # 一级分类表
    CREATE TABLE`base_category1`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
    `name` varchar (10) NOT-NULL COMMENT '分类名称',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='一级分类表';
  
  # 二级分类表
  CREATE TABLE `base_category2`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
    `name` varchar (208) NOT NULL COMMENT '二级分类名称',
    `category1_id` bigint(20) DEFAULT NULL COMMENT '一级分类编号',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='二级分类表';
  ```

- 那么如果规划为—张表呢，表结构如下所示：

  ```mysql
  CREATE TABLE `base_category`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
    `name` varchar (200)NOT NULL COMMENT '分类名不',
    `category_parent_id` bigint(20) DEFAULT NULL COMMENT '父分类编号',
    PRIMARY KEY ( id  ) USING BTREE
  )ENGINE=InnoDB AUTO_INCRENENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT= '分类表';
  ```

  - 如果这样分类的话，那么查询一级分类时候，就需要判断父分类编号是否为空，但是如果插入二级分类的时候也是空，就容易造成业务数据混乱。而且查询二级分类的时候 IS NOT NULL 条件是无法使用到索引的。同时，这样的设计也不符合第二范式（因为父分类编号并不依赖分类编号 ID，因为父分类编号可以有很多数据为 NULL)，所以需要进行表的拆分。因此无论是业务需求还是数据库表的规范来看都应该拆分为两张表。

下面再把弱实体转换成数据表：

- 地址实体转换成地址表（user_address），如下所示：

  ```mysql
  CREATE TABLE `user_address`(
  `id` bigint(20)NOT NULL AUTO_INCREMENT COMMENT '编号',
  'province' varchar (500)DEFAULT NULL COMMENT'省',
  `city` varchar (500) DEFAULT NULL COMMENT '市',
  `user_address` varchar (500) DEFAULT NULL COMMENT '具体地址',
  `user_id bipint(20)` DEFAULT NULL COMMENT '用户id',
  `consignee` varchar( 40) DEFAULT NULL COMMENT '收件人',
  `phone_num ` varchar(40) DEFAULT NULL COMMENT ‘联系方式',
  `is_default` varchar( 1) DEFAULT NULL COMMENT '是否是默认',
  PRIMARY KEY (`id`)
  )ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户地址表';
  ```

- 订单实体转换成订单表（order_info)，如下所示，实际业务中订单的信息会非常多，我们这里做了简化。

  ```mysql
  CREATE TABLE `order_info`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '编号',
    `consignee` varchar (100) DEFAULT NULL COMMENT '收货人',
    `consignee_tel` varchar(20) DEFAULT NULL COMMENT'收件人电话',
    `total_amount` decimal( 10,2)DEFAULT NULL COMMENT '总金额',
    `user_id` bigint(20) DEFAULT NULL COMMENT'用户id',
    `payment_way` varchar(20)DEFAULT NULL COMMENT'付款方式',
    `delivery_address` varchar( 1000) DEFAULT NULL COMMENT'送货地址',
    `create_time` datetime DEFAULT NULL COMMENT'下单时间',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCRENENT=1 DEFAULT CHARSET=utf8 ROW_FORNAT=DYNAMIC COMMENT= '订单表';
  ```

- 订单详情实体转换成订单详情表（order_detail)，如下所示。（用于体现多对多关系的，见下节）：

  ```mysql
  # 订单详情表
  CREATE TABLE `order_detail`(
    `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '订单详情编号',
    `order_id` bigint(20) DEFAULT NULL COMMENT '订单编号',
    `sku_id` bigint(20)DEFAULT NULL COMMENT 'sku_id',
    `sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
    `sku_num` varchar(200) DEFAULT NULL COMMENT '购买个数',
    `create_time` datetime DEFAULT NULL COMMENT'操作时间',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='订单明细表';
  ```

- 购物车实体转换成购物车表（cart_info），如下所示：

  ```mysql
  CREATE TABLE `cart_info`(
    `cart_id` bigint(20)NOT NULL AUTO_INCREMENT COMMENT'编号',
    `user_id` varchar(200) DEFAULT NULL COMMENT'用户id',
    `sku_id` bigint(20)DEFAULT NULL COMMENT 'skuid' ,
    `sku_num` int( 11)DEFAULT NULL COMMENT '数量',
    `img_url` varchar ( 500) DEFAULT NULL COMMENT '图片文件',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCRENENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='购物车表';
  ```

- 评论实体转换成评论表（members），如下所示：

  ```mysql
  CREATE TABLE `sku_comments`(
    `comment_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT'评论编号',
    `user_id` bigin)t (20) DEFAULT NULL COMMENT'用户编号',
    `sku_id` decimal( 10,0) DEFAULT NULI COMMENT '商品编号',
    `comment` varchar(2000)DEFAULT NULL COMMENT '评论内容',
    `create_time` datetime DEFAULT NULL COMMENT '评论时间',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCRENENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMNENT='商品评论表';
  ```

- 商品实体转换成商品表（members），如下所示：

  ```mysql
  CREATE TABLE `sku_info`(
    `sku_id` bigint(20) NOT NULL AUTO_INCREMENT COPMENT'商品编号(itemID)',
     `price` decimal(10,0) DEFAULT NULL COMMENT'价格',
    `sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
    `sku_desc` varchar(2000) DEFAULT NULL COMMENT'商品规格描述',
    `category3_id` bigint(20) DEFAULT NULL COMMENT'三级分类id(冗余)',
    `color` varchar (2000) DEFAULT NULL COMMENT '颜色',
    `is_sale` tinyint(3) NOT NULL DEFAULT '0' CONMMENT'是否销售(1:是0:否)',
    PRIMARY KEY (`id`) USING BTREE
  )ENGINE=InnoDB AUTO_INCRENENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT= '商品表';
  ```

**2、一个多对多的关系转换成一个数据表**

这个 ER 模型中的多对多的关系有 1 个，即商品和订单之间的关系，同品类的商品可以出现在不同的订单中，不同的订单也可以包含同一类型的商品，所以它们之间的关系是多对多。针对这种情况需要设计一个独立的表来表示，这种表一般称为中间表。

我们可以设计一个独立的订单详情表，来代表商品和订单之间的包含关系。这个表关联到 2 个实体，分别是订单、商品。所以，表中必须要包括这 2 个实体转换成的表的主键。除此之外，我们还要包括该关系自有的属性：商品数量，商品下单价格以及商品名称。

```mysql
# 订单详情表
CREATE TABLE `order_detail`(
`id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '订单详情编号',
`order_id` bigint(20)DEFAULT NULL COMMENT '订单编号',
`sku_id` bigint(20) DEFAULT NULL COMMENT 'sku_id ',
`sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
`sku_num` varchar(200)DEFAULT NULL COMMENT '购买个数',
`create_time` datetime DEFAULT NULL COMMENT '操作时间',
PRIMARY KEY (`id`) USING BTREE
)ENGINE=InnoDB AUTO_INCRENENT=1 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='订单明细表';
```

> 公司的订单相关表主要有：order、order_item、sku、spu…，其中 order_detail 相当于 order_item。

**3、通过外键来表达1对多的关系**

在上面的表的设计中，我们可以用外键来表达一对多的关系。比如在商品评论表 sku_comments中，我们分别把 user_id、sku_id 定义成外键，以使用下面的语句设置外键。

```mysql
CONSTRAINT fk_comment_user FOREIGN KEY (user_id) REFERENCES user_info (id)

CONSTRAINT fk_comment_sku FOREIGN KEY (sku_id) REFERENCES sku_info (sku_id)
```

外键约束主要是在数据库层面上保证数据的一致性，但是因为插入和更新数据需要检查外键，理论上性能会有所下降，对性能是负面的影响。

`实际的项目，不建议使用外键。`一方面是降低开发的复杂度（有外键的话主从表类的操作必须先操作主表），另外是有外键在处理数据的时候非常麻烦。在电商平台，由于并发业务量比较大，所以一般不设置外键，以免影响数据库性能。

在应用层面做数据的一致性检查，本来就是一个正常的功能需求。如学生选课的场景，课程肯定不是输入的，而是通过下拉或查找等方式从系统中进行选取，就能够保证是合法的课程 ID，因此就不需要靠数据库的外键来检查了。

**4、把属性转换成表的字段**

在刚刚的设计中，我们也完成了把属性都转换成了表的字段，比如把商品属性转换成了商品信息表中的字段。

```mysql
CREATE TABLE `sku_info`(
  `sku_id` bigint(20) NOT NULL AUTO_INCREMENT COPMENT'商品编号(itemID)',
   `price` decimal(10,0) DEFAULT NULL COMMENT'价格',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'sku名称',
  `sku_desc` varchar(2000) DEFAULT NULL COMMENT'商品规格描述',
  `category3_id` bigint(20) DEFAULT NULL COMMENT'三级分类id(冗余)',
  `color` varchar (2000) DEFAULT NULL COMMENT '颜色',
  `is_sale` tinyint(3) NOT NULL DEFAULT '0' CONMMENT'是否销售(1:是0:否)',
  PRIMARY KEY (`id`) USING BTREE
)ENGINE=InnoDB AUTO_INCRENENT=45 DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT= '商品表';
```

到这里，我们通过创建电商项目业务流程的 ER 模型，再把 ER 模型转换成具体的数据表的过程，完成利用 ER 模型设计电商项目数据库的工作。

其实，任何一个基于数据库的应用项目，都可以通过这种先建立 ER 模型，再转换成数据表的方式，完成数据库的设计工作。创建 ER 模型不是目的，目的是把业务逻辑梳理清楚，设计出优秀的数据库。不是为了建模而建模，要利用创建 ER 模型的过程来整理思路，这样创建 ER 模型才有意义。

![image-20231106205251437](mysql-series-16-database-design-specifications/image-20231106205251437.png)

## 数据表的设计原则

综合以上内容，总结出数据表设计的一般原则：`三少一多`。

1. 数据表的个数越少越好。
   - RDBMS 的核心在于对实体和联系的定义，也就是 E-R 图（Entity Relationship Diagram），数据表越少，证明实体和联系设计得越简洁，既方便理解又方便操作。
2. 数据表中的字段个数越少越好。
   - 字段个数越多，数据冗余的可能性越大。设置字段个数少的前提是各个字段相互独立，而不是某个字段的取值可以由其他字段计算出来。当然字段个数少是相对的，通常会在数据冗余和检索效率中进行平衡。
3. 数据表中联合主键的字段个数越少越好。
   - 设置主键是为了确定唯一性，当一个字段无法确定唯一性的时候，就需要采用联合主键的方式（也就是用多个字段来定义一个主健）。联合主键中的字段越多，占用的索列空间越大，不仅会加大理解难度，还会增加运行时间和索引空间，因此联合主键的字段个数越少越好。
4. 使用主键和外键越多越好。
   - 数据库的设计实际上就是定义各种表，以及各种字段之间的关系。这些关系越多，证明这些实体之间的冗余度越低，利用度越高。这样做的好处在于不仅保证了数据表之间的独立性，还能提升相互之间的关联使用率。
   - 这里的外键指业务上实现外键，也就是逻辑外键。不一定使用外键约束实现。

"三少一多" 原则的核心就是简单可复用。简单指的是用更少的表、更少的字段、更少的联合主键字段来完成数据表的设计。可复用则是通过主键、外键的使用来增强数据表之间的复用率。因为一个主键可以理解是一张表的代表。键设计得越多，证明它们之间的利用率越高。

>注意：这个原则并不是绝对的，有时候我们需要牺牲数据的冗余度来换取数据处理的效率。

## 数据库对象编写建议

前面讲了数据库的设计规范，下面给出的这些规范适用于大多数公司，按照下面的规范来使用数据库，这样数据库可以发挥出更高的性能。

### 关于库

1. 【强制】库的名称必须控制`在 32 个字符以内`，只能使用英文字母、数字和下划线，建议以英文字母开头。
2. 【强制】库名中英文一律`小写`，不同单词采用`下划线分割`，须见名知意。
3. 【强制】库的名称格式：`业务系统名称_子系统名`。
4. 【强制】库名`禁止使用关键字`，如 type，order 等。
5. 【强制】创建数据库时必须显式指定字符集，并且字符集只能是`utf8 或者 utf8mb4`。创建数据库 SQL 举例：CREATE DATABASE crm_fund DEFAULT CHARACTER SET 'utf8'。
6. 【建议】对于程序连接数据库账号，遵循`权限最小原则`。使用数据库账号只能在一个 DB 下使用，不准跨库。程序使用的账号原则上不准有 drop 权限。
7. 【建议】临时库以`tmp_`为前缀，并以日期为后缀；备份库以`bak_`为前缀，并以日期为后缀。

### 关于表、列

1. 【强制】表和列的名称必须控制在 32 个字符以内，表名只能使用英文字母、数字和下划线，建议以英文字母开头。

2. 【强制】 表名、列名一律小写，不同单词采用下划线分割，须见名知意。

3. 【强制】表名要求有模块名强相关，同一模块的表名尽量使用统一前缀。比如：crm_fund_item。

4. 【强制】创建表时必须显式指定字符集为 utf8 或 utf8mb4。

5. 【强制】表名、列名禁止使用关键字，如 type，order 等。

6. 【强制】创建表时必须显式指定表存储引擎类型。如无特殊需求，一律为 InnoDB。

7. 【强制】建表`必须有 comment`。

8. 【强制】字段命名应尽可能使用表达实际含义的英文单词或缩写。如：公司 ID，不要使用 corporation_id，而用 corp_id 即可。

9. 【强制】布尔值类型的字段命名为`is_`描述。如 member 表上表示是否为 enabled 的会员的字段命名为 is_enabled。

10. 【强制】`禁止在数据库中存储图片、文件等大的二进制数据。`通常文件很大，短时间内造成数据量快速增长，数据库进行数据库读取时，通常会进行大量的随机 I/O 操作，文件很大时，I/O 操作很耗时。通常存储于文件服务器（如 FastDFS），数据库只存储文件地址信息。

11. 【建议】建表时关于主键：表必须有主键。

    - 强制要求主键为 id，类型为 int 或 bigint，且为 auto_increment，建议使用 unsigned 无符号型。
    - 标识表里每一行主体的字段不要设为主键，建议设为其他字段如 user_id，order_id 等，并建立 unique key 索引。因为如果设为主键且主键值为随机插入，则会导致 InnoDB 内部页分裂和大量随机 I/O，性能下降。

12. 【建议】核心表（如用户表）必须有行数据的创建时间字段（create_time）和最后更新时间字段（update_time），便于查问题。

13. 【建议】表中所有字段尽量都是 NOT NULL 属性，业务可以根据需要定义 DEFAULT 值。 因为使用 NULL 值会存在每一行都会占用额外存储空间、数据迁移容易出错、聚合函数计算结果偏差、业务代码容易出现空指针等问题。

14. 【建议】所有存储相同数据的列名和列类型必须一致（一般作为关联列，如果查询时关联列类型不一致会自动进行数据类型隐式转换，会造成列上的索引失效，导致查询效率降低）。

15. 【建议】中间表（或临时表）用于保留中间结果集，名称以`tmp_`开头。备份表用于备份或抓取源表快照，名称以`bak_`开头。中间表和备份表定期清理。

16. 【示范】一个较为规范的建表语句：

    ```mysql
    CREATE TABLE user_info (
      `id` int unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
      `user_id` bigint(11) NOT NULL COMMENT '用户id',
      `username` varchar(45) NOT NULL COMMENT '真实姓名',
      `email` varchar(30) NOT NULL COMMENT '用户邮箱',
      `nickname` varchar(45) NOT NULL COMMENT '昵称',
      `birthday` date NOT NULL COMMENT '生日',
      `sex` tinyint(4) DEFAULT '0' COMMENT '性别',
      `short_introduce` varchar(150) DEFAULT NULL COMMENT '一句话介绍自己，最多50个汉字',
      `user_resume` varchar(300) NOT NULL COMMENT '用户提交的简历存放地址',
      `user_register_ip` int NOT NULL COMMENT '用户注册时的源ip',
      `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
      `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
      `user_review_status` tinyint NOT NULL COMMENT '用户资料审核状态，1为通过，2为审核中，3为未通过，4为还未提交审核',
      PRIMARY KEY (`id`),
      UNIQUE KEY `uniq_user_id` (`user_id`),
      KEY `idx_username`(`username`),
      KEY `idx_create_time_status`(`create_time`,`user_review_status`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='网站用户基本信息';
    ```

17. 【建议】创建表时，可以使用可视化工具。这样可以确保表、字段相关的约定都能设置上。实际上，我们通常很少自己写 DDL 语句，可以使用一些可视化工具来创建和操作数据库和数据表。可视化工具除了方便，还能直接帮我们将数据库的结构定义转化成 SQL 语言，方便数据库和数据表结构的导出和导入。

### 关于索引

1. 【强制】InnoDB 表必须主键为 id int/bigint auto_increment，且主键值禁止被更新。
2. 【强制】InnoDB 和 MyISAM 存储引擎表，索引类型必须为 BTREE（此处是官方写法，代表的其实就是 B+Tree）。
3. 【建议】主键的名称以`pk_`开头，唯一键以`uni_`或`uk_`开头，普通索引以`idx_`开头，一律使用小写格式，以字段的名称或缩写作为后缀。
4. 【建议】多单词组成的 columnname，取前几个单词首字母，加末单词组成 column_name。如 sample 表 member_id 上的索引：idx_sample_mid。
5. 【建议】单个表上的索引个数不能超过 6 个。
6. 【建议】在建立索引时，多考虑建立联合索引，并把区分度最高的字段放在最前面。
7. 【建议】在多表 JOIN 的 SQL 里，保证被驱动表的连接列上有索引，这样 JOIN 执行效率最高。
8. 【建议】建表或加索引时，保证表里互相不存在冗余索引。 比如：如果表里已经存在 key(a, b)， 则 key(a) 为冗余索引，需要删除。

### SQL 编写

1. 【强制】程序端 SELECT 语句必须指定具体字段名称，禁止写成 *。
2. 【建议】程序端 INSERT 语句指定具体字段名称，不要写成 INSERT INTO t1 VALUES(…)。
3. 【建议】除静态表或小表（100 行以内），DML 语句必须有 WHERE 条件，且使用索引查找。
4. 【建议】INSERT INTO…VALUES(XX), (XX), (XX)…，这里 XX 的值不要超过 5000 个。值过多虽然上线很快，但会引起主从同步延迟。
5. 【建议】SELECT 语句不要使用 UNION，推荐使用 UNION ALL，并且 UNION 子句个数限制在 5 个以内。
6. 【建议】线上环境，多表 JOIN 不要超过 5 个表。
7. 【建议】减少使用 ORDER BY，和业务沟通能不排序就不排序，或将排序放到程序端去做。`ORDER BY、GROUP BY、DISTINCT 这些语句较为耗费 CPU，数据库的 CPU 资源是极其宝贵的。`
8. 【建议】包含了 ORDER BY、GROUP BY、DISTINCT 这些查询的语句，WHERE 条件过滤出来的结果集请保持在 1000 行以内，否则 SQL 会很慢。
9. 【建议】对单表的多次 ALTER 操作必须合并为一次。对于超过 100 W 行的大表进行 ALTER TABLE，必须经过 DBA 审核，并在业务低峰期执行，多个 ALTER 需整合在一起。 因为 ALTER TABLE 会产生表锁，期间阻塞对于该表的所有写入，对于业务可能会产生极大影响。
10. 【建议】批量操作数据时，需要控制事务处理间隔时间，进行必要的 sleep。
11. 【建议】事务里包含 SQL 不超过 5 个。因为过长的事务会导致锁数据较久，MySQL 内部缓存、连接消耗过多等问题。
12. 【建议】事务里更新语句尽量基于主键或 UNIQUE KEY，如 UPDATE… WHERE id = XX，否则会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql-advanced.md