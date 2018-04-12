# mysql核心知识点

## 数据库四种事务隔离级别
* ① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。
* ② Repeatable read (可重复读)：可避免脏读、不可重复读的发生。（默认级别）
* ③ Read committed (读已提交)：可避免脏读的发生。
* ④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。

## 事务的特性
* A（Atomicity） 原子性
* C（Consistency） 一致性
* I（Isolation） 隔离性
* D（Durability） 持久性


## mysql索引
1. 聚簇索引和非聚簇索引的区别（数据结构，作用）
    * 聚簇索引锁B+树结构,所有的数据都在叶子节点
    * 非聚簇是B树结构，所有节点都包含数据,存储的是指针，而不是表中数据
2. 数据库主键用int还是uuid？（效率，用途）
    - int做主键，占用空间小，查询速度快。如果有分布式存储要求，主键会重复。
    - uuid做主键，占用空间大，查询相对慢，支持分布式存储，主键不重复。
3. 加锁原理
    事务加锁是根据索引

4. mysql死锁原因

5. mysql死锁的解决方法

## mysql主键选取
1. 主键自增，查询效率高，索引占用空间小。分布式环境，分表，分库，主键会重复。
2. uuid
3. 自定义主键（特定算法，比如）

## mvvc多版本控制


## sql语句在mysql中的执行过程

## sql语句在数据上执行的顺序

## mysql优化的步骤
1. show status和慢查询日志找出慢sql
2. Explain查看执行计划（type，key，Extra）
3. show profile／s查看mysql线程消耗掉具体时间（高级）
4. show trace查看mysql如何选择执行计划

## 插入大批量数据优化方式

## insert语句优化

## orderby语句优化

## 分页查询优化

## explain字段详解


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
## 左连接，有连接，内连接

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
