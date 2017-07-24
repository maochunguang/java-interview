# 数据库基础和进阶知识
## mysql
1. 索引
2. 加锁原理
## mysql优化的步骤
1. show status和慢查询日志找出慢sql
2. Explain查看执行计划（type，key，Extra）
3. show profile／s查看mysql线程消耗掉具体时间（高级）
4. show trace查看mysql如何选择执行计划

## 联合查询和子查询的效率比较
mysql5.5之前联合查询明显优于子查询，因为不需要创建中间表，mysql5.5之后子查询有所提升，
建议**优先使用联合查询**。
## mongodb
1. 分片
2. 集群