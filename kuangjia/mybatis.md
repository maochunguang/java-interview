# mybatis核心
MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录

## mybatis原理？

## mybatis核心对象及其作用？
SqlSessionFactoryBuilder，创建工厂类
SqlSessionFactory，创建会话
SqlSession，提供操作接口
MapperProxy， 代理mapper接口后，用于找到sql执行

## mybatis和hibernate的优缺点

## mybatis的缓存原理
mybatis有两级缓存，一级缓存和二级缓存。
* 一级缓存是会话级别的，默认开启，维护在BaseExecutor中。
* 二级缓存是namespace共享，需要在Mapper.xml中开启，维护在CachingExecutor中。

## mybatis三种执行器的区别？
1. SimpleExecutor，使用后直接关闭Statement
2. ReuseExecutor，放在缓存中，可复用：PrepareStatement
3. BatchExecutor，支持复用而且可以批量执行update()，通过`ps.addBatch()`实现 `handler.batch(stmt)`

## Mybatis支持哪些数据源类型？
UNPOOLED：不带连接池的数据源
POOLED：带连接池的数据源，在PooledDataSource中维护PooledConnection
JNDI：使用容器的数据源，比如Tomcat配置了C3P0
自定义数据源：

## 关联查询的延迟加载是怎么实现的？
动态代理，在创建实体类对象时进行代理，在调用对象的相关方法时触发二次查询。

## Mybatis翻页的方式和区别？
* 逻辑翻页：通过RowBounds对象，在内存中分页，占用内存高，性能低下。
* 物理翻页：通过改写sql，可用插件拦截Executor实现。占用内存低，性能高。

## Mybatis如何集成到spring的？
* SqlSessionTemplate中有内部类SqlSessionInterceptor对DefaultSqlSession进行代理；
* MapperFactoryBean继承了SqlSessionDaoSupport获取SqlSessionTemplate；
* 接口注册到IOC容器中的beanClass是MapperFactoryBean

## DefaultSqlSession和SqlSessionTemplate的区别？
SqlSessionTemplate是线程安全的。
### 1）为什么 SqlSessionTemplate 是线程安全的？

其内部类 SqlSessionInterceptor 的 invoke()方法中的 getSqlSession()方法：
如果当前线程已经有存在的 SqlSession 对象，会在 ThreadLocal 的容器中拿到SqlSessionHolder，获取 DefaultSqlSession。
如果没有，则会 new 一个 SqlSession，并且绑定到 SqlSessionHolder，放到ThreadLocal 中。
SqlSessionTemplate 中在同一个事务中使用同一个 SqlSession。
调用 closeSqlSession()关闭会话时，如果存在事务，减少 holder 的引用计数。否则直接关闭 SqlSession。
### 2）在编程式的开发中，有什么方法保证 SqlSession 的线程安全？
SqlSessionManager 同时实现了 SqlSessionFactory、SqlSession 接口，通过ThreadLocal 容器维护 SqlSession

## mybatis中的设计模式
1. 工厂模式：SqlSessionFactory
2. 单例模式：SqlSessionFactory，Configuration
3. 建造者：SqlSessionFactoryBuilder
4. 装饰者模式：CachingExecor simple reuse batch三种exector的装饰，LRUCache FifoCache对PerpetualCache的装饰
5. 代理模式：Spring集成Mybatis SqlSessionInterceptor，MapperProxy，Plugin，延迟加载，Log输出（ConnectionLogger，StatementLogger）
6. 模板方法：Executor，BaseExecutor,SimpleExecutor