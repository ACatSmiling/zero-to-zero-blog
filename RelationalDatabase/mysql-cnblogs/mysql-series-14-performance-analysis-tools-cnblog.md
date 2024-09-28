>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-21`*

`在数据库调优中，目标就是 响应时间更快，吞吐量更大。`利用宏观的监控工具和微观的日志分析可以帮助快速找到调优的思路和方式。

## 数据库服务器的优化步骤

当遇到数据库调优问题的时候，该如何思考呢？这里把思考的流程整理成下面这张图。

整个流程划分成了` 观察（Show status）`和`行动（Action）`两个部分。字母 S 的部分代表观察（会使用相应的分析工具），字母 A 代表的部分是行动（对应分析可以采取的行动）：

![image-20230711090137783](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001744328-1716363988.png)

数据库调优时，可以通过观察了解数据库整体的运行状态，通过性能分析工具，可以帮助了解执行慢的 SQL 有哪些，查看具体的 SQL 执行计划，甚至是 SQL 执行中的每一步的成本代价，这样才能定位问题所在，找到了问题，再采取相应的行动。

- 首先，在 S1 部分，我们需要观察服务器的状态是否存在周期性的波动。如果`存在周期性波动`，有可能是周期性节点的原因，比如双十一、促销活动等。这样的话，可以通过 A1 这一步骤解决，也就是加缓存，或者更改缓存失效策略。
- 如果缓存策略没有解决，或者不是周期性波动的原因，就需要进一步`分析查询延迟和卡顿的原因`。接下来进入 S2 这一步，需要开`启慢查询`，慢查询可以帮助定位执行慢的 SQL 语句。可以通过设置`long_query_time`参数定义慢查询的阈值，如果 SQL 执行时间超过了 long_query_time，则会认为是慢查询。当收集上来这些慢查询之后，就可以通过分析工具对慢查询日志进行分析。
- 在 S3 这一步骤中，我们知道了执行慢的 SQL，可以针对性的用`EXPLAIN`查看对应 SQL 语句的执行计划，或者用`SHOW PROFILE`查看 SQL 中每一个步骤的时间成本。然后，就可以了解 SQL 查询慢是因为执行时间长，还是等待时间长。
- 如果是 SQL 等待时间长，进入 A2 步骤。在这一步骤中，可以`调优服务器的参数`，比如适当增加数据库缓冲池等。如果是 SQL 执行时间长，就进入 A3 步骤，这一步中需要考虑是索引设计的问题，还是查询关联的数据表过多，亦或者是因为数据表的字段设计问题导致了这一问题，然后在对应的维度上进行相应的调整。
- 如果 A2 和 A3 都不能解决问题，我们需要考虑数据库自身的 SQL 查询性能是否已经达到了瓶颈，如果确认没有达到性能瓶颈，就需要重新检查，重复以上的步骤。如果已经`达到了性能瓶颈`，进入 A4 阶段，需要考虑`增加服务器`，采用`读写分离`的架构，或者考虑对数据库进行`分库分表`，比如垂直分库、垂直分表和水平分表等。

以上就是数据库调优的流程思路，如果发现执行 SQL 时存在不规则延迟或卡顿的时候，就可以采用分析工具帮忙定位有问题的 SQL，这三种分析工具可以理解是 SQL 调优的三个步骤：`慢查询`、`EXPLAIN`和`SHOW PROFILING`。

小结：

![image-20230712090052115](https://img2023.cnblogs.com/blog/3488201/202409/3488201-20240922001743913-191770430.png)

- 可以看到数据库调优的步骤中越往金字塔尖走，其成本越高，效果越差，因此在数据库调优的过程中，要重点把握金字塔底部的 SQL 及索引调优，数据库表结构调优，系统配置参数调优等软件层面的调优。

## 查看系统性能参数

可以使用`SHOW STATUS`语句查询一些数据库服务器的性能参数和使用频率，语法：

```mysql
SHOW [GLOBAL][SESSION] STATUES LIKE '[参数]'
```

一些常用的性能参数如下：

- Connections：连接 MySQL 服务器的次数。
- Uptime：MySQL 服务器的上线时间。
- Slow_queries：慢查询的次数。
- Innodb_rows_read：SELECT 查询返回的行数。
- Innodb_rows_inserted：执行 INSERT 操作插入的行数。
- Innodb_rows_updated：执行 UPDATE 操作更新的行数。
- Innodb_rows_deleted：执行 DELETE 操作删除的行数。
- Com_select：查询操作的次数。
- Com_insert：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。
- Com_update：更新操作的次数。
- Com_delete：删除操作的次数。

## 统计 SQL 的查询成本：last_query_cost

一条 SQL 查询语句在执行前需要确定查询执行计划，如果存在多种执行计划的话，MySQL 会计算每个执行计划所需要的成本，从中选择`成本最小`的一个作为最终执行的执行计划。

如果想要查看某条 SQL 语句的查询成本，可以在执行完这条 SQL 语句之后，通过查看当前会话中的`last_query_cost`变量值来得到当前查询的成本。last_query_cost 也是`评价一个查询的执行效率`的一个常用指标，这个查询成本对应的是`SQL 语句所需要读取的页的数量`。

以 student_info 表为例：

1. 查询 id = 900001 的记录，可以直接在聚簇索引上进行查找。查看查询优化器的成本，实际上只需要检索一个页即可，`Value`表示 I/O 加载的数据页的页数。

   ```mysql
   mysql> SELECT * FROM student_info WHERE id = 900001;
   +--------+------------+--------+-----------+----------+---------------------+
   | id     | student_id | name   | course_id | class_id | create_time         |
   +--------+------------+--------+-----------+----------+---------------------+
   | 900001 |     103954 | BfYzAg |     10064 |    10138 | 2023-07-02 18:54:38 |
   +--------+------------+--------+-----------+----------+---------------------+
   1 row in set (0.00 sec)
   
   mysql> SHOW STATUS LIKE 'last_query_cost';
   +-----------------+----------+
   | Variable_name   | Value    |
   +-----------------+----------+
   | Last_query_cost | 1.000000 |
   +-----------------+----------+
   1 row in set (0.01 sec)
   ```

2. 扩大下查询范围，student_id > 199900 的学生记录，大概需要进行 102120 个页的查询。

   ```mysql
   mysql> SELECT * FROM student_info WHERE student_id > 199900;
   +--------+------------+--------+-----------+----------+---------------------+
   | id     | student_id | name   | course_id | class_id | create_time         |
   +--------+------------+--------+-----------+----------+---------------------+
   |    607 |     199908 | PYxrsM |     10000 |    10080 | 2023-07-02 18:53:47 |
   |   1788 |     199993 | XLQSTN |     10092 |    10070 | 2023-07-02 18:53:47 |
   |   3042 |     199909 | OTcESy |     10023 |    10148 | 2023-07-02 18:53:47 |
   | 999891 |     199922 | mlvtHf |     10087 |    10184 | 2023-07-02 18:54:44 |
   +--------+------------+--------+-----------+----------+---------------------+
   514 rows in set (0.27 sec)
   
   mysql> SHOW STATUS LIKE 'last_query_cost';
   +-----------------+---------------+
   | Variable_name   | Value         |
   +-----------------+---------------+
   | Last_query_cost | 102120.464739 |
   +-----------------+---------------+
   1 row in set (0.00 sec)
   ```

3. 再次扩大查询范围，student_id > 199000 的学生记录，大概需要进行 100512 个页的查询。

   ```mysql
   mysql> SELECT * FROM student_info WHERE student_id > 199000;
   +--------+------------+--------+-----------+----------+---------------------+
   | id     | student_id | name   | course_id | class_id | create_time         |
   +--------+------------+--------+-----------+----------+---------------------+
   |     18 |     199656 | CNfphR |     10058 |    10029 | 2023-07-02 18:53:47 |
   |     30 |     199615 | gEEhXr |     10015 |    10058 | 2023-07-02 18:53:47 |
   |     57 |     199193 | uaUups |     10021 |    10105 | 2023-07-02 18:53:47 |
   | 999891 |     199922 | mlvtHf |     10087 |    10184 | 2023-07-02 18:54:44 |
   +--------+------------+--------+-----------+----------+---------------------+
   5023 rows in set (0.19 sec)
   
   mysql> SHOW STATUS LIKE 'last_query_cost';
   +-----------------+---------------+
   | Variable_name   | Value         |
   +-----------------+---------------+
   | Last_query_cost | 100512.649000 |
   +-----------------+---------------+
   1 row in set (0.00 sec)
   ```

>SQL查询是一个动态的过程，从页加载的角度，我们可以得到以下两点结论：
>
>- `位置决定效率`：如果页就在数据库缓冲池中，那么效率是最高的，否则还需要从内存或者磁盘中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多。即`数据库缓冲池 > 内存 > 磁盘`。
>- `批量决定效率`：如果我们从磁盘中单一页进行随机读，那么效率是很低的（差不多 10 ms），而采用顺序读取的方式，批量对页进行读取，平均一页的读取效率就会提升很多，甚至要快于单个页面在内存中的随机读取。即`顺序读取 > 随机读取`。
>
>所以说，遇到 I/O 并不用担心，方法找对了，效率还是很高的。我们首先要考虑数据存放的位置，如果是经常使用的数据就要尽量放到缓冲池中，其次我们可以充分利用磁盘的吞吐能力，一次性批量读取数据，这样单个页的读取效率也就得到了提升。
>
>注：缓冲池和查询缓存并不是一个东西。

## 定位执行慢的 SQL：慢查询日志

MySQL 的慢查询日志，用来记录在 MySQL 中`响应时间超过阈值`的语句，具体指运行时间超过`long_query_time`值的 SQL，会被记录到慢查询日志中。long_query_time 的默认值是 10，意思是运行 10 秒以上（不含 10 秒）的语句，认为是超出了最大的忍耐时间值。

它的主要作用是，帮助我们发现那些执行时间特别长的 SQL 查询，并且有针对性的进行优化，从而提高系统的整体效率。当我们的数据库服务器发生阻塞、运行变慢的时候，检查一下慢查询日志，找到那些慢查询，对解决问题很有帮助。

默认情况下，MySQL 数据库`没有开启慢查询日志`，需要手动来设置这个参数。**如果不是调优需要的话，一般不建议启动该参数。**因为开启慢查询日志，会或多或少来带一定的性能影响。

> 慢查询日志，支持将日志记录写入文件。

### 开启慢查询日志

#### 开启 slow_query_log

查看慢查询日志是否开启，以及日志的位置：

```mysql
mysql> SHOW VARIABLES LIKE '%slow_query_log%';
+---------------------+--------------------------------------+
| Variable_name       | Value                                |
+---------------------+--------------------------------------+
| slow_query_log      | OFF                                  |
| slow_query_log_file | /var/lib/mysql/de5e82a9b92d-slow.log |
+---------------------+--------------------------------------+
2 rows in set (0.00 sec)
```

修改慢查询日志状态为开启，注意这里要加 `global`，因为它是全局系统变量，否则会报错：

```mysql
mysql> SET GLOBAL slow_query_log='ON';
Query OK, 0 rows affected (0.00 sec)
```

再次查看：

```mysql
mysql> SHOW VARIABLES LIKE '%slow_query_log%';
+---------------------+--------------------------------------+
| Variable_name       | Value                                |
+---------------------+--------------------------------------+
| slow_query_log      | ON                                   |
| slow_query_log_file | /var/lib/mysql/de5e82a9b92d-slow.log |
+---------------------+--------------------------------------+
2 rows in set (0.00 sec)
```

#### 修改 long_query_time 阈值

查看慢查询的时间阈值设置：

```mysql
mysql> SHOW VARIABLES LIKE '%long_query_time%';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set (0.00 sec)
```

按需重新设置，例如设置为 1 秒：

```mysql
# 测试发现设置 global 的方式对当前 session 的 long_query_time 失效，只对新连接的客户端有效，所以可以一并执行下列语句
mysql> SET GLOBAL long_query_time = 1;
Query OK, 0 rows affected (0.00 sec)

mysql> SET long_query_time = 1;
Query OK, 0 rows affected (0.00 sec)
```

#### 补充：配置文件中设置参数

如下的方式相较于前面的命令行方式，可以看作是永久设置。

修改`my.cnf`文件，在 [mysqld] 下增加或修改参数`slow_query_log`，`slow_query_log_file`和`long_query_time`后，然后重启 MySQL 服务器。

```cnf
[mysqld]
slow_query_log=ON # 开启慢查询日志的开关
slow_query_log_file=/var/lib/mysql/slow.log # 慢查询日志的目录和文件名信息
long_query_time=3 # 设置慢查询的阈值为3秒，超出此设定值的SQL即被记录到慢查询日志
log_output=FILE
```

如果不指定存储路径，慢查询日志将默认存储到 MySQL 数据库的数据文件夹下，如果不指定文件名，默认文件名为 hostname-slow.log。

### 案例演示

第一步，建表：

```mysql
mysql> CREATE TABLE `student` (
    ->     `id` INT(11) NOT NULL AUTO_INCREMENT,
    ->     `stuno` INT NOT NULL ,
    ->     `name` VARCHAR(20) DEFAULT NULL,
    ->     `age` INT(3) DEFAULT NULL,
    ->     `classId` INT(11) DEFAULT NULL,
    ->     PRIMARY KEY (`id`)
    -> ) ENGINE=INNODB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
Query OK, 0 rows affected, 4 warnings (0.03 sec)
```

第二步，开启允许创建函数：

```mysql
mysql> SHOW GLOBAL VARIABLES LIKE '%log_bin_trust_function_creators%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | OFF   |
+---------------------------------+-------+
1 row in set (0.01 sec)

mysql> SET GLOBAL log_bin_trust_function_creators='ON';
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW GLOBAL VARIABLES LIKE '%log_bin_trust_function_creators%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | ON    |
+---------------------------------+-------+
1 row in set (0.00 sec)
```

第三步，创建函数：

```mysql
# 随机产生字符串函数 rand_string，同前一章节

# 随机数函数 rand_num，同前一章节
```

第四步，创建存储过程：

```mysql
mysql> DELIMITER //
mysql> CREATE PROCEDURE insert_stu1(  START INT , max_num INT )
    -> BEGIN 
    -> DECLARE i INT DEFAULT 0; 
    -> SET autocommit = 0; # 设置手动提交事务
    -> REPEAT # 循环
    -> SET i = i + 1; # 赋值
    -> INSERT INTO student (stuno, NAME ,age ,classId ) VALUES
    -> ((START+i),rand_string(6),rand_num(10,100),rand_num(10,1000)); 
    -> UNTIL i = max_num 
    -> END REPEAT; 
    -> COMMIT; # 提交事务
    -> END //
Query OK, 0 rows affected (0.01 sec)

mysql> DELIMITER ;
```

第五步，调用存储过程：

```mysql
mysql> CALL insert_stu1(100001,4000000);
Query OK, 0 rows affected (4 min 8.07 sec)
```

第六步，验证是否成功：

```mysql
mysql> SELECT COUNT(*) FROM student;
+----------+
| COUNT(*) |
+----------+
|  4000000 |
+----------+
1 row in set (0.21 sec)
```

### 慢查询演示

执行下面的查询操作，进行慢查询语句的测试：

```mysql
mysql> SELECT * FROM student WHERE stuno = 3455655;
+---------+---------+--------+------+---------+
| id      | stuno   | name   | age  | classId |
+---------+---------+--------+------+---------+
| 3355654 | 3455655 | WOcWyE |   60 |     968 |
+---------+---------+--------+------+---------+
1 row in set (1.21 sec)

mysql> SELECT * FROM student WHERE name = 'ZfCwDz';
+---------+---------+--------+------+---------+
| id      | stuno   | name   | age  | classId |
+---------+---------+--------+------+---------+
|  427220 |  527221 | zfcWDZ |   33 |     311 |
|  781361 |  881362 | ZFcwdZ |   80 |     898 |
|  812704 |  912705 | ZFcwdZ |   81 |     965 |
| 2138264 | 2238265 | zFCWdz |   31 |     727 |
| 2602748 | 2702749 | zFCWdz |   35 |     938 |
| 2763745 | 2863746 | zfcWDZ |   31 |     239 |
| 2978248 | 3078249 | zFCWdz |   30 |     708 |
+---------+---------+--------+------+---------+
7 rows in set (1.26 sec)
```

因为此时慢日志的时间阈值为 1 秒，上面的两条 SQL，都属于慢日志。查看下慢查询的记录：

```mysql
mysql> SHOW STATUS LIKE 'slow_queries';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| Slow_queries  | 3     |
+---------------+-------+
1 row in set (0.00 sec)
```

>在 MySQL 中，除了上述`slow_queries`变量，控制慢查询日志的还有另外一个变量`min_examined_row_limit`。这个变量的意思是，查询扫描过的最少记录数。这个变量和查询执行时间，共同组成了判别一个查询是否慢查询的条件。**如果查询扫描过的记录数大于等于这个变量的值，并且查询执行时间超过 long_query_time 的值，那么这个查询就被记录到慢查询日志中。**反之，则不被记录到慢查询日志中。另外，min_examined_row_limit 默认是 0，我们也一般不会去修改它。
>
>```mysql
>mysql> SHOW VARIABLES LIKE 'min%';
>+------------------------+-------+
>| Variable_name          | Value |
>+------------------------+-------+
>| min_examined_row_limit | 0     |
>+------------------------+-------+
>1 row in set (0.00 sec)
>```
>
>当这个值为默认值 0 时，与 long_query_time=10 合在一起，表示只要查询的执行时间超过 10 秒钟，哪怕一个记录也没有扫描过，都要被记录到慢查询日志中。你也可以根据需要，通过修改 "my.ini" 文件，来修改查询时长，或者通过 SET 指令，用 SQL 语句修改 min_examined_row_limit 的值。

### 慢查询日志分析工具

在生产环境中，如果要手工分析日志，查找、分析 SQL，显然是个体力活，MySQL 提供了日志分析工具`mysqldumpslow`。

>说明：
>
>1. 该工具并不是 MySQL 内置的，不要在 MySQL 下执行，可以直接在根目录或者其他位置执行。
>2. 该工具只有 Linux 下才是开箱可用的，实际上生产中 MySQL 数据库一般也是部署在 Linux 环境中的。如果是 Windows 环境，可以参考 https://www.cnblogs.com/-mrl/p/15770811.html。

通过 mysqldumpslow 可以查看慢查询日志帮助：

```bash
# 创建软连接
xisun@xisun-develop:~/mysql/mysql-8.0.33-linux-glibc2.17-x86_64-minimal/bin$ sudo ln -sf /home/xisun/mysql/mysql-8.0.33-linux-glibc2.17-x86_64-minimal/bin/mysqldumpslow /usr/bin/

# 分析慢日志
xisun@xisun-develop:~/apps$ sudo mysqldumpslow -s t -t 5 mysql/data/de5e82a9b92d-slow.log 

Reading mysql slow query log from mysql/data/de5e82a9b92d-slow.log
Count: 1  Time=248.07s (248s)  Lock=0.00s (0s)  Rows=0.0 (0), root[root]@[127.0.0.1]
  CALL insert_stu1(N,N)

Count: 1  Time=1.26s (1s)  Lock=0.00s (0s)  Rows=7.0 (7), root[root]@[127.0.0.1]
  SELECT * FROM student WHERE name = 'S'

Count: 1  Time=1.21s (1s)  Lock=0.00s (0s)  Rows=1.0 (1), root[root]@[127.0.0.1]
  SELECT * FROM student WHERE stuno = N

Died at /usr/bin/mysqldumpslow line 162, <> chunk 3.

# 添加 -a 参数，显示 SQL 中的真实数据，不将具体的数值被 N 代替，字符串被 S 代替
xisun@xisun-develop:~/apps$ sudo mysqldumpslow -s t -t 5 -a mysql/data/de5e82a9b92d-slow.log 

Reading mysql slow query log from mysql/data/de5e82a9b92d-slow.log
Count: 1  Time=248.07s (248s)  Lock=0.00s (0s)  Rows=0.0 (0), root[root]@[127.0.0.1]
  CALL insert_stu1(100001,4000000)

Count: 1  Time=1.26s (1s)  Lock=0.00s (0s)  Rows=7.0 (7), root[root]@[127.0.0.1]
  SELECT * FROM student WHERE name = 'ZfCwDz'

Count: 1  Time=1.21s (1s)  Lock=0.00s (0s)  Rows=1.0 (1), root[root]@[127.0.0.1]
  SELECT * FROM student WHERE stuno = 3455655

Died at /usr/bin/mysqldumpslow line 162, <> chunk 3.
```

> Docker 容器中可能没有 mysqldumpslow 命令，此时，可以下载 MySQL 源码，上传到宿主机。MySQL 慢日志路径为 /var/lib/mysql，此路径在启动 Docker 容器时，已经被挂载到宿主机，可以在宿主机上，使用下载的 MySQL 源码中的 mysqldumpslow 命令分析慢日志。
>
> 查看 mysqldumpslow 帮助：
>
> ```bash
> $ mysqldumpslow --help
> Usage: mysqldumpslow [ OPTS... ] [ LOGS... ]
> 
> Parse and summarize the MySQL slow query log. Options are
> 
> --verbose    verbose
> --debug      debug
> --help       write this text to standard output
> 
> -v           verbose
> -d           debug
> -s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default
>     al: average lock time
>     ar: average rows sent
>     at: average query time
>      c: count
>      l: lock time
>      r: rows sent
>      t: query time  
> -r           reverse the sort order (largest last instead of first)
> -t NUM       just show the top n queries
> -a           don't abstract all numbers to N and strings to 'S'
> -n NUM       abstract numbers with at least n digits within names
> -g PATTERN   grep: only consider stmts that include this string
> -h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
>    default is '*', i.e. match all
> -i NAME      name of server instance (if using mysql.server startup script)
> -l           don't subtract lock time from total time
> ```
>
> mysqldumpslow 命令的具体参数如下：
>
> - `-a`：不将数字抽象成 N，字符串抽象成 S。
>
> - `-s`：是表示按照何种方式排序：
>   - c：访问次数
>   - l：锁定时间
>   - r：返回记录
>   - t：查询时间
>   - al：平均锁定时间
>   - ar：平均返回记录数
>   - at：平均查询时间 （默认方式）
>   - ac：平均查询次数
>
> - `-t`：即为返回前面多少条的数据。
>
> - `-g`：后边搭配一个正则匹配模式，大小写不敏感的；
>
> mysqldumpslow 常用查询：
>
> ```bash
> # 得到返回记录集最多的 10 个 SQL
> $ mysqldumpslow -s r -t 10 /var/lib/mysql/xisun-slow.log
> 
> #  得到访问次数最多的 10 个 SQL
> $ mysqldumpslow -s c -t 10 /var/lib/mysql/xisun-slow.log
> 
> # 得到按照时间排序的前 10 条里面含有左连接的查询语句
> $ mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/xisun-slow.log
> 
> # 另外建议在使用这些命令时结合 | 和 more 使用，否则有可能出现爆屏情况
> $ mysqldumpslow -s r -t 10 /var/lib/mysql/xisun-slow.log | more
> ```

### 关闭慢查询日志

#### 临时关闭

```mysql
mysql> SET GLOBAL slow_query_log='OFF';
```

#### 永久关闭

修改 my.cnf 或 my.ini 文件，把 mysqld 组下的 slow_query_log 值设置为 OFF，修改保存后，再重启 MySQL 服务，即可生效。

### 删除慢查询日志

慢查询日志都是使用`mysqladmin -uroot -p flush-logs slow` 命令来删除重建的。使用时一定要注意，一旦执行了这个命令，慢查询日志都只存在于新的日志文件中，如果需要旧的查询日志，就必须事先备份。或者也可以直接手动删除慢查询日志。

## 查看 SQL 执行成本：SHOW PROFILE

`// TODO`

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql-advanced.md