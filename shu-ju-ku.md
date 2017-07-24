# 数据库基础和进阶知识
## mysql索引
1. 聚簇索引和非聚簇索引的区别（数据结构，作用）
* 聚簇索引锁B+树结构
* 非聚簇是B树结构
2. 数据库主键用int还是uuid？（效率，用途）
2. 加锁原理
## mysql优化的步骤
1. show status和慢查询日志找出慢sql
2. Explain查看执行计划（type，key，Extra）
3. show profile／s查看mysql线程消耗掉具体时间（高级）
4. show trace查看mysql如何选择执行计划

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

## mysql常用参数配置
1. 慢查询配置
2. 默认事物级别

## 联合查询和子查询的效率比较
mysql5.5之前联合查询明显优于子查询，因为不需要创建中间表，mysql5.5之后子查询有所提升，
建议**优先使用联合查询**。
## mongodb
1. 分片
2. 集群