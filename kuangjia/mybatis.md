# mybatis核心

## mybatis是什么？

## mybatis原理？

## mybatis和hibernate的优缺点

## mybatis的缓存原理


## mybatis中的设计模式
1. 工厂模式：SqlSessionFactory
2. 单例模式：SqlSessionFactory，Configuration
3. 建造者：SqlSessionFactoryBuilder
4. 装饰者模式：CachingExecor simple reuse batch三种exector的装饰，LRUCache FifoCache对PerpetualCache的装饰
5. 代理模式：Spring集成Mybati SqlSessionInterceptor，MapperProxy，Plugin，延迟加载，Log输出（ConnectionLogger，StatementLogger）
6. 模板方法：Executor，BaseExecutor,SimpleExecutor