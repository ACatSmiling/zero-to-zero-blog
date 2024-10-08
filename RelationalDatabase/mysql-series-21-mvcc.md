>*`Author: ACatSmiling`*
>
>*`Since: 2024-09-21`*

## 什么是 MVCC

**`MVCC`**：**Multiversion Concurrency Control，多版本并发控制。**顾名思义，**MVCC 是通过数据行的多个版本管理来实现数据库的并发控制**。这项技术使得在 InnoDB 的事务隔离级别下执行一致性读操作有了保证。换言之，就是为了查询一些正在被另一个事务更新的行，并且可以看到它们被更新之前的值，这样在做查询的时候就不用等待另一个事务释放锁。

MVCC 没有正式的标准，在不同的 DBMS 中 MVCC 的实现方式可能是不同的，也不是普遍使用的（可以参考相关的 DBMS 文档）。这里讲解 InnoDB 中 MVCC 的实现机制（MySQL 其它的存储引擎并不支持它）。

## 快照读与当前读

MVCC 在 MySQL InnoDB 中的实现主要是为了提高数据库并发性能，用更好的方式去处理读-写冲突，做到即使有读写冲突时，也能做到不加锁，非阻塞并发读，而这个读指的就是快照读，而非当前读。当前读实际上是一种加锁的操作，是悲观锁的实现，而 MVCC 本质是采用乐观锁思想的一种方式。

### 快照读

**`快照读`**：**又叫一致性读，读取的是快照数据。**不加锁的简单的 SELECT 都属于快照读，即不加锁的非阻塞读。比如：

```mysql
SELECT * FROM player WHERE ...;
```

之所以出现快照读的情况，是**基于提高并发性能**的考虑，快照读的实现是基于 MVCC，它在很多情况下，避免了加锁操作，降低了开销。

既然是基于多版本，那么**快照读可能读到的并不一定是数据的最新版本**，而有可能是之前的历史版本。

快照读的**前提是隔离级别不是串行级别**，串行级别下的快照读会退化成当前读。

### 当前读

**`当前读`**：**读取的是记录的最新版本**（最新数据，而不是历史版本的数据），读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。加锁的 SELECT，或者对数据进行增删改操作，都会进行当前读。比如：

```mysql
SELECT * FROM student LOCK IN SHARE MODE; # 共享锁

SELECT * FROM student FOR UPDATE; # 排他锁

INSERT INTO student values ...; # 排他锁

DELETE FROM student WHERE ...; # 排他锁

UPDATE student SET ...; # 排他锁
```

> 注意：**InnoDB 增删改操作默认加 X 锁，读操作默认不加锁。**

## 知识点回顾

### 再谈隔离级别

事务有 4 个隔离级别，可能存在三种并发问题：（准确来说是四种，还有一种是脏写）

![image-20240720235523060](mysql-series-21-mvcc/image-20240720235523060.png)

在 MySQL 中，默认的隔离级别是可重复读，可以解决脏读和不可重复读的问题，如果仅从定义的角度来看，它并不能解决幻读问题。如果想要解决幻读问题，就需要采用串行化的方式，也就是将隔离级别提升到最高，但这样一来就会大幅降低数据库的事务并发能力。

**MVCC 可以不采用锁机制，而是通过乐观锁的方式来解决不可重复读和幻读问题！它可以在大多数情况下替代行级锁，降低系统的开销。**

![image-20240720235653609](mysql-series-21-mvcc/image-20240720235653609.png)

> MySQL 中，是遵循上图的处理方式，可重复读和串行化两种隔离级别，都可以解决幻读的问题。
>
> - 如果隔离级别是可重复读，采用的是 MVCC 的方式，这是 MySQL 默认的隔离级别。
> - 如果隔离级别是串行化，采用的是加锁的方式。
> - **如果采用加锁的方式，使用的是间隙锁解决幻读问题。**

### 隐藏字段、undo log 版本链

回顾一下 undo log 的版本链，对于使用 InnoDB 存储引擎的表来说，它的聚簇索引记录中都包含两个必要的隐藏列。

1. `trx_id`：每次一个事务对某条聚簇索引记录进行改动时，都会把该事务的事务 id 赋值给 trx_id 隐藏列。
2. `roll_pointer`：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到 undo log 中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

举例：student 表数据如下。

```mysql
mysql> SELECT * FROM student;
+----+--------+--------+
| id | name   | class  |
+----+--------+--------+
|  1 | 张三   | 一班    |
+----+--------+--------+
1 row in set (0.07 sec)
```

假设插入该记录的事务 id 为 8，那么此刻该条记录的示意图如下所示：

![image-20240721000255201](mysql-series-21-mvcc/image-20240721000255201.png)

>**insert undo 只在事务回滚时起作用，当事务提交后，该类型的 undo log 就没用了，它占用的 Undo Log Segment 也会被系统回收（也就是该 undo log 占用的 Undo 页面链表要么被重用，要么被释放)。**

假设之后两个事务 id 分别为 10、20 的事务对这条记录进行 UPDATE 操作，操作流程如下：

| 发生时间顺序 | 事务 10                                        | 事务 20                                        |
| ------------ | ---------------------------------------------- | ---------------------------------------------- |
| 1            | BEGIN;                                         |                                                |
| 2            |                                                | BEGIN;                                         |
| 3            | UPDATE student SET name = "李四" WHERE id = 1; |                                                |
| 4            | UPDATE student SET name = "王五" WHERE id = 1; |                                                |
| 5            | COMMIT;                                        |                                                |
| 6            |                                                | UPDATE student SET name = "钱七" WHERE id = 1; |
| 7            |                                                | UPDATE student SET name = "宋八" WHERE id = 1; |
| 8            |                                                | COMMIT;                                        |

>有人可能会想，能不能在两个事务中交叉更新同一条记录呢？
>
>答案是不能！因为这种情况，就是一个事务修改了另一个未提交事务修改过的数据，属于脏写。
>
>InnoDB 使用锁来保证不会有脏写情况的发生，也就是在第一个事务更新了某条记录后，就会给这条记录加锁，另一个事务再次更新时，就需要等待第一个事务提交了，把锁释放之后才可以继续更新。

每次对记录进行改动，都会记录一条 undo log，每条 undo log 也都有一个 roll_pointer 属性（INSERT 操作对应的 undo log 没有该属性，因为 INSERT 记录没有更早的版本，它自己是起始的版本)，可以将这些 undo log 都连起来，串成一个链表：

![image-20240721001216698](mysql-series-21-mvcc/image-20240721001216698.png)

对该记录每次更新后，都会将旧值放到一条 undo log 中，就算是该记录的一个旧版本，随着更新次数的增多，所有的版本都会被 roll_pointer 属性连接成一个链表，把这个链表称之为`版本链`，版本链的头节点就是当前记录最新的值。

另外，每个版本中还包含生成该版本时对应的事务 id。

## MVCC 实现原理之 ReadView

**`MVCC 的实现依赖于：隐藏字段、undo log 版本链、ReadView。`**

### 什么是 ReadView

在 MVCC 机制中，多个事务对同一个行记录进行更新会产生多个历史快照，这些历史快照保存在 undo log 里。如果一个事务想要查询这个行记录，需要读取哪个版本的行记录呢？这时就需要用到 ReadView 了，它解决了行的可见性问题。

**`ReadView`**：**就是事务在使用 MVCC 机制进行快照读操作时产生的读视图。**当事务启动时，会生成数据库系统当前的一个快照，InnoDB 为每个事务构造了一个数组，用来记录并维护系统当前`活跃事务的 ID`（"活跃" 指的就是，启动了但还没提交)

>ReadView 和事务是一对一的关系。

### 设计思路

使用 READ UNCONNMITTED 隔离级别的事务，由于可以读到未提交事务修改过的记录，所以直接读取的记录就是最新版本了。此时，不需要使用 MVCC，也就不需要 ReadView。

使用 SERIALIZABLE 隔离级别的事务，InnoDB 规定使用加锁的方式来访问记录。此时，不需要使用 MVCC，也就不需要 ReadView。

**使用 READ COMMITTED 和 REPEATABLE READ 隔离级别的事务，都必须保证读到已经提交了的事务修改过的记录。假如另一个事务已经修改了记录但是尚未提交，是不能直接读取最新版本的记录的，核心问题就是需要判断一下版本链中的哪个版本是当前事务可见的，这是 ReadView 要解决的主要问题。**

ReadView 中主要包含 4 个比较重要的内容，分别如下：

1. `creator_trx_id`：创建这个 ReadView 的事务 ID。
2. `trx_ids`：表示在生成 ReadView 时，当前系统中活跃的读写事务的事务 id 列表。
3. `up_limit_id`：活跃的事务中最小的事务 ID。
4. `low_limit_id`：表示生成 ReadView 时，系统中应该分配给下一个事务的 id 值。low_limit_id 是当前系统最大的事务 id 值，这里要注意是系统中的事务 id，需要区别于正在活跃的事务 id。

>注意：**low_limit_id 并不是 trx_ids 中的最大值，实际上，low_limit_id 不存在于 trx_ids 中。**事务 id 是递增分配的，比如，现在有 id 为 1，2，3 这三个事务，之后 id 为 3 的事务提交了。那么一个新的读事务在生成 ReadView 时，trx_ids 就包括 1 和 2，up_limit_id 的值就是 1，low_limit_id 的值就是 4。

**举例：**

trx_ids 为 trx2、trx3、trx5 和 trx8 的集合，系统的最大事务 id（low_limit_id）为 trx8 + 1（如果在此之前没有其他的新增事务），活跃的最小事务 id（up_limit_id）为 trx2。

![image-20240721085100350](mysql-series-21-mvcc/image-20240721085100350.png)

### ReadView 的规则

有了这个 ReadView，这样在访问某条记录时，只需要按照下边的步骤判断该记录在 undo log 版本链中的某个版本是否可见：

- 如果被访问版本的 trx_id 属性值`等于 ReadView 中的 creator_trx_id 值`，意味着当前事务在访问它自己修改过的记录，所以`该版本可以被当前事务访问`。
- 如果被访问版本的 trx_id 属性值`小于 ReadView 中的 up_limit_id 值`，表明生成该版本的事务在当前事务生成 ReadView 前已经提交，所以`该版本可以被当前事务访问`。
- 如果被访问版本的 trx_id 属性值`大于或等于 ReadView 中的 low_limit_id 值`，表明生成该版本的事务在当前事务生成 ReadView 后才开启，所以`该版本不可以被当前事务访问`。（否则会出现脏读）
- 如果被访问版本的 trx_id 属性值`在 ReadView 的 up_limit_id 和 low_limit_id 之间`，那就需要判断一下 trx_id 属性值是不是在 trx_ids 列表中。
  - 如果在，说明创建 ReadView 时生成该版本的事务还是活跃的，`该版本不可以被当前事务访问`。
  - 如果不在，说明创建 ReadView 时生成该版本的事务已经被提交，`该版本可以被当前事务访问`。

> 此处被访问版本，是指 undo log 版本链中的版本。 

### MVCC 整体操作流程

了解了这些概念之后，来看下当查询一条记录的时候，系统如何通过 MVCC 找到它：

1. 首先，获取事务自己的版本号，也就是事务 id；
2. 获取（生成）ReadView；
3. 查询得到的数据，然后与 ReadView 中的事务版本号进行比较；
4. 如果不符合 ReadView 规则（当前版本不能被访问），就需要从 undo log 中获取历史快照；
5. 最后返回符合规则的数据。

**如果某个版本的数据对当前事务不可见的话，那就`顺着 undo log 版本链`找到下一个版本的数据，继续按照上边的步骤判断可见性，依此类推，直到版本链中的最后一个版本。如果最后一个版本也不可见的话，那么就意味着该条记录对该事务完全不可见，查询结果就不包含该记录。**

>InnoDB中，MVCC 是通过`undo log 版本链 + ReadView`进行数据读取：undo log 版本链保存了历史快照，而 ReadView 规则帮我们判断当前版本的数据是否可见。

在隔离级别为`读已提交`（READ COMMITTED）时，**一个事务中的每一次 SELECT 查询都会重新获取一次 ReadView**。示例：

| 事务                                 | 说明               |
| ------------------------------------ | ------------------ |
| BEGIN;                               |                    |
| SELECT * FROM student WHERE id  > 2; | 获取一次 Read View |
| …                                    |                    |
| SELECT * FROM student WHERE id  > 2; | 获取一次 Read View |
| COMMIT;                              |                    |

>注意，此时同样的查询语句都会重新获取一次 ReadView，这时如果 ReadView 不同，就可能产生不可重复读或者幻读的情况，这样符合Read Committed的规则特点。

当隔离级别为`可重复读`（REPEATABLE READ）的时候，就避免了不可重复读，这是因为**一个事务只在第一次 SELECT 的时候会获取一次 ReadView，而后面所有的 SELECT 都会复用这个 ReadView**。示例：

![image-20240721101450225](mysql-series-21-mvcc/image-20240721101450225.png)

## 举例说明

假设现在 student 表中只有一条由事务 id 为 8 的事务插入的一条记录：

```mysql
mysql> SELECT * FROM student;
+----+--------+--------+
| id | name   | class  |
+----+--------+--------+
|  1 | 张三   | 一班    |
+----+--------+--------+
1 row in set (0.07 sec)
```

MVCC 只能在`READ COMMITTED`和`REPEATABLE READ`两个隔离级别下工作。接下来看一下 READ COMMITTED 和 REPEATABLE READ 所谓的生成 ReadView 的时机不同，到底不同在哪里。

>关于不同隔离级别下 ReadView 的事务 id，可以概括如下：
>
>- 对于 RC 隔离级别：
> - 在一个事务中，每次查询会创建 id 为 0 的 ReadView。
> - 一旦有修改操作，会切换到以当前事务 id 为 creator_trx_id 的新 ReadView。
>- 对于 RR 隔离级别：
> - 在一个事务中，只有第一次的查询会创建一个 Read View。
> - 这个 ReadView 的 creator_trx_id 就是当前的事务 id。
>
>**RR 要求整个事务的查询都要一致，所以只有第一次查询才会生成一个 ReadView。而 RC 可以在同一事务内读取不同版本的数据，所以每次修改和查询都会生成新的 ReadView。**

### READ COMMITTED 隔离级别下

**`READ COMMITTED：每次读取数据前都生成一个 ReadView。`**

现在有两个事务 id 分别为 10、20 的事务在执行：

```mysql
# Transaction 10
BEGIN;
UPDATE student SET name = "李四" WHERE id = 1;
UPDATE student SET name = "王五" WHERE id = 1;

# Transaction 20
BEGIN;
# 更新了一些别的表的记录 (为了分配事务 id)
...
```

> 说明：**事务执行过程中，只有在第一次真正修改记录时（比如使用 INSERT、DELETE、UPDATE 语句），才会被分配一个单独的事务 id，这个事务 id 是递增的。**所以我们才在事务 20 中更新一些别的表的记录，目的是让它分配事务 id。

此刻，表 student 中 id 为 1 的记录得到的 undo log 版本链如下所示：

![image-20240721102342138](mysql-series-21-mvcc/image-20240721102342138.png)

假设现在有一个使用  READ COMMITTED 隔离级别的事务开始执行：

```mysql
# 使用 READ COMMITTED 隔离级别的事务

BEGIN;
# SELECT1 操作，此时，Transaction 10 和 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值为'张三'
```

这个 SELECT1 的执行过程如下：

1. 步骤一：在执行 SELECT 语句时会先生成一个 ReadView，ReadView 的 trx_ids 列表的内容就是 [10, 20]，up_limit_id 为 10，low_limit_id 为 21，creator_trx_id 为 0。
2. 步骤二：从 undo log 版本链中挑选可见的记录，从图中看出，最新版本的列 name 的内容是 '王五'，该版本的 trx_id 值为 10，在 trx_ids 列表内（说明 ReadView 生成时，trx_id 为 10 的事务还是活跃的），所以不符合可见性要求，根据 roll_pointer 跳到下一个版本。
3. 步骤三：下一个版本的列 name 的内容是 '李四'，该版本的 trx_id 值也为 10，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4. 步骤四：下一个版本的列 name 的内容是 '张三'，该版本的 trx_id 值为 8，小于 ReadView 中的 up_limit_id 值 10，所以这个版本是符合要求的，最后，返回给用户的版本就是这条列 name 为 '张三' 的记录。

之后，把 事务 id 为 10 的事务提交一下：

```mysql
# Transaction 10
BEGIN;

UPDATE student SET name = "李四" WHERE id = 1;
UPDATE student SET name = "王五" WHERE id = 1;

COMMIT;
```

然后再到事务 id 为 20 的事务中，更新一下表 student 中 id 为 1 的记录：

```mysql
# Transaction 20
BEGIN;

# 更新了一些别的表的记录
...
UPDATE student SET name = "钱七" WHERE id = 1;
UPDATE student SET name = "宋八" WHERE id = 1;
```

此刻，表 student 中 id 为 1 的记录的版本链就长这样：

![image-20240721103731212](mysql-series-21-mvcc/image-20240721103731212.png)

然后，再到刚才使用 READ COMMITTED 隔离级别的事务中继续查找这个 id 为 1 的记录，如下：

```mysql
# 使用 READ COMMITTED 隔离级别的事务
BEGIN;

# SELECT1 操作，此时，Transaction 10 和 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值为'张三'

# SELECT2 操作，此时，Transaction 10 提交，Transaction 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值为'王五'
```

这个 SELECT2 的执行过程如下:

1. 步骤一：在执行 SELECT 语句时会又会单独生成一个 ReadView，该 ReadView 的 trx_ids 列表的内容就是 [20]，up_limit_id 为 20，low_limit_id 为 21，creator_trx_id 为 0。
2. 步骤二：从 undo log 版本链中挑选可见的记录，从图中看出，最新版本的列 name 的内容是 '宋八'，该版本的 trx_id 值为20，在 trx_ids 列表内，所以不符合可见性要求，根据 roll_pointer 跳到下一个版本。
3. 步骤三：下一个版本的列 name 的内容是 '钱七'，该版本的 trx_id 值为 20，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4. 步骤四：下一个版本的列 name 的内容是 '王五'，该版本的 trx_id 值为 10，小于 ReadView 中的 up_limit_id 值 20，所以这个版本是符合要求的，最后，返回给用户的版本就是这条列 name 为 '王五' 的记录。

以此类推，如果之后事务 id 为 20 的记录也提交了，再次在使用 READ COMMITED 隔离级别的事务中，查询表 student 中 id 值为 1 的记录时，得到的结果就是 '宋八' 了，具体流程我们就不分析了。

> **强调：** 使用 READ COMMITTED 隔离级别的事务，在每次查询开始时，都会生成一个独立的 ReadView。

### REPEATABLE READ 隔离级别下

**`REPEATABLE READ：只会在第一次执行查询语句时生成一个 ReadView，之后的查询就不会重复生成了，而是复用这个 ReadView。`**

比如，系统里有两个事务 id 分别为 10、20 的事务在执行：

```mysql
# Transaction 10
BEGIN;
UPDATE student SET name = "李四" WHERE id = 1;
UPDATE student SET name = "王五" WHERE id = 1;

# Transaction 20
BEGIN;
# 更新了一些别的表的记录
...
```

此刻，表 student 中 id 为 1 的记录得到的版本链表如下所示：

![image-20240721110043183](mysql-series-21-mvcc/image-20240721110043183.png)

假设现在有一个使用 REPEATABLE READ 隔离级别的事务开始执行：

```mysql
# 使用 REPEATABLE READ 隔离级别的事务
BEGIN;

# SELECT1 操作，此时，Transaction 10 和 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值为'张三'
```

这个 SELECT1 的执行过程如下：

1. 步骤一：在执行 SELECT 语句时会先生成一个 ReadView，ReadView 的 trx_ids 列表的内容就是 [10, 20]，up_limit_id 为 10，low_limit_id 为 21，creator_trx_id 为 0。
2. 步骤二：然后从 undo log 版本链中挑选可见的记录，从图中看出，最新版本的列 name 的内容是 '王五'，该版本的 trx_id 值为 10，在 trx_ids 列表内，所以不符合可见性要求，根据 roll_pointer 跳到下一个版本。
3. 步骤三：下一个版本的列 name 的内容是 '李四'，该版本的 trx_id 值也为 10，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4. 步骤四：下一个版本的列 name 的内容是 '张三'，该版本的 trx_id 值为 8，小于 ReadView 中的 up_limit_id 值10，所以这个版本是符合要求的，最后，返回给用户的版本就是这条列 name 为 '张三 ' 的记录。

之后，我们把事务 id 为 10 的事务提交一下，就像这样：

```mysql
# Transaction 10
BEGIN;

UPDATE student SET name = "李四" WHERE id = 1;
UPDATE student SET name = "王五" WHERE id = 1;

COMMIT;
```

然后，再到事务 id 为 20 的事务中更新一下表 student 中 id 为 1 的记录：

```mysql
# Transaction 20
BEGIN;

# 更新了一些别的表的记录
...
UPDATE student SET name = "钱七" WHERE id = 1;
UPDATE student SET name = "宋八" WHERE id = 1;
```

此刻，表 student 中 id 为 1 的记录的版本链长这样：

![image-20240721110539834](mysql-series-21-mvcc/image-20240721110539834.png)

然后，再到刚才使用 REPEATABLE READ 隔离级别的事务中继续查找这个id 为 1 的记录，如下：

```mysql
# 使用 REPEATABLE READ 隔离级别的事务
BEGIN;

# SELECT1 操作，此时，Transaction 10 和 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值为'张三'

# SELECT2 操作，此时，Transaction 10 提交，Transaction 20 未提交
SELECT * FROM student WHERE id = 1; # 得到的列 name 的值仍为'张三'
```

SELECT2 的执行过程如下:

1. 步骤一：因为当前事务的隔离级别为 REPEATABLE READ，而之前在执行 SELECT1 时已经生成过 ReadView 了，所以此时直接复用之前的 ReadView，之前的 ReadView 的 trx_ids 列表的内容就是 [10, 20]，up_limit_id 为 10，low_limit_id 为 21，creator_trx_id 为 0。
2. 步骤二：然后从 undo log 版本链中挑选可见的记录，从图中可以看出，最新版本的列 name 的内容是 '宋八'，该版本的 trx_id 值为 20，在 trx_ids 列表内，所以不符合可见性要求，根据 roll_pointer 跳到下一个版本。
3. 步骤三：下一个版本的列 name 的内容是 '钱七'，该版本的 trx_id 值为 20，也在 trx_ids 列表内，所以也不符合要求，继续跳到下一个版本。
4. 步骤四：下一个版本的列 name 的内容是 '王五'，该版本的 trx_id 值为 10，而 trx_ids 列表中是包含值为 10 的事务 id 的，所以该版本也不符合要求。同理，下一个列 name 的内容是 '李四' 的版本也不符合要求，继续跳到下一个版本。
5. 步骤五：下一个版本的列 name 的内容是 '张三'，该版本的 trx_id 值为 8，小于 ReadView 中的 up_limit_id 值 10，所以这个版本是符合要求的，最后，返回给用户的版本就是这条列 name 为 '张三' 的记录。

两次 SELECT 查询得到的结果是重复的，记录的列 name 值都是 '张三'，这就是可重复读的含义。如果我们之后再把事务 id 为 20 的记录提交了，然后再到刚才使用 REPEATABLE READ 隔离级别的事务中，继续查找这个 id 为 1 的记录，得到的结果还是 '张三'，具体执行过程大家可以自己分析一下。

### 如何解决幻读

接下来说明 InnoDB 是如何解决幻读的。

假设现在表 student 中只有一条数据，数据内容中，主键 id = 1，隐藏的 trx_id = 10，它的 undo log 如下图所示：

![image-20240721120852189](mysql-series-21-mvcc/image-20240721120852189.png)

假设现在有事务 A 和事务 B 并发执行，事务 A 的事务 id 为 20，事务 B 的事务 id 为 30。

步骤一：事务 A 开始第一次查询数据，查询的 SQL 语句如下。

```mysql
SELECT * FROM student WHERE id >= 1;
```

在开始查询之前，MySQL 会为事务 A 产生一个 ReadView，此时 ReadView 的内容如下：trx_ids = [20, 30]，up_limit_id = 20，low_limit_id = 31，creator_trx_id = 20。

由于此时表 student 中只有一条数据，且符合 WHERE id >= 1 条件，因此会查询出来。然后根据 ReadView机制，发现该行数据的 trx_id = 10，小于事务 A 的 ReadView 里 up_limit_id，这表示这条数据是事务 A 开启之前，其他事务就已经提交了的数据，因此事务 A 可以读取到。

结论：事务 A 的第一次查询，能读取到一条数据，id = 1。

步骤二：接着事务 B，往表 student 中新插入两条数据，并提交事务。

```mysql
INSERT INTO student(id, name) VALUES(2, '李四');
INSERT INTO student(id, name) VALUES(3, '王五');
```

此时，表 student 中就有三条数据了，对应的 undo log 如下图所示：

![image-20240721121549931](mysql-series-21-mvcc/image-20240721121549931.png)

步骤三：接着事务 A 开启第二次查询，根据可重复读隔离级别的规则，此时事务 A 并不会再重新生成 ReadView。此时表 student 中的 3 条数据都满足 WHERE id >= 1 的条件，因此会先查出来。然后根据 ReadView 机制，判断每条数据是不是都可以被事务 A 看到。

1. 首先 id = 1 的这条数据，前面已经说过了，可以被事务 A 看到。
2. 然后是 id = 2 的数据，它的 trx_id = 30，此时事务 A 发现，这个值处于 up_limit_id 和 low_limit_id 之间，因此还需要再判断 30 是否处于 trx_ids 数组内。由于事务 A 的 trx_ids = [20, 30]，因此在数组内，这表示 id = 2 的这条数据是与事务 A 在同一时刻启动的其他事务提交的，所以这条数据不能让事务 A 看到。
3. 同理，id = 3 的这条数据，trx_id 也为 30，因此也不能被事务 A 看见。

如下图所示：

![image-20240721122343548](mysql-series-21-mvcc/image-20240721122343548.png)

结论：最终事务 A 的第二次查询，只能查询出 id = 1 的这条数据，这和事务 A 的第一次查询的结果是一样的，因此没有出现幻读现象，所以说在 MySQL 的可重复读隔离级别下，不存在幻读问题。

## 总结

这里介绍了 MVCC 在 READ COMMITTD、REPEATABLE READ 这两种隔离级别的事务，在执行快照读操作时访问记录的版本链的过程。这样使不同事务的读-写、写-读操作并发执行，从而提升系统性能

核心点在于 ReadView 的原理，READ COMMITTD、REPEATABLE READ 这两个隔离级别的一个很大不同就是生成 ReadView 的时机不同：

- READ COMMITTD 在每一次进行普通 SELECT 操作前，都会生成一个ReadView。
- REPEATABLE READ 只在第一次进行普通 SELECT 操作前生成一个 ReadView，之后的查询操作都重复使用这个 ReadView。

>说明：之前说执行 DELETE 语句或者更新主键的 UPDATE 语句，并不会立即把对应的记录完全从页面中删除，而是执行一个所谓的`delete mark`操作（标记 0 -> 1），相当于只是对记录打上了一个删除标志位，这主要就是为 MVCC 服务的。另外后面回滚也可能用到这个 delete mark。

通过 MVCC 可以解决：

- `读写之间阻塞的问题`：通过 MVCC 可以让读写互相不阻塞，即读不阻塞写，写不阻塞读，这样就可以提升事务并发处理能力。
- `降低了死锁的概率`：这是因为 MVCC 采用了乐观锁的方式，读取数据时并不需要加锁，对于写操作，也只锁定必要的行。
- `解决快照读的问题`：当查询数据库在某个时间点的快照时，只能看到这个时间点之前事务提交更新的结果，而不能看到这个时间点之后事务提交的更新结果。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/RelationalDatabase/mysql-advanced.md