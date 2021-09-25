# 分布式ID生成系统设计

## 分布式id系统要求
1. 保证全局唯一，不会重复
2. 生成的id有递增趋势，不是无序的字符串（可选）
3. 性能高，可用性高（可选）


## 方案一、uuid生成

**实现方式：**

````java
    public static String getUuid(){
        return UUID.randomUUID().toString();
    }
````

**优点：**

1. 实现简单，
2. 性能高

**缺点：**

1. 无序的字符串，不具备趋势自增特性
2. 没有具体的业务含义
3. 长度过长16字节128位，36位长度的字符串，存储以及查询对MySQL的性能消耗较大，MySQL官方明确建议主键要尽量越短越好，作为数据库主键 UUID 的无序性会导致数据位置频繁变动，严重影响性能





## 方案二、数据库自增ID

**实现方式：**

````sql
CREATE TABLE SEQID.SEQUENCE_ID (
    id bigint(20) unsigned NOT NULL auto_increment,
    value char(10)NOT NULL default '', PRIMARY KEY (id),
)ENGINE=MyISAM Comment '分布式id';

insert into SEQUENCE_ID(value) VALUES ('values');
````

**优点：**

1. 实现简单，
2. 自增有序

**缺点：**

1. DB单点存在宕机风险，
2. 无法扛住高并发场景





## 方案三、