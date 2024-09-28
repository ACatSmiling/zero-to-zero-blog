>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-20`*

## 存储过程与函数

MySQL 从 5.0 版本开始支持存储过程和函数。存储过程和函数能够将复杂的 SQL 逻辑封装在一起，应用程序无须关注存储过程和函数内部复杂的 SQL 逻辑，而只需要简单地调用存储过程和函数即可。

### 存储过程概述

**含义：**存储过程的英文是 Stored Procedure 。它的思想很简单，就是一组经过`预先编译`的 SQL 语句的封装。

**执行过程：**存储过程预先存储在 MySQL 服务器上，需要执行的时候，客户端只需要向服务器端发出调用存储过程的命令，服务器端就可以把预先存储好的这一系列 SQL 语句全部执行。

**好处：**

- 简化操作，提高了 SQL 语句的重用性，减少了开发程序员的压力。
- 减少操作过程中的失误，提高效率。
- 减少网络传输量（客户端不需要把所有的 SQL 语句通过网络发给服务器） 。
- 减少了 SQL 语句暴露在网上的风险，也提高了数据查询的安全性。

**和视图、函数的对比：**

- 它和视图有着同样的优点，清晰、安全，还可以减少网络传输量。不过它和视图不同，`视图是虚拟表`，通常不对底层数据表直接操作，而`存储过程是程序化的 SQL`，可以直接操作底层数据表，相比于面向集合的操作方式，能够实现一些更复杂的数据处理。
- 一旦存储过程被创建出来，使用它就像使用函数一样简单，直接通过调用存储过程名即可。**`相较于函数，存储过程是没有返回值的。`**

`存储过程的参数类型可以是 IN、OUT 和 INOUT`。根据这点分类如下：

- 没有参数（无参数无返回）。
- 仅仅带 IN 类型（有参数无返回） 。
- 仅仅带 OUT 类型（无参数有返回） 。
- 既带 IN 又带 OUT（有参数有返。回）。
- 带 INOUT（有参数有返回）
- 注意：IN、OUT、INOUT 都可以在一个存储过程中带多个。

### 创建存储过程

语法：

```mysql
CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名 参数类型,...)
[characteristics ...]
    BEGIN
        存储过程体
    END
```

- 参数前面的符号的意思：

  - `IN`：当前参数为输入参数，也就是表示入参；存储过程只是读取这个参数的值。**如果没有定义参数种类， 默认就是 IN ，表示输入参数。**
  - `OUT`：当前参数为输出参数，也就是表示出参；执行完成之后，调用这个存储过程的客户端或者应用程序就可以读取这个参数返回的值了。
  - `INOUT`：当前参数既可以为输入参数，也可以为输出参数。

- 形参类型可以是 MySQL 数据库中的任意类型。

- characteristics 表示创建存储过程时指定的对存储过程的约束条件，其取值信息如下：

  ```mysql
  LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'
  ```

  - `LANGUAGE SQL`：说明存储过程执行体是由 SQL 语句组成的，当前系统支持的语言为 SQL。
  - `[NOT] DETERMINISTIC`：指明存储过程执行的结果是否确定。DETERMINISTIC 表示结果是确定的，每次执行存储过程时，相同的输入会得到相同的输出。NOT DETERMINISTIC 表示结果是不确定的，相同的输入可能得到不同的输出。如果没有指定任意一个值，默认为 NOT DETERMINISTIC。
  - `{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }`：指明子程序使用SQL语句的限制。
    - **CONTAINS SQL**：表示当前存储过程的子程序包含 SQL 语句，但是并不包含读写数据的 SQL 语句；
    - **NO SQL**：表示当前存储过程的子程序中不包含任何 SQL 语句；
    - **READS SQL DATA**：表示当前存储过程的子程序中包含读数据的 SQL 语句；
    - **MODIFIES SQL DATA**：表示当前存储过程的子程序中包含写数据的 SQL 语句。
    - 默认情况下，系统会指定为 CONTAINS SQL。
  - `SQL SECURITY { DEFINER | INVOKER }`：执行当前存储过程的权限，即指明哪些用户能够执行当前存储过程。
    - **DEFINER**：表示只有当前存储过程的创建者或者定义者才能执行当前存储过程；
    - **INVOKER**：表示拥有当前存储过程的访问权限的用户能够执行当前存储过程。
    - 如果没有设置相关的值，则 MySQL 默认指定值为 DEFINER。
  - `COMMENT 'string'`：注释信息，可以用来描述存储过程。

- 存储过程体中可以有多条 SQL 语句，如果仅仅一条 SQL 语句，则可以省略 BEGIN 和 END。编写存储过程并不是一件简单的事情，可能存储过程中需要复杂的 SQL 语句。

  ```mysql
  BEGIN…END：BEGIN…END 中间包含了多个语句，每个语句都以 ';' 号为结束符
  DECLARE：DECLARE 用来声明变量，使用的位置在于 BEGIN…END 语句中间，而且需要在其他语句使用之前进行变量的声明
  SET：赋值语句，用于对变量进行赋值
  SELECT… INTO：把从数据表中查询的结果存放到变量中，也就是为变量赋值
  ```

- 需要设置新的结束标记。

  ```mysql
  DELIMITER 新的结束标记
  ```

  ```mysql
  # 示例
  DELIMITER $
  
  CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名 参数类型,...)
  [characteristics ...]
      BEGIN
          sql语句1;
          sql语句2;
      END $
  
  DELIMITER ;
  ```

  - MySQL 默认的语句结束符号为分号 ";"，为了避免与存储过程中 SQL 语句结束符相冲突，需要使用 DELIMITER 重新设置存储过程的结束符。比如："DELIMITER //" 语句的作用是将 MySQL 的结束符设置为 //，并以 "END //" 结束存储过程。存储过程定义完毕之后再使用 "DELIMITER ;" 恢复默认结束符。
  - DELIMITER 也可以指定其他符号作为结束符。当使用 DELIMITER 命令时，应该避免使用反斜杠 "\\" 字符，因为反斜线是 MySQL 的转义字符。

示例：

```mysql
# 创建存储过程 select_all_data()，查看 employees 表的所有数据
DELIMITER $

CREATE PROCEDURE select_all_data()
    BEGIN
        SELECT * FROM employees;
    END $

DELIMITER ;

# 创建存储过程 avg_employee_salary()，返回所有员工的平均工资
DELIMITER //

CREATE PROCEDURE avg_employee_salary()
    BEGIN
        SELECT AVG(salary) AS avg_salary FROM employees;
    END //

DELIMITER ;

# 创建存储过程 show_max_salary()，用来查看 employees 表的最高薪资值
DELIMITER //

CREATE PROCEDURE show_max_salary()
    LANGUAGE SQL
    NOT DETERMINISTIC
    CONTAINS SQL
    SQL SECURITY DEFINER
    COMMENT '查看最高薪资'
    BEGIN
        SELECT MAX(salary) FROM employees;
    END //

DELIMITER ;

# 创建存储过程 show_min_salary()，查看 employees 表的最低薪资值，并将最低薪资通过 OUT 参数 ms 输出
DELIMITER //

CREATE PROCEDURE show_min_salary(OUT ms DOUBLE)
    BEGIN
    	SELECT MIN(salary) INTO ms FROM employees;
    END //

DELIMITER ;

# 创建存储过程 show_someone_salary()，查看 employees 表的某个员工的薪资，并用 IN 参数 empname 输入员工姓名
DELIMITER //

CREATE PROCEDURE show_someone_salary(IN empname VARCHAR(20))
    BEGIN
    	SELECT salary FROM employees WHERE ename = empname;
    END //
    
DELIMITER ;

# 创建存储过程 show_someone_salary2()，查看 employees 表的某个员工的薪资，并用 IN 参数 empname 输入员工姓名，用 OUT 参数 empsalary 输出员工薪资
DELIMITER //

    CREATE PROCEDURE show_someone_salary2(IN empname VARCHAR(20), OUT empsalary DOUBLE)
    BEGIN
    	SELECT salary INTO empsalary FROM emps WHERE ename = empname;
    END //
    
DELIMITER ;

# 创建存储过程 show_mgr_name()，查询某个员工领导的姓名，并用 INOUT 参数 empname 输入员工姓名，输出领导的姓名
DELIMITER //

CREATE PROCEDURE show_mgr_name(INOUT empname VARCHAR(20))
    BEGIN
    	SELECT ename INTO empname FROM emps
    	WHERE eid = (SELECT MID FROM emps WHERE ename = empname);
    END //
    
DELIMITER ;
```

### 调用存储过程

存储过程有多种调用方法。存储过程必须`使用 CALL 语句调用`，并且存储过程和数据库相关，如果要执行其他数据库中的存储过程，需要指定数据库名称，例如 CALL dbname.procname。

语法：

```mysql
CALL 存储过程名(实参列表);
```

- 调用 IN 模式的参数：

  ```mysql
  CALL sp1('值');
  ```

- 调用 OUT 模式的参数：

  ```mysql
  SET @name;
  CALL sp1(@name);
  SELECT @name;
  ```

- 调用 INOUT 模式的参数：

  ```mysql
  SET @name = 值;
  CALL sp1(@name);
  SELECT @name;
  ```

示例：

```mysql
# 创建存储过程，实现累加运算
mysql> DELIMITER //
mysql> CREATE PROCEDURE `add_num`(IN n INT)
    -> 	BEGIN
    -> 		DECLARE i INT;
    -> 		DECLARE sum INT;
    -> 		SET i = 1;
    -> 		SET sum = 0;
    -> 		WHILE i <= n DO
    -> 			SET sum = sum + i;
    -> 			SET i = i +1;
    -> 		END WHILE;
    -> 		SELECT sum;
    -> 	END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
mysql> CALL add_num(50); 
+------+
| sum  |
+------+
| 1275 |
+------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```

### 存储函数的使用

语法：

```mysql
CREATE FUNCTION 函数名(参数名 参数类型,...)
RETURNS 返回值类型
[characteristics ...]
    BEGIN
        函数体 # 函数体中肯定有 RETURN 语句
    END
```

- 参数列表：指定参数为 IN、OUT 或 INOUT 只对 PROCEDURE 是合法的，`FUNCTION 中总是默认为 IN 参数`。
- RETURNS type 语句表示函数返回数据的类型；RETURNS 子句只能对 FUNCTION 做指定，对函数而言这是`强制`的。它用来指定函数的返回类型，而且函数体中也必须包含一个 RETURN value 语句。
- characteristic 为创建函数时指定的对函数的约束，取值与创建存储过程时相同。
- 函数体使用 BEGIN…END 来表示 SQL 代码的开始和结束，如果函数体只有一条语句，可以省略 BEGIN…END。

### 调用存储函数

在 MySQL 中，存储函数的使用方法与 MySQL 内部函数的使用方法是一样的。换言之，用户自己定义的存储函数与 MySQL 内部函数是一个性质的。**区别在于，存储函数是用户自己定义的，而内部函数是 MySQL 的开发者定义的。**

语法：

```mysql
SELECT 函数名(实参列表);
```

示例：

```mysql
# 创建存储函数 email_by_name()，参数定义为空，该函数查询 Abel 的 email 并返回，数据类型为字符串型
mysql> DELIMITER //
mysql> CREATE FUNCTION email_by_name()
    -> RETURNS VARCHAR(25)
    -> DETERMINISTIC
    -> CONTAINS SQL
    -> 	BEGIN
    -> 		RETURN (SELECT email FROM employees WHERE last_name = 'Abel');
    -> 	END //
Query OK, 0 rows affected (0.03 sec)

mysql> DELIMITER ;
mysql> SELECT email_by_name();
+-----------------+
| email_by_name() |
+-----------------+
| EABEL           |
+-----------------+
1 row in set (0.00 sec)

# 创建存储函数 email_by_id()，参数传入 emp_id，该函数查询 emp_id 的 email 并返回，数据类型为字符串型
mysql> DELIMITER //
mysql> CREATE FUNCTION email_by_id(emp_id INT)
    -> RETURNS VARCHAR(25)
    -> DETERMINISTIC
    -> CONTAINS SQL
    -> 	BEGIN
    -> 		RETURN (SELECT email FROM employees WHERE employee_id = emp_id);
    -> 	END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;
mysql> SET @emp_id = 102;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT email_by_id(102);
+------------------+
| email_by_id(102) |
+------------------+
| LDEHAAN          |
+------------------+
1 row in set (0.00 sec)

# 创建存储函数 count_by_id()，参数传入 dept_id，该函数查询 dept_id 部门的员工人数并返回，数据类型为整型
mysql> DELIMITER //
mysql> CREATE FUNCTION count_by_id(dept_id INT)
    -> RETURNS INT
    -> LANGUAGE SQL
    -> NOT DETERMINISTIC
    -> READS SQL DATA
    -> SQL SECURITY DEFINER
    -> COMMENT '查询部门平均工资'
    -> 	BEGIN
    -> 		RETURN (SELECT COUNT(*) FROM employees WHERE department_id = dept_id);
    -> 	END //
Query OK, 0 rows affected (0.03 sec)

mysql> DELIMITER ;
mysql> SET @dept_id = 50;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT count_by_id(@dept_id);
+-----------------------+
| count_by_id(@dept_id) |
+-----------------------+
|                    45 |
+-----------------------+
1 row in set (0.00 sec
```

### 存储过程与存储函数对比

|          | 关键字    | 调用语法          | 返回值              | 应用场景                         |
| -------- | --------- | ----------------- | ------------------- | -------------------------------- |
| 存储过程 | PROCEDURE | `CALL 存储过程()` | 理解为有 0 个或多个 | 一般用于更新                     |
| 存储函数 | FUNCTION  | `SELECT 函数()`   | 只能是一个          | 一般用于查询结果为一个值并返回时 |

此外，`存储函数可以放在查询语句中使用，存储过程不行。`反之，存储过程的功能更加强大，包括能够执行对表的操作（比如创建表，删除表等）和事务操作，这些功能是存储函数不具备的。

### 存储过程和存储函数的查看、修改、删除

#### 查看

MySQL 存储了存储过程和函数的状态信息，用户可以使用 SHOW STATUS 语句或 SHOW CREATE 语句来查看，也可直接从系统的 information_schema 数据库中查询。

**方式 1 ：使用`SHOW CREATE`语句查看存储过程和存储函数的创建信息。**

语法：

```mysql
SHOW CREATE {PROCEDURE | FUNCTION} 存储过程名或函数名
```

示例：

```mysql
mysql> SHOW CREATE FUNCTION count_by_id;
+-------------+-----------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
| Function    | sql_mode                                                                                                              | Create Function                                                                                                                                                               | character_set_client | collation_connection | Database Collation |
+-------------+-----------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
| count_by_id | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION | CREATE DEFINER=`root`@`%` FUNCTION `count_by_id`(dept_id INT) RETURNS int
    READS SQL DATA
BEGIN
RETURN (SELECT COUNT(*) FROM employees WHERE department_id = dept_id);
END | utf8mb3              | utf8_general_ci      | utf8mb4_0900_ai_ci |
+-------------+-----------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)
```

**方式 2：使用`SHOW STATUS`语句查看存储过程和存储函数的状态信息。**

语法：

```mysql
SHOW {PROCEDURE | FUNCTION} STATUS [LIKE 'pattern']
```

- 返回子程序的特征，如数据库、名字、类型、创建者及创建和修改日期。
- [LIKE 'pattern']：匹配存储过程或函数的名称，可以省略。当省略不写时，会列出MySQL数据库中存在的所有存储过程或函数的信息。

示例：

```mysql
 mysql> SHOW FUNCTION STATUS LIKE 'count_by_id';
+-----------+-------------+----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| Db        | Name        | Type     | Definer | Modified            | Created             | Security_type | Comment | character_set_client | collation_connection | Database Collation |
+-----------+-------------+----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
| atguigudb | count_by_id | FUNCTION | root@%  | 2023-05-13 05:37:31 | 2023-05-13 05:37:31 | DEFINER       |         | utf8mb3              | utf8_general_ci      | utf8mb4_0900_ai_ci |
+-----------+-------------+----------+---------+---------------------+---------------------+---------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)
```

**方式 3：从 information_schema.Routines 表中查看存储过程和函数的信息。**

语法：

```mysql
SELECT * FROM information_schema.Routines
WHERE ROUTINE_NAME = '存储过程或存储函数名' [AND ROUTINE_TYPE = {'PROCEDURE|FUNCTION'}];
```

>MySQL 中存储过程和函数的信息存储在 information_schema 数据库下的 Routines 表中，可以通过查询该表的记录来查询存储过程和函数的信息。
>
>如果在 MySQL 数据库中存在存储过程和函数名称相同的情况，最好指定 ROUTINE_TYPE 查询条件来指明查询的是存储过程还是存储函数。

示例：

```mysql
mysql> SELECT * FROM information_schema.Routines WHERE ROUTINE_NAME = 'count_by_id' AND ROUTINE_TYPE = 'FUNCTION';
+---------------+-----------------+----------------+--------------+--------------+-----------+--------------------------+------------------------+-------------------+---------------+--------------------+--------------------+----------------+----------------+--------------+----------------------------------------------------------------------------------+---------------+-------------------+-----------------+------------------+-----------------+----------+---------------+---------------------+---------------------+-----------------------------------------------------------------------------------------------------------------------+-----------------+---------+----------------------+----------------------+--------------------+
| SPECIFIC_NAME | ROUTINE_CATALOG | ROUTINE_SCHEMA | ROUTINE_NAME | ROUTINE_TYPE | DATA_TYPE | CHARACTER_MAXIMUM_LENGTH | CHARACTER_OCTET_LENGTH | NUMERIC_PRECISION | NUMERIC_SCALE | DATETIME_PRECISION | CHARACTER_SET_NAME | COLLATION_NAME | DTD_IDENTIFIER | ROUTINE_BODY | ROUTINE_DEFINITION                                                               | EXTERNAL_NAME | EXTERNAL_LANGUAGE | PARAMETER_STYLE | IS_DETERMINISTIC | SQL_DATA_ACCESS | SQL_PATH | SECURITY_TYPE | CREATED             | LAST_ALTERED        | SQL_MODE                                                                                                              | ROUTINE_COMMENT | DEFINER | CHARACTER_SET_CLIENT | COLLATION_CONNECTION | DATABASE_COLLATION |
+---------------+-----------------+----------------+--------------+--------------+-----------+--------------------------+------------------------+-------------------+---------------+--------------------+--------------------+----------------+----------------+--------------+----------------------------------------------------------------------------------+---------------+-------------------+-----------------+------------------+-----------------+----------+---------------+---------------------+---------------------+-----------------------------------------------------------------------------------------------------------------------+-----------------+---------+----------------------+----------------------+--------------------+
| count_by_id   | def             | atguigudb      | count_by_id  | FUNCTION     | int       |                     NULL |                   NULL |                10 |             0 |               NULL | NULL               | NULL           | int            | SQL          | BEGIN
RETURN (SELECT COUNT(*) FROM employees WHERE department_id = dept_id);
END |          NULL | SQL               | SQL             | NO               | READS SQL DATA  |     NULL | DEFINER       | 2023-05-13 05:37:31 | 2023-05-13 05:37:31 | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION |                 | root@%  | utf8mb3              | utf8_general_ci      | utf8mb4_0900_ai_ci |
+---------------+-----------------+----------------+--------------+--------------+-----------+--------------------------+------------------------+-------------------+---------------+--------------------+--------------------+----------------+----------------+--------------+----------------------------------------------------------------------------------+---------------+-------------------+-----------------+------------------+-----------------+----------+---------------+---------------------+---------------------+-----------------------------------------------------------------------------------------------------------------------+-----------------+---------+----------------------+----------------------+--------------------+
1 row in set (0.00 sec)
```

#### 修改

修改存储过程或存储函数，不影响存储过程或存储函数功能，只是修改相关特性。

语法：

```mysql
ALTER {PROCEDURE | FUNCTION} 存储过程或函数的名 [characteristic ...]
```

>修改存储过程使用 ALTER PROCEDURE 语句，修改存储函数使用 ALTER FUNCTION 语句。除此之外，这两个语句的结构是一样的，语句中的所有参数也是一样的。

其中，characteristic 指定存储过程或函数的特性，其取值信息与创建存储过程、函数时的取值信息略有不同。

```mysql
{ CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
| SQL SECURITY { DEFINER | INVOKER }
| COMMENT 'string'
```

- CONTAINS SQL：表示子程序包含 SQL 语句，但不包含读或写数据的语句。
- NO SQL：表示子程序中不包含 SQL 语句。
- READS SQL DATA：表示子程序中包含读数据的语句。
- MODIFIES SQL DATA：表示子程序中包含写数据的语句。
- SQL SECURITY { DEFINER | INVOKER }：指明谁有权限来执行。
  - DEFINER：表示只有定义者自己才能够执行。
  - INVOKER：表示调用者可以执行。
- COMMENT 'string'：表示注释信息。

示例：

```mysql
# 修改存储过程，将读写权限改为 MODIFIES SQL DATA，并指明调用者可以执行
ALTER PROCEDURE CountProc
MODIFIES SQL DATA
SQL SECURITY INVOKER

# 修改存储函数，将读写权限改为 READS SQL DATA，并加上注释信息 'FIND NAME'
ALTER FUNCTION CountProc
READS SQL DATA
COMMENT 'FIND NAME'
```

#### 删除

语法：

```mysql
DROP {PROCEDURE | FUNCTION} [IF EXISTS] 存储过程或函数的名
```

### 存储过程的优缺点

#### 优点

- **存储过程可以一次编译多次使用。**存储过程只在创建时进行编译，之后的使用都不需要重新编译，这就提升了 SQL 的执行效率。
- **可以减少开发工作量。**将代码封装成模块，实际上是编程的核心思想之一，这样可以把复杂的问题拆解成不同的模块，然后模块之间可以重复使用，在减少开发工作量的同时，还能保证代码的结构清晰。
- **存储过程的安全性强。**在设定存储过程的时候可以设置对用户的使用权限，这样就和视图一样具有较强的安全性。
- **可以减少网络传输量。**因为代码封装到存储过程中，每次使用只需要调用存储过程即可，这样就减少了网络传输量。
- **良好的封装性。**在进行相对复杂的数据库操作时，原本需要使用一条一条的 SQL 语句，可能要连接多次数据库才能完成的操作，现在变成了一次存储过程，只需要连接一次即可。

#### 缺点

- **可移植性差。**存储过程不能跨数据库移植，比如在 MySQL、Oracle 和 SQL Server 里编写的存储过程，在换成其他数据库时都需要重新编写。
- **调试困难。**只有少数 DBMS 支持存储过程的调试。对于复杂的存储过程来说，开发和维护都不容易。虽然也有一些第三方工具可以对存储过程进行调试，但要收费。
- **存储过程的版本管理很困难。**比如数据表索引发生变化了，可能会导致存储过程失效。在开发软件的时候往往需要进行版本管理，但是存储过程本身没有版本控制，版本迭代更新的时候很麻烦。
- **不适合高并发的场景。**高并发的场景需要减少数据库的压力，有时数据库会采用分库分表的方式，而且对可扩展性要求很高，在这种情况下，存储过程会变得难以维护， 增加数据库的压力 ，显然就不适用了。

>阿里开发规范
>
>【强制】禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。

## 变量、流程控制与游标

### 变量

在 MySQL 数据库中，变量分为`系统变量`以及`用户自定义变量`。

#### 系统变量

##### 系统变量分类

系统变量由系统定义，不是用户定义，属于`服务器层面`。启动 MySQL 服务，生成 MySQL 服务实例期间，MySQL 将为 MySQL 服务器内存中的系统变量赋值，这些系统变量定义了当前 MySQL 服务实例的属性、特征。这些系统变量的值要么是`编译 MySQL 时参数的默认值`，要么是`配置文件 (例如 my.ini 等) 中的参数值`。

> 可以通过网址 https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html 查看 MySQL 文档的系统变量。

系统变量分为`全局系统变量（需要添加 global 关键字）`以及`会话系统变量（需要添加 session 关键字）`，有时也把全局系统变量简称为全局变量，把会话系统变量称为 local 变量。如果不添加关键字，`默认会话级别`。静态变量（在 MySQL 服务实例运行期间，它们的值不能使用 set 动态修改）属于特殊的全局系统变量。

> **MySQL 8.0 的新特性 — 全局变量的持久化。**
>
> 在 MySQL 数据库中，全局变量可以通过 SET GLOBAL 语句来设置。例如，设置服务器语句超时的限制，可以通过设置系统变量 max_execution_time 来实现：
>
> ```mysql
> SET GLOBAL MAX_EXECUTION_TIME = 2000;
> ```
>
> 使用 SET GLOBAL 语句设置的变量值只会`临时生效`。数据库重启后，服务器又会从 MySQL 配置文件中读取变量的默认值。 MySQL 8.0 版本新增了`SET PERSIST`命令。例如，设置服务器的最大连接数为 1000：
>
> ```mysql
> SET PERSIST GLOBAL max_connections = 1000;
> ```
>
> MySQL 会将该命令的配置保存到数据目录下的`mysqld-auto.cnf`文件中，下次启动时会读取该文件，用其中的配置来覆盖默认的配置文件。
>
> 示例：
>
> ```mysql
> # 查看全局变量 max_connections 的值
> mysql> SHOW VARIABLES LIKE '%max_connections%';
> +------------------------+-------+
> | Variable_name          | Value |
> +------------------------+-------+
> | max_connections        | 1000  |
> | mysqlx_max_connections | 100   |
> +------------------------+-------+
> 2 rows in set (0.00 sec)
> 
> # 设置全局变量 max_connections 的值
> mysql> SET PERSIST max_connections = 1500;
> Query OK, 0 rows affected (0.00 sec)
> 
> # 重启 MySQL 服务器, 再次查询 max_connections 的值
> mysql> SHOW VARIABLES LIKE '%max_connections%';
> +------------------------+-------+
> | Variable_name          | Value |
> +------------------------+-------+
> | max_connections        | 1500  |
> | mysqlx_max_connections | 100   |
> +------------------------+-------+
> 2 rows in set (0.01 sec)
> ```

每一个 MySQL 客户机成功连接 MySQL 服务器后，都会产生与之对应的会话。会话期间，MySQL 服务实例会在 MySQL 服务器内存中生成与该会话对应的会话系统变量，这些会话系统变量的初始值是全局系统变量值的复制。如下图：

![image-20230520211210885](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001609742-39048609.png)

- 全局系统变量针对于所有会话（连接）有效，但`不能跨重启`。
- 会话系统变量仅针对于当前会话（连接）有效。会话期间，当前会话对某个会话系统变量值的修改，不会影响其他会话同一个会话系统变量的值。
- 会话 1 对某个全局系统变量值的修改，会导致会话 2 中同一个全局系统变量值的修改。
- MySQL 中，有些系统变量只能是全局的，例如 max_connections，用于限制服务器的最大连接数；有些系统变量作用域既可以是全局又可以是会话，例如 character_set_client，用于设置客户端的字符集；有些系统变量的作用域只能是当前会话，例如 pseudo_thread_id，用于标记当前会话的 MySQL 连接 ID。

##### 查看系统变量

**查看所有或部分系统变量：**

```mysql
# 查看所有全局变量
SHOW GLOBAL VARIABLES

# 查看所有会话变量
SHOW SESSION VARIABLES
# 或
SHOW VARIABLES

# 查看满足条件的部分全局变量
SHOW GLOBAL VARIABLES LIKE '%标识符%'

# 查看满足条件的部分会话变量
SHOW SESSION VARIABLES LIKE '%标识符%'
```

示例：

```mysql
mysql> SHOW GLOBAL VARIABLES LIKE 'admin_%';
+------------------------+-----------------+
| Variable_name          | Value           |
+------------------------+-----------------+
| admin_address          |                 |
| admin_port             | 33062           |
| admin_ssl_ca           |                 |
| admin_ssl_capath       |                 |
| admin_ssl_cert         |                 |
| admin_ssl_cipher       |                 |
| admin_ssl_crl          |                 |
| admin_ssl_crlpath      |                 |
| admin_ssl_key          |                 |
| admin_tls_ciphersuites |                 |
| admin_tls_version      | TLSv1.2,TLSv1.3 |
+------------------------+-----------------+
11 rows in set (0.01 sec)

mysql> SHOW SESSION VARIABLES LIKE 'admin_%';
+------------------------+-----------------+
| Variable_name          | Value           |
+------------------------+-----------------+
| admin_address          |                 |
| admin_port             | 33062           |
| admin_ssl_ca           |                 |
| admin_ssl_capath       |                 |
| admin_ssl_cert         |                 |
| admin_ssl_cipher       |                 |
| admin_ssl_crl          |                 |
| admin_ssl_crlpath      |                 |
| admin_ssl_key          |                 |
| admin_tls_ciphersuites |                 |
| admin_tls_version      | TLSv1.2,TLSv1.3 |
+------------------------+-----------------+
11 rows in set (0.00 sec)
```

**查看指定系统变量：**

作为 MySQL 编码规范，MySQL 中的`系统变量以 @@ 开头`，其中`@@global.`仅用于标记全局系统变量，`@@session.`仅用于标记会话系统变量。@@ 首先标记会话系统变量，如果会话系统变量不存在，则标记全局系统变量。

```mysql
# 查看指定的系统变量的值
SELECT @@global.变量名

# 查看指定的会话变量的值
SELECT @@session.变量名
# 或者
SELECT @@变量名
```

**修改系统变量的值：**

有些时候，数据库管理员需要修改系统变量的默认值，以便修改当前会话或者 MySQL 服务实例的属性、特征。具体方法：

方式 1：`修改 MySQL 配置文件`，继而修改 MySQL 系统变量的值（该方法`需要重启 MySQL 服务`）。
方式 2：在 MySQL 服务运行期间，`使用 set 命令`重新设置系统变量的值。

```mysql
# 为某个全局变量赋值
SET @@global.变量名 = 变量值
# 或者
SET GLOBAL 变量名 = 变量值

# 为某个会话变量赋值
SET @@session.变量名 = 变量值
# 或者
SET SESSION 变量名 = 变量值
```

示例：

```mysql
mysql> SELECT @@global.autocommit;
+---------------------+
| @@global.autocommit |
+---------------------+
|                   1 |
+---------------------+
1 row in set (0.01 sec)

mysql> SET GLOBAL autocommit = 0;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @@global.autocommit;
+---------------------+
| @@global.autocommit |
+---------------------+
|                   0 |
+---------------------+
1 row in set (0.00 sec)


mysql> SELECT @@global.max_connections;
+--------------------------+
| @@global.max_connections |
+--------------------------+
|                      151 |
+--------------------------+
1 row in set (0.00 sec)

mysql> SET GLOBAL max_connections = 1000;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @@global.max_connections;
+--------------------------+
| @@global.max_connections |
+--------------------------+
|                     1000 |
+--------------------------+
1 row in set (0.00 sec)
```

#### 用户变量

##### 用户变量分类

用户变量是用户自己定义的，作为 MySQL 编码规范，MySQL 中的`用户变量以 @ 开头`。根据作用范围不同，又分为`会话用户变量`和`局部变量`。

- 会话用户变量：作用域和会话变量一样，`只对当前连接会话有效`。
- 局部变量：`只在 BEGIN 和 END 语句块中有效`，局部变量`只能在存储过程和函数中使用`。

##### 会话用户变量

定义会话用户变量：

```mysql
# 方式一：'=' 或 ':='
SET @用户变量 = 值
SET @用户变量 := 值

# 方式二：':=' 或 INTO 关键字
SELECT @用户变量 := 表达式 [FROM 等子句]
SELECT 表达式 INTO @用户变量 [FROM 等子句]
```

查看用户变量的值 （查看、比较、运算等）:

```mysql
SELECT @用户变量
```

示例：

```mysql
mysql> SET @a = 1;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @a;
+------+
| @a   |
+------+
|    1 |
+------+
1 row in set (0.00 sec)


mysql> SELECT @num := COUNT(*) FROM employees;
+------------------+
| @num := COUNT(*) |
+------------------+
|              107 |
+------------------+
1 row in set, 1 warning (0.00 sec)

mysql> SELECT @num;
+------+
| @num |
+------+
|  107 |
+------+
1 row in set (0.00 sec)


mysql> SELECT AVG(salary) INTO @avgsalary FROM employees;
Query OK, 1 row affected (0.00 sec)

mysql> SELECT @avgsalary;
+-------------------+
| @avgsalary        |
+-------------------+
| 6461.682242990654 |
+-------------------+
1 row in set (0.00 sec)


# 查看某个未声明的变量时，将得到 NULL 值
mysql> SELECT @big; 
+------------+
| @big       |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)
```

##### 局部变量

定义：可以使用`DECLARE`语句定义一个局部变量。

作用域：仅仅在定义它的 BEGIN ... END 中有效。

位置：只能放在 BEGIN ... END 中，而且只能放在第一句。

```mysql
BEGIN
    # 声明局部变量
    DECLARE 变量名1 变量数据类型 [DEFAULT 变量默认值];
    DECLARE 变量名2, 变量名3, ... 变量数据类型 [DEFAULT 变量默认值];
    
    # 为局部变量赋值
    SET 变量名1 = 值;
    SELECT 值 INTO 变量名2 [FROM 子句];
    
    # 查看局部变量的值
    SELECT 变量1, 变量2, 变量3;
END
```

**定义变量：**

```mysql
# 如果没有 DEFAULT 子句，初始值为 NULL
DECLARE 变量名 类型 [default 值]
```

**变量赋值：**

```mysql
# 方式一：一般用于赋简单的值
SET 变量名 = 值;
SET 变量名 := 值;

# 方式二：一般用于赋表中的字段值
SELECT 字段名或表达式 INTO 变量名 FROM 表;
```

**使用变量（查看、比较、运算等）:**

```mysql
SELECT 局部变量名;
```

##### 会话用户变量 VS 局部变量

![image-20230521005741612](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001609402-1743005600.png)

### 定义条件与处理程序

`定义条件`是事先定义程序执行过程中可能遇到的问题，`处理程序`定义了在遇到问题时应当采取的处理方式，并且保证存储过程或函数在遇到警告或错误时能继续执行。这样可以增强存储程序处理问题的能力，避免程序异常停止运行。

> 说明：定义条件和处理程序在存储过程、存储函数中都是支持的。

##### 案例分析

创建一个名称为 UpdateDataNoCondition 的存储过程：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE UpdateDataNoCondition()
    -> BEGIN
    -> SET @x = 1;
    -> UPDATE employees SET email = NULL WHERE last_name = 'Abel';
    -> SET @x = 2;
    -> UPDATE employees SET email = 'aabbel' WHERE last_name = 'Abel';
    -> SET @x = 3;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
```

调用存储过程：

```mysql
mysql> CALL UpdateDataNoCondition();
ERROR 1048 (23000): Column 'email' cannot be null

mysql> SELECT @x;
+------+
| @x   |
+------+
|    1 |
+------+
1 row in set (0.00 sec)
```

可以看到，此时 @x 变量的值为 1。结合创建存储过程的 SQL 语句代码可以得出：在存储过程中未定义条件和处理程序，且当存储过程中执行的 SQL 语句报错时，MySQL 数据库会抛出错误，并退出当前 SQL 逻辑，不再向下继续执行。

##### 定义条件

定义条件就是给 MySQL 中的错误码命名，这有助于存储的程序代码更清晰。它将一个`错误名字`和`指定的错误条件`关联起来。这个名字可以随后被用在定义处理程序的 DECLARE HANDLER 语句中。

定义条件使用 DECLARE 语句，语法：

```mysql
DECLARE 错误名称 CONDITION FOR 错误码(或错误条件)
```

错误码说明：

- `MySQL_error_code`和`sqlstate_value`都可以表示 MySQL 的错误。
  - MySQL_error_code 是数值类型错误代码。
  - sqlstate_value 是长度为 5 的字符串类型错误代码。
- 例如，在 ERROR 1418 (HY000) 中，1418 是 MySQL_error_code，'HY000' 是 sqlstate_value。
- 例如，在 ERROR 1142 (42000) 中，1142 是 MySQL_error_code，'42000' 是 sqlstate_value。

示例：

```mysql
# 定义 'Field_Not_Be_NULL' 错误名，与 MySQL 中违反非空约束的错误类型 'ERROR 1048 (23000)' 对应
# 使用 MySQL_error_code
DECLARE Field_Not_Be_NULL CONDITION FOR 1048;
# 使用 sqlstate_value
DECLARE Field_Not_Be_NULL CONDITION FOR SQLSTATE '23000';

# 定义 'ERROR 1148(42000)' 错误，名称为 command_not_allowed
# 使用 MySQL_error_code
DECLARE command_not_allowed CONDITION FOR 1148;
# 使用 sqlstate_value
DECLARE command_not_allowed CONDITION FOR SQLSTATE '42000';
```

##### 定义处理程序

可以为 SQL 执行过程中发生的某种类型的错误定义特殊的处理程序。

定义处理程序使用 DECLARE 语句，语法：

```mysql
DECLARE 处理方式 HANDLER FOR 错误类型 处理语句
```

**处理方式：**有 3 个取值，CONTINUE、EXIT、UNDO。

- CONTINUE：表示遇到错误不处理，继续执行。
- EXIT：表示遇到错误马上退出。
- UNDO：表示遇到错误后撤回之前的操作，MySQL 中暂时不支持这样的操作。

**错误类型：**即条件，可以有如下取值。

- SQLSTATE '字符串错误码'：表示长度为 5 的 sqlstate_value 类型的错误代码。
- MySQL_error_code：匹配数值类型错误代码。
- 错误名称：表示 DECLARE ... CONDITION 定义的错误条件名称。

- SQLWARNING：匹配所有以 01 开头的 SQLSTATE 错误代码。
- NOT FOUND：匹配所有以 02 开头的 SQLSTATE 错误代码。
- SQLEXCEPTION：匹配所有没有被 SQLWARNING 或 NOT FOUND 捕获的 SQLSTATE 错误代码。

**处理语句：**如果出现上述条件之一，则采用对应的处理方式，并执行指定的处理语句。语句可以是像 "SET 变量 = 值" 这样的简单语句，也可以是使用 "BEGIN ... END" 编写的复合语句。

定义处理程序的几种方式，代码如下：

```mysql
# 方法一：捕获 sqlstate_value
DECLARE CONTINUE HANDLER FOR SQLSTATE '42S02' SET @info = 'NO_SUCH_TABLE';

# 方法二：捕获 mysql_error_value
DECLARE CONTINUE HANDLER FOR 1146 SET @info = 'NO_SUCH_TABLE';

# 方法三：先定义条件，再调用
DECLARE no_such_table CONDITION FOR 1146;
DECLARE CONTINUE HANDLER FOR NO_SUCH_TABLE SET @info = 'NO_SUCH_TABLE';

# 方法四：使用 SQLWARNING
DECLARE EXIT HANDLER FOR SQLWARNING SET @info = 'ERROR';

# 方法五：使用 NOT FOUND
DECLARE EXIT HANDLER FOR NOT FOUND SET @info = 'NO_SUCH_TABLE';

# 方法六：使用 SQLEXCEPTION
DECLARE EXIT HANDLER FOR SQLEXCEPTION SET @info = 'ERROR';
```

##### 案例解决

在存储过程中，定义处理程序，捕获 sqlstate_value 值，当遇到 MySQL_error_code 值为 1048 时，执行 CONTINUE 操作，并且将 @proc_value 的值设置为 -1。

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE UpdateDataWithCondition()
    -> BEGIN
    -> # 定义处理程序
    -> DECLARE CONTINUE HANDLER FOR 1048 SET @proc_value = -1;
    -> SET @x = 1;
    -> UPDATE employees SET email = NULL WHERE last_name = 'Abel';
    -> SET @x = 2;
    -> UPDATE employees SET email = 'aabbel' WHERE last_name = 'Abel';
    -> SET @x = 3;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
mysql> CALL UpdateDataWithCondition();
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT @x, @proc_value;
+------+-------------+
| @x   | @proc_value |
+------+-------------+
|    3 |          -1 |
+------+-------------+
1 row in set (0.00 sec)
```

### 流程控制

解决复杂问题不可能通过一个 SQL 语句完成，而是需要执行多个 SQL 操作。`流程控制语句`的作用就是控制存储过程中 SQL 语句的执行顺序，是完成复杂操作必不可少的一部分。只要是执行的程序，流程就分为三大类：

- `顺序结构`：程序从上往下依次执行。
- `分支结构`：程序按条件进行选择执行，从两条或多条路径中选择一条执行。
- `循环结构`：程序满足一定条件下，重复执行一组语句。

针对 MySQL 的流程控制语句主要有 3 类。`注意：只能用于存储程序。`

- `条件判断语句`：IF 语句和 CASE 语句。
- `循环语句`：LOOP、WHILE 和 REPEAT 语句。
- `跳转语句`：ITERATE 和 LEAVE 语句。

#### 分支结构之 IF

语法：

```mysql
IF 表达式1 THEN 操作1
[ELSEIF 表达式2 THEN 操作2]……
[ELSE 操作N]
END IF
```

- 根据表达式的结果为 TRUE 或 FALSE 执行相应的语句。"[]" 中的内容是可选的。
- 特点：① 不同的表达式对应不同的操作；② 使用在 BEGIN…END 中。

示例：

```mysql
# 存储过程 update_salary_by_eid1，定义 IN 参数 emp_id，输入员工编号，判断其薪资如果低于 8000 元且入职时间超过 5 年，就涨薪 500 元，否则不变
mysql> DELIMITER //
mysql> CREATE PROCEDURE update_salary_by_eid1(IN emp_id INT)
    -> BEGIN
    -> DECLARE emp_salary DOUBLE;
    -> DECLARE hire_year DOUBLE;
    -> SELECT salary INTO emp_salary FROM employees WHERE employee_id = emp_id;
    -> SELECT DATEDIFF(CURDATE(), hire_date)/365 INTO hire_year
    -> FROM employees WHERE employee_id = emp_id;
    -> IF emp_salary < 8000 AND hire_year > 5
    -> THEN UPDATE employees SET salary = salary + 500 WHERE employee_id = emp_id;
    -> END IF;
    -> END //
Query OK, 0 rows affected (0.00 sec)
mysql> DELIMITER ;

mysql> SELECT employee_id, hire_date, salary FROM employees WHERE employee_id = 105;
+-------------+------------+---------+
| employee_id | hire_date  | salary  |
+-------------+------------+---------+
|         105 | 1997-06-25 | 4800.00 |
+-------------+------------+---------+
1 row in set (0.00 sec)

mysql> CALL update_salary_by_eid1(105);
Query OK, 1 row affected (0.01 sec)

mysql> SELECT employee_id, hire_date, salary FROM employees WHERE employee_id = 105;
+-------------+------------+---------+
| employee_id | hire_date  | salary  |
+-------------+------------+---------+
|         105 | 1997-06-25 | 5300.00 |
+-------------+------------+---------+
1 row in set (0.00 sec)
```

#### 分支结构之 CASE

语法 1：

```mysql
# 情况一：类似 switch
CASE 表达式
WHEN 值1 THEN 结果1或语句1(如果是语句, 需要加分号)
WHEN 值2 THEN 结果2或语句2(如果是语句, 需要加分号)
...
ELSE 结果n或语句n(如果是语句, 需要加分号)
END [case](如果是放在BEGIN...END中, 需要加上case, 如果放在SELECT后面不需要)
```

语法 2：

```mysql
# 情况二：类似多重 if
CASE
WHEN 条件1 THEN 结果1或语句1(如果是语句, 需要加分号)
WHEN 条件2 THEN 结果2或语句2(如果是语句, 需要加分号)
...
ELSE 结果n或语句n(如果是语句, 需要加分号)
END [case](如果是放在BEGIN...END, 中需要加上case, 如果放在SELECT后面不需要)
```

示例：

```mysql
# 语法一
CASE val
WHEN 1 THEN SELECT 'val is 1';
WHEN 2 THEN SELECT 'val is 2';
ELSE SELECT 'val is not 1 or 2';
END CASE;

# 语法二
CASE
WHEN val IS NULL THEN SELECT 'val is null';
WHEN val < 0 THEN SELECT 'val is less than 0';
WHEN val > 0 THEN SELECT 'val is greater than 0';
ELSE SELECT 'val is 0';
END CASE;
```

#### 循环结构之 LOOP

LOOP 循环语句用来重复执行某些语句。LOOP 内的语句一直重复执行直到循环被退出（使用 LEAVE 子句），跳出循环过程。

语法：

```mysql
[loop_label:] LOOP
循环执行的语句
END LOOP [loop_label]
```

- loop_label 表示 LOOP 语句的标注名称，可以省略。

示例：

```mysql
# 使用 LOOP 语句进行循环操作，id 值小于 10 时将重复执行循环过程
DECLARE id INT DEFAULT 0;
add_loop:LOOP
    SET id = id +1;
    IF id >= 10 THEN LEAVE add_loop;
    END IF;
END LOOP add_loop;

# 存储过程 update_salary_loop()，声明 OUT 参数 num，存储过程中实现循环涨薪，薪资涨为原来的 1.1 倍，直到全公司的平均薪资达到 12000 结束
DELIMITER //
CREATE PROCEDURE update_salary_loop(OUT num INT)
BEGIN
    DECLARE avg_salary DOUBLE;
    DECLARE loop_count INT DEFAULT 0;
    SELECT AVG(salary) INTO avg_salary FROM employees;
    label_loop:LOOP
        IF avg_salary >= 12000 THEN LEAVE label_loop;
        END IF;
        UPDATE employees SET salary = salary * 1.1;
        SET loop_count = loop_count + 1;
        SELECT AVG(salary) INTO avg_salary FROM employees;
    END LOOP label_loop;
    SET num = loop_count;
END //
DELIMITER ;
```

#### 循环结构之 WHILE

WHILE 语句创建一个带条件判断的循环过程。WHILE 在执行语句时，先对指定的表达式进行判断，如果为真，就执行循环内的语句，否则退出循环。

语法：

```mysql
[while_label:] WHILE 循环条件 DO
循环体
END WHILE [while_label];
```

- while_label 为 WHILE 语句的标注名称，可以省略；如果循环条件结果为真，WHILE 语句内的语句或语句群被执行，直至循环条件为假，退出循环。

示例：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE test_while()
    -> BEGIN
    -> DECLARE i INT DEFAULT 0;
    -> WHILE i < 10 DO
    -> SET i = i + 1;
    -> END WHILE;
    -> SELECT i;
    -> END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;
mysql> CALL test_while();
+------+
| i    |
+------+
|   10 |
+------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

# 存储过程 update_salary_while()，声明 OUT 参数 num，存储过程中实现循环降薪，薪资降为原来的 90%，直到全公司的平均薪资达到 5000 结束
DELIMITER //
CREATE PROCEDURE update_salary_while(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE ;
    DECLARE while_count INT DEFAULT 0;
    SELECT AVG(salary) INTO avg_sal FROM employees;
    WHILE avg_sal > 5000 DO
        UPDATE employees SET salary = salary * 0.9;
        SET while_count = while_count + 1;
        SELECT AVG(salary) INTO avg_sal FROM employees;
    END WHILE;
    SET num = while_count;
END //
DELIMITER ;
```

#### 循环结构之 REPEAT

REPEAT 语句创建一个带条件判断的循环过程。与 WHILE 循环不同的是，REPEAT 循环首先会执行一次循环，然后在 UNTIL 中进行表达式的判断，如果满足条件就退出，即 END REPEAT；如果条件不满足，则会就继续执行循环，直到满足退出条件为止。

语法：

```mysql
[repeat_label:] REPEAT
循环体的语句
UNTIL 结束循环的条件表达式
END REPEAT [repeat_label]
```

- repeat_label 为 REPEAT 语句的标注名称，可以省略；REPEAT 语句内的语句或语句群被重复，直至 expr_condition 为真。

示例：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE test_repeat()
    -> BEGIN
    -> DECLARE i INT DEFAULT 0;
    -> REPEAT
    -> SET i = i + 1;
    -> UNTIL i >= 10
    -> END REPEAT;
    -> SELECT i;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
mysql> CALL test_repeat();
+------+
| i    |
+------+
|   10 |
+------+
1 row in set (0.01 sec)

Query OK, 0 rows affected (0.01 sec)

# 存储过程 update_salary_repeat()，声明 OUT 参数 num，存储过程中实现循环涨薪，薪资涨为原来的 1.15 倍，直到全公司的平均薪资达到 13000 结束
DELIMITER //
CREATE PROCEDURE update_salary_repeat(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE ;
    DECLARE repeat_count INT DEFAULT 0;
    SELECT AVG(salary) INTO avg_sal FROM employees;
    REPEAT
        UPDATE employees SET salary = salary * 1.15;
        SET repeat_count = repeat_count + 1;
        SELECT AVG(salary) INTO avg_sal FROM employees;
        UNTIL avg_sal >= 13000
    END REPEAT;
    SET num = repeat_count;
END //
DELIMITER ;
```

#### 循环结构对比

- 三种循环都可以省略名称，但如果循环中添加了循环控制语句（LEAVE 或 ITERATE），则必须添加名称。 

- LOOP：一般用于实现简单的 "死" 循环；WHILE：先判断后执行；REPEAT：先执行后判断，无条件至少执行一次。

#### 跳转语句之 LEAVE

LEAVE 语句：可以用在循环语句内，或者以 BEGIN 和 END 包裹起来的程序体内，表示跳出循环或者跳出程序体的操作，可以把 LEAVE 理解为 break。

语法：

```mysql
LEAVE 标记名
```

- label 参数表示循环的标志。LEAVE 和 BEGIN ... END 或循环一起被使用。

示例：

```mysql
# 存储过程 leave_begin()，声明 INT 类型的 IN 参数 num，给 BEGIN...END 加标记名，并在 BEGIN...END 中使用 IF 语句判断 num 参数的值：
# 	如果 num <= 0，则使用 LEAVE 语句退出 BEGIN...END
# 	如果 num = 1，则查询 employees 表的平均薪资
# 	如果 num = 2，则查询 employees 表的最低薪资
# 	如果 num > 2，则查询 employees 表的最高薪资
mysql> DELIMITER //
mysql> CREATE PROCEDURE leave_begin(IN num INT)
    -> begin_label: BEGIN
    -> IF num<=0
    -> THEN LEAVE begin_label;
    -> ELSEIF num=1
    -> THEN SELECT AVG(salary) FROM employees;
    -> ELSEIF num=2
    -> THEN SELECT MIN(salary) FROM employees;
    -> ELSE
    -> SELECT MAX(salary) FROM employees;
    -> END IF;
    -> SELECT COUNT(*) FROM employees;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
mysql> CALL leave_begin(0);
Query OK, 0 rows affected (0.00 sec)

mysql> CALL leave_begin(1);
+-------------+
| AVG(salary) |
+-------------+
| 6466.355140 |
+-------------+
1 row in set (0.00 sec)

+----------+
| COUNT(*) |
+----------+
|      107 |
+----------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> CALL leave_begin(2);
+-------------+
| MIN(salary) |
+-------------+
|     2100.00 |
+-------------+
1 row in set (0.00 sec)

+----------+
| COUNT(*) |
+----------+
|      107 |
+----------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> CALL leave_begin(3);
+-------------+
| MAX(salary) |
+-------------+
|    24000.00 |
+-------------+
1 row in set (0.00 sec)

+----------+
| COUNT(*) |
+----------+
|      107 |
+----------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```

```mysql
# 存储过程 leave_while()，声明 OUT 参数 num，存储过程中使用 WHILE 循环降低薪资为原来薪资的 90%，直到全公司的平均薪资小于等于 10000
DELIMITER //
CREATE PROCEDURE leave_while(OUT num INT)
BEGIN
    DECLARE avg_sal DOUBLE; # 记录平均工资
    DECLARE while_count INT DEFAULT 0; # 记录循环次数
    SELECT AVG(salary) INTO avg_sal FROM employees; # ① 初始化条件
    while_label:WHILE TRUE DO # ② 循环条件
        # ③ 循环体
        IF avg_sal <= 10000 THEN
        LEAVE while_label;
        END IF;
        UPDATE employees SET salary = salary * 0.9;
        SET while_count = while_count + 1;
        # ④ 迭代条件
        SELECT AVG(salary) INTO avg_sal FROM employees;
    END WHILE;
    # 赋值
    SET num = while_count;
END //
DELIMITER ;
```

#### 跳转语句之 ITERATE

ITERATE 语句：只能用在循环语句（LOOP、REPEAT 和 WHILE 语句）内，表示重新开始循环，将执行顺序转到语句段开头处。可以把 ITERATE 理解为 continue。

语法：

```mysql
ITERATE label
```

- label 参数表示循环的标志。ITERATE 语句必须跟在循环标志前面。

示例：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE test_iterate()
    -> BEGIN
    -> DECLARE num INT DEFAULT 0;
    -> my_loop:LOOP
    -> SET num = num + 1;
    -> IF num < 10
    -> THEN ITERATE my_loop;
    -> ELSEIF num > 15
    -> THEN LEAVE my_loop;
    -> END IF;
    -> SELECT 'AABBCC';
    -> END LOOP my_loop;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
mysql> CALL test_iterate();
+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

+--------+
| AABBCC |
+--------+
| AABBCC |
+--------+
1 row in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
```

### 游标

#### 游标是什么

虽然可以通过筛选条件 WHERE 和 HAVING，或者是限定返回记录的关键字 LIMIT 返回一条记录，但是，却无法在结果集中像指针一样，向前定位一条记录、向后定位一条记录，或者是`随意定位到某一条记录`，并对记录的数据进行处理。

这个时候，就可以用到`游标 (或光标)`。游标，提供了一种灵活的操作方式，让我们能够对结果集中的每一条记录进行定位，并对指向的记录中的数据进行操作的数据结构。`游标让 SQL 这种面向集合的语言有了面向过程开发的能力。`

在 SQL 中，游标是一种临时的数据库对象，可以指向存储在数据库表中的数据行指针。这里游标 充当了
指针的作用 ，我们可以通过操作游标来对数据行进行操作。

**MySQL 中游标可以在存储过程和函数中使用。**

#### 游标使用步骤

游标必须在声明处理程序之前被声明，并且变量和条件还必须在声明游标或处理程序之前被声明。

如果想要使用游标，一般需要经历四个步骤。不同的 DBMS 中，使用游标的语法可能略有不同。

**第一步，声明游标。**

在 MySQL 中，使用`DECLARE 关键字声明游标`，语法：

```mysql
DECLARE cursor_name CURSOR FOR select_statement;
```

上述语法适用于 MySQL，SQL Server，DB2 和 MariaDB。如果是用 Oracle 或者 PostgreSQL，需要写成：

```postgresql
DECLARE cursor_name CURSOR IS select_statement;
```

要使用 SELECT 语句来获取数据结果集，而此时还没有开始遍历数据，**select_statement 代表的是 SELECT 语句，返回一个用于创建游标的结果集。**

示例：

```mysql
DECLARE cur_emp CURSOR FOR SELECT employee_id, salary FROM employees;
```

**第二步，打开游标。**

当定义好游标之后，如果想要使用游标，必须先打开游标。打开游标的时候，SELECT 语句的查询结果集就会送到游标工作区，为后面游标的`逐条读取`结果集中的记录做准备。语法：

```mysql
OPEN cursor_name;
```

示例：

```mysql
OPEN cur_emp;
```

**第三步，使用游标（从游标中取得数据）。**

语法：

```mysql
FETCH cursor_name INTO var_name [, var_name] ...
```

作用是使用 cursor_name 这个游标来读取当前行，并且将数据保存到 var_name 这个变量中，然后游标指针指到下一行。如果游标读取的数据行有多个列名，则在 INTO 关键字后面赋值给多个变量名即可。

> 注意：
>
> - `var_name 必须在声明游标之前就定义好。`
>
> - `游标的查询结果集中的字段数，必须跟 INTO 后面的变量数一致。`否则，在存储过程执行的时候，MySQL 会提示错误。

示例：

```mysql
FETCH cur_emp INTO emp_id, emp_sal;
```

**第四步，关闭游标。**

有 OPEN 就会有 CLOSE，也就是打开和关闭游标。**当使用完游标后需要关闭掉该游标，因为游标会占用系统资源 ，如果不及时关闭，游标会一直保持到存储过程结束，影响系统运行的效率。**关闭游标的操作，会释放游标占用的系统资源。

语法：

```mysql
CLOSE cursor_name;
```

关闭游标之后，就不能再检索查询结果中的数据行，如果需要检索只能再次打开游标。

示例：

```mysql
CLOSE cur_emp;
```

#### 游标使用示例

创建存储过程 get_count_by_limit_total_salary()，声明 IN 参数 limit_total_salary，DOUBLE类型；声明 OUT 参数 total_count，INT 类型。存储过程的功能：实现累加薪资最高的几个员工的薪资值，直到薪资总和达到 limit_total_salary 参数的值，返回累加的人数给 total_count。

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE get_count_by_limit_total_salary(IN limit_total_salary DOUBLE,OUT total_count INT)
    -> BEGIN
    -> DECLARE sum_salary DOUBLE DEFAULT 0; # 记录累加的总工资
    -> DECLARE cursor_salary DOUBLE DEFAULT 0; # 记录某一个工资值
    -> DECLARE emp_count INT DEFAULT 0; # 记录循环个数
    -> # 定义游标
    -> DECLARE emp_cursor CURSOR FOR SELECT salary FROM employees ORDER BY salary DESC;
    -> # 打开游标
    -> OPEN emp_cursor;
    -> REPEAT
    -> # 使用游标（从游标中获取数据）
    -> FETCH emp_cursor INTO cursor_salary;
    -> SET sum_salary = sum_salary + cursor_salary;
    -> SET emp_count = emp_count + 1;
    -> UNTIL sum_salary >= limit_total_salary
    -> END REPEAT;
    -> SET total_count = emp_count;
    -> # 关闭游标
    -> CLOSE emp_cursor;
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;

mysql> SET @total_count='';
Query OK, 0 rows affected (0.00 sec)

mysql> CALL get_count_by_limit_total_salary(100000, @total_count);
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @total_count;
+--------------+
| @total_count |
+--------------+
|            7 |
+--------------+

mysql> CALL get_count_by_limit_total_salary(150000, @total_count);
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @total_count;
+--------------+
| @total_count |
+--------------+
|           11 |
+--------------+
1 row in set (0.00 sec)
```

#### 游标总结

游标是 MySQL 的一个重要的功能，为逐条读取结果集中的数据，提供了完美的解决方案。跟在应用层面实现相同的功能相比，游标可以在存储程序中使用，效率高，程序也更加简洁。

但同时也会带来一些性能问题，比如在使用游标的过程中，会对数据行进行`加锁`，这样在业务并发量大的时候，不仅会影响业务之间的效率，还会消耗系统资源，造成内存不足，这是因为`游标是在内存中进行的处理`。

## 触发器

在实际开发中，经常会遇到这样的情况：有 2 个或者多个相互关联的表，如商品信息和库存信息，分别存放在 2 个不同的数据表中，在添加一条新商品记录的时候，为了保证数据的完整性，必须同时在库存表中添加一条库存记录。

这样一来，我们就必须把这两个关联的操作步骤写到程序里面，而且要用事务包裹起来，确保这两个操作成为一个原子操作，要么全部执行，要么全部不执行。要是遇到特殊情况，可能还需要对数据进行手动维护，这样就很容易忘记其中的一步，导致数据缺失。

这个时候，可以使用`触发器`。创建一个触发器，让商品信息数据的插入操作自动触发库存数据的插入操作。这样一来，就不用担心因为忘记添加库存数据而导致的数据缺失了。

### 概述

MySQL 从 5.0.2 版本开始支持触发器。MySQL 的触发器和存储过程一样，都是嵌入到 MySQL 服务器的一段程序。

触发器是由`事件来触发某个操作`，这些事件包括`INSERT`、`UPDATE`、`DELETE`事件。所谓事件就是指用户的动作或者触发某项行为。如果定义了触发程序，当数据库执行这些语句时候，就相当于事件发生了，就会`自动`激发触发器执行相应的操作。

当对数据表中的数据执行插入、更新和删除操作，需要自动执行一些数据库逻辑时，可以使用触发器来实现。

### 触发器的创建

语法：

```mysql
CREATE TRIGGER 触发器名称
{BEFORE|AFTER} {INSERT|UPDATE|DELETE} ON 表名
FOR EACH ROW
触发器执行的语句块;
```

- `表名`：表示触发器监控的对象。
- `BEFORE|AFTER`：表示触发的时间。BEFORE 表示在事件之前触发；AFTER 表示在事件之后触发。
- `INSERT|UPDATE|DELETE`：表示触发的事件。
  - INSERT：表示插入记录时触发；
  - UPDATE：表示更新记录时触发；
  - DELETE：表示删除记录时触发。
- `触发器执行的语句块`：可以是单条 SQL 语句，也可以是由 BEGIN…END 结构组成的复合语句块。

示例：

```mysql
# 创建数据表
mysql> CREATE TABLE test_trigger (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> t_note VARCHAR(30)
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> CREATE TABLE test_trigger_log (
    -> id INT PRIMARY KEY AUTO_INCREMENT,
    -> t_log VARCHAR(30)
    -> );
Query OK, 0 rows affected (0.04 sec)

# 创建名称为 before_insert 的触发器，向 test_trigger 数据表插入数据之前，向 test_trigger_log 数据表中插入 before_insert 的日志信息
mysql> DELIMITER //
mysql> CREATE TRIGGER before_insert
    -> BEFORE INSERT ON test_trigger
    -> FOR EACH ROW
    -> BEGIN
    -> INSERT INTO test_trigger_log (t_log)
    -> VALUES('before_insert');
    -> END //
Query OK, 0 rows affected (0.02 sec)

mysql> DELIMITER ;

# 向 test_trigger 数据表中插入数据
mysql> INSERT INTO test_trigger (t_note) VALUES ('TEST BEFORE INSERT TRIGGER');
Query OK, 1 row affected (0.00 sec)

# 查看 test_trigger_log 数据表中的数据
mysql> SELECT * FROM test_trigger;
+----+----------------------------+
| id | t_note                     |
+----+----------------------------+
|  1 | TEST BEFORE INSERT TRIGGER |
+----+----------------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM test_trigger_log;
+----+---------------+
| id | t_log         |
+----+---------------+
|  1 | before_insert |
+----+---------------+
1 row in set (0.00 sec)
```

```mysql
# 创建名称为 after_insert 的触发器，向 test_trigger 数据表插入数据之后，向 test_trigger_log 数据表中插入 after_insert 的日志信息
mysql> DELIMITER //
mysql> CREATE TRIGGER after_insert
    -> AFTER INSERT ON test_trigger
    -> FOR EACH ROW
    -> BEGIN
    -> INSERT INTO test_trigger_log (t_log)
    -> VALUES('after_insert');
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;

# 向 test_trigger 数据表中插入数据
mysql> INSERT INTO test_trigger (t_note) VALUES ('TEST AFTER INSERT TRIGGER');
Query OK, 1 row affected (0.01 sec)

# 查看 test_trigger_log 数据表中的数据
mysql> SELECT * FROM test_trigger;
+----+----------------------------+
| id | t_note                     |
+----+----------------------------+
|  1 | TEST BEFORE INSERT TRIGGER |
|  2 | TEST AFTER INSERT TRIGGER  |
+----+----------------------------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM test_trigger_log;
+----+---------------+
| id | t_log         |
+----+---------------+
|  1 | before_insert |
|  2 | before_insert |
|  3 | after_insert  |
+----+---------------+
3 rows in set (0.00 sec)

```

### 触发器的查看和删除

#### 查看触发器

查看触发器是查看数据库中已经存在的触发器的定义、状态和语法信息等。

方式 1：查看当前数据库的所有触发器的定义。

```mysql
SHOW TRIGGERS
```

方式 2：查看当前数据库中某个触发器的定义。

```mysql
SHOW CREATE TRIGGER 触发器名
```

方式 3：从系统库 information_schema 的 TRIGGERS 表中查询 salary_check_trigger 触发器的信息。

```mysql
SELECT * FROM information_schema.TRIGGERS;
```

#### 删除触发器

触发器也是数据库对象，删除触发器也用 DROP 语句，语法：

```mysql
DROP TRIGGER IF EXISTS 触发器名称
```

### 触发器的优缺点

#### 优点

**1、触发器可以确保数据的完整性。**

假设用进货单头表（demo.importhead）来保存进货单的总体信息，包括进货单编号、供货商编号、仓库编号、总计进货数量、总计进货金额和验收日期。用进货单明细表（demo.importdetails）来保存进货商品的明细，包括进货单编号、商品编号、进货数量、进货价格和进货金额。

![image-20230521232114165](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001608985-1108613192.png)

每当录入、删除和修改一条进货单明细数据的时候，进货单明细表里的数据就会发生变动。这个时候，在进货单头表中的总计数量和总计金额就必须重新计算，否则，进货单头表中的总计数量和总计金额就不等于进货单明细表中数量合计和金额合计了，这就是数据不一致。

![image-20230521232141988](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001608576-1916747267.png)

为了解决这个问题，可以使用触发器，规定每当进货单明细表有数据插入、修改和删除的操作时，自动触发 2 步操作：

- 1）重新计算进货单明细表中的数量合计和金额合计；
- 2）用第一步中计算出来的值更新进货单头表中的合计数量与合计金额。

这样一来，进货单头表中的合计数量与合计金额的值，就始终与进货单明细表中计算出来的合计数量与合计金额的值相同，数据就是一致的，不会互相矛盾。

**2、触发器可以帮助我们记录操作日志。**
利用触发器，可以具体记录什么时间发生了什么。比如，记录修改会员储值金额的触发器，就是一个很好的例子。这对还原操作执行时的具体场景，更好地定位问题原因很有帮助。

**3、触发器还可以用在操作数据前，对数据进行合法性检查。**

比如，超市进货的时候，需要库管录入进货价格。但是，人为操作很容易犯错误，比如说在录入数量的时候，把条形码扫进去了；录入金额的时候，看串了行，录入的价格远超售价，导致账面上的巨亏……

这些都可以通过触发器，在实际插入或者更新操作之前，对相应的数据进行检查，及时提示错误，防止错误数据进入系统。

#### 缺点

**1、触发器最大的一个问题就是可读性差。**

因为触发器存储在数据库中，并且由事件驱动，这就意味着触发器有可能`不受应用层的控制`，这对系统维护是非常有挑战的。

比如，创建触发器用于修改会员储值操作。如果触发器中的操作出了问题，会导致会员储值金额更新失败。例如：

```mysql
mysql> update demo.membermaster set memberdeposit = 20 where memberid = 2;
ERROR 1054 (42S22): Unknown column 'aa' in 'field list'
```

结果显示，系统提示错误，字段 aa 不存在。这是因为，触发器中的数据插入操作多了一个字段，系统提示错误。可是，如果你不了解这个触发器，很可能会认为是更新语句本身的问题，或者是会员信息表的结构出了问题。说不定你还会给会员信息表添加一个叫 aa 的字段，试图解决这个问题，结果只能是白费力。

**2、相关数据的变更，可能会导致触发器出错。**

特别是数据表结构的变更，都可能会导致触发器出错，进而影响数据操作的正常运行。这些都会由于触发器本身的隐蔽性，影响到应用中错误原因排查的效率。

#### 注意点

注意，如果在子表中定义了外键约束，并且外键指定了 ON UPDATE/DELETE CASCADE/SET NULL 子句，此时修改父表被引用的键值或删除父表被引用的记录行时，也会引起子表的修改和删除操作，此时基于子表的 UPDATE 和 DELETE 语句定义的触发器并不会被激活。

例如：基于子表员工表（t_employee）的 DELETE 语句定义了触发器 t1，而子表的部门编号（did）字段定义了外键约束，引用了父表部门表（t_department）的主键列部门编号（did），并且该外键加了 ON DELETE SET NULL 子句，那么如果此时删除父表部门表（t_department）在子表员工表（t_employee）有匹配记录的部门记录时，会引起子表员工表（t_employee）匹配记录的部门编号（did）修改为NULL。但是，此时不会激活触发器 t1，只有直接对子表员工表（t_employee）执行 DELETE 语句时，才会激活触发器 t1。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql.md