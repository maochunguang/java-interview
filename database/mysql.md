# mysql核心知识点

## 数据库四种事务隔离级别
* ① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。
* ② Repeatable read (可重复读)：可避免脏读、不可重复读的发生。（默认级别）
* ③ Read committed (读已提交)：可避免脏读的发生。
* ④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。

## 事务的特性
* A（Atomicity） 原子性
  * 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
* C（Consistency） 一致性
  *  执行事务前后，数据保持一致，多个事务对同一个数据读取的结果是相同的
* I（Isolation） 隔离性
  * 并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
* D（Durability） 持久性
  * 一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。


## 左连接，右连接，内连接

## mysql索引
**Mysql目前提供了四种索引：**
1. B-Tree索引： 最常见的索引类型， ⼤部分引擎都⽀持B树索引。
2. HASH索引： 只有Memory引擎⽀持， 使⽤场景简单。
3. R-Tree索引（空间索引） ： 空间索引是MyISAM的⼀个特殊索引类型， 主要⽤于地理空间数据类型， 通常使⽤较少， 不做特别介绍。
4. Full-text（全⽂索引） ： 全⽂索引也是 MyISAM 的⼀个特殊索引类型， 主要⽤于全⽂索引， InnoDB从MySQL5.6版本开始提供对全⽂索引的⽀持

**MyISAM、 InnoDB、 Memory三个常⽤引擎⽀持的索引类型⽐较**

| 索引类型  | MyISAM | InnoDB | Memory |
| --------- | ------ | ------ | ------ |
| B-Tree    | 支持   | 支持   | 支持   |
| HASH      | 不支持 | 不支持 | 支持   |
| R-Tree    | 支持   | 不支持 | 不支持 |
| Full-text | 支持   | 不支持 | 不支持 |


聚簇索引和非聚簇索引的区别（数据结构，作用）
    * 聚簇索引锁B+树结构,所有的数据都在叶子节点
    * 非聚簇是B树结构，所有节点都包含数据,存储的是指针，而不是表中数据


## 加锁原理
事务加锁是根据索引

### mysql死锁原因

### mysql死锁的解决方法

## mysql主键选取
1. 主键自增，查询效率高，索引占用空间小。分布式环境，分表，分库，主键会重复。
2. uuid，占用空间大，查询相对慢，支持分布式存储，主键不重复。
3. 自定义主键（特定算法，比如）

## mvvc多版本控制


## sql语句在mysql中的执行过程

## sql语句在数据上执行的顺序

## mysql优化的步骤
1、show status，查看执行状态
   1. Com_select：执⾏SELECT操作的次数，⼀次查询只累加1。
   2. Com_insert：执⾏INSERT操作的次数，对于批量插⼊的INSERT操作，只累加⼀次。
   3. Com_update：执⾏UPDATE操作的次数。
   4. Com_delete：执⾏DELETE操作的次数。


2、 慢查询日志找出慢sql
   1. 通过慢查询⽇志定位那些执⾏效率较低的SQL语句，⽤--log-slow-queries[= file_name]选项启动时，mysqld写⼀个包含所有执⾏时间超过long_query_time秒的SQL语句的⽇志⽂件。


3、 Explain查看执行计划（type，key，Extra）
```sql
   mysql> explain select sum(amount) from customer a, payment b where 1=1 and
    a.customer_id= b.customer_id and email = 'JANE.BENNETT@sakilacustomer.org'\G
    *************************** 1. row ***************************
    id: 1
    select_type: SIMPLE
    table: a
    type: ALL
    possible_keys: PRIMARY
    key: NULL
    key_len: NULL
    ref: NULL
    rows: 583
    Extra: Using where
    *************************** 2. row ***************************
    id: 1
    select_type: SIMPLE
    table: b
    type: ref
    possible_keys: idx_fk_customer_id
    key: idx_fk_customer_id
    key_len: 2
    ref: sakila.a.customer_id
    rows: 12
    Extra:
    2 rows in set (0.00 sec)
```
* select_type：表⽰SELECT的类型，常见的取值有
  * SIMPLE（简单表，即不使⽤表连接或者⼦查询）、
  * PRIMARY（主查询，即外层的查询）、
  * UNION（UNION中的第⼆个或者后⾯的查询语句）、
  * SUBQUERY（⼦查询中的第⼀个SELECT）等。
* table：输出结果集的表。
* type：表⽰MySQL在表中找到所需⾏的⽅式，或者叫访问类型，
  * type=ALL，全表扫描
  * type=index，索引全扫描
  * type=range，索引范围扫描
  * type=ref，使用非唯一索引扫描或者唯一索引的前缀扫描
  * type=eq_req，使用的索引是唯一索引
  * type=const/system，单表中最多只有一个匹配行，非常迅速
  * type=NULL，mysql不需要访问表或者索引，直接获得结果
  * 类型type还有其他值，如ref_or_null（与ref类似，区别在于条件中包含对NULL的查询）、index_merge（索引合并优化）、unique_subquery（in 的后⾯是⼀个查询主键字段的⼦查询）、index_subquery（与unique_subquery 类似，区别在于in 的后⾯是查询⾮唯⼀索引字段的⼦查询）等。
* possible_keys：表⽰查询时可能使⽤的索引。
* key：表⽰实际使⽤的索引。
* key_len：使⽤到索引字段的长度。
* rows：扫描⾏的数量。
* Extra：执⾏情况的说明和描述，包含不适合在其他列中显⽰但是对执⾏计划⾮常重要的额外信息。

 
4、show profile查看mysql线程消耗掉具体时间（高级）
通过have_profiling参数，能够看到当前MySQL是否⽀持profile
show profile能够在做SQL优化时帮助我们了解时间都耗费到哪⾥去了。⽽MySQL
5.6则通过trace⽂件进⼀步向我们展⽰了优化器是如何选择执⾏计划的。


5、show trace查看mysql如何选择执行计划
⾸先打开trace，设置格式为JSON，设置trace最⼤能够使⽤的内存⼤⼩，避免解析过程中因为默认内存过⼩⽽不能够完整显⽰。
```sql
mysql> SET OPTIMIZER_TRACE="enabled=on",END_MARKERS_IN_JSON=on;
Query OK, 0 rows affected (0.03 sec)
mysql> SET OPTIMIZER_TRACE_MAX_MEM_SIZE=1000000;
Query OK, 0 rows affected (0.00 sec)
```

6、确定问题采取相应的优化措施


## sql优化

### 插入大批量数据优化方式
当⽤load命令导⼊数据的时候， 适当的设置可以提⾼导⼊的速度。
1. MyISAM存储引擎
   ```sql
   --- 打开或者关闭MyISAM表⾮唯⼀索引的更新
   ALTER TABLE tbl_name DISABLE KEYS;
   loading the data;
   ALTER TABLE tbl_name ENABLE KEYS;
   ```
2. Innodb存储引擎
   1. 因为InnoDB类型的表是按照主键的顺序保存的， 所以将导⼊的数据按照主键的顺序排列， 可以有效地提⾼导⼊数据的效率
   2. 在导⼊数据前执⾏SET UNIQUE_CHECKS=0， 关闭唯⼀性校验， 在导⼊结束后执⾏SET UNIQUE_CHECKS=1， 恢复唯⼀性校验， 可以提⾼导⼊的效率
   3. 如果应⽤使⽤⾃动提交的⽅式， 建议在导⼊前执⾏SET AUTOCOMMIT=0，关闭⾃动提交， 导⼊结束后再执⾏SET AUTOCOMMIT=1， 打开⾃动提交， 也可以提⾼导⼊的效率
   
### insert语句优化
1. 如果同时从同⼀客户端插⼊很多⾏， 应尽量使⽤多个值表的INSERT语句， 这种⽅式将⼤⼤缩减客户端与数据库之间的连接、 关闭等消耗。
2. 如果从不同客户端插⼊很多⾏， 可以通过使⽤ INSERT DELAYED语句得到更⾼的速度。 DELAYED的含义是让INSERT语句马上执⾏， 其实数据都被放在内存的队列中， 并没有真正写⼊磁盘。
3. 如果进⾏批量插⼊， 可以通过增加bulk_insert_buffer_size变量值的⽅法来提⾼速度， 但是， 这只能对MyISAM表使⽤。
4. 当从⼀个⽂本⽂件装载⼀个表时， 使⽤ LOAD DATA INFILE。 这通常⽐使⽤很多INSERT语句快20倍。

### orderby语句优化

### groupby语句优化

### OR语句优化
对于含有OR的查询⼦句， 如果要利⽤索引， 则OR之间的每个条件列都必须⽤到索引； 如果没有索引， 则应该考虑增加索引

### 分页查询优化
1. 第⼀种优化思路，在索引上完成排序分页的操作，最后根据主键关联回原表查询所需要的其他列内容。
2. 把LIMIIT查询转换成某个位置的查询，只适合在排序字段不会出现重复值的特定环境， 

## mysql组合索引最左原则详解
假设表test有id,A,B,C,D五个字段，id是主键，组合索引是C-D,请问以下sql语句哪些会走索引？
```sql
select * from test where C=1;--走索引
select * from test where D=1 and C=1 ;--走索引，mysql会优化
select * from test where C=1 and D=1 ;--走索引
select * from test where D=1;--不走
select C from test where D=1;--走索引，以及部分表扫描,所选字段在组合索引内。
select A, B from test where D=1;--不走索引，全表扫描，所选字段不再组合索引内。
```


## mysql常用参数配置
1. 慢查询配置
2. 默认事物级别
3. 最大连接数


## 联合查询和子查询的效率比较
mysql5.5之前联合查询明显优于子查询，因为不需要创建中间表，mysql5.5之后子查询有所提升，
建议**优先使用联合查询**。

## 数据库表设计的原则

## mysql集群模式
1. 主从
2. 双主+keepalived

## mysql主从同步的原理

## binlog的三种方式
statement，基于sql的模式
row，基于行
mixed，混合模式

## 主从同步延迟是怎么产生的？

## 解决主从同步延迟

## 典型的sql语句编写


> 参考《深入浅出mysql第二版》，《高性能mysql第三版》，两本书。
