## spring循环依赖如何解决

### spring解决循环依赖的原理
Spring处理循环依赖的基本过程：

1. Bean的注册阶段： 当Spring容器启动时，它会解析配置信息，扫描并注册Bean定义。在这个阶段，Spring会为每个Bean创建一个ObjectFactory，用于在需要的时候实际创建Bean的实例。

2. 提前暴露： 当一个Bean A依赖于另一个Bean B，而Bean B 又依赖于 Bean A 时，Spring 会尝试提前暴露尚未完全初始化的Bean A 实例给 Bean B。这样，Bean B 就可以在初始化过程中引用到 Bean A 的实例。

3. 三级缓存： Spring 使用三级缓存来处理循环依赖。这三级缓存分别是singletonObjects、earlySingletonObjects 和singletonFactories。这些缓存分别用于存储已完全初始化的单例Bean、提前暴露的Bean和尚未完全初始化的Bean的ObjectFactory。

4. 实际实例化： 当Bean A 的实例被提前暴露给 Bean B 后，Spring 将继续完成 Bean A 的初始化。完成后，Bean A 的实例将存储在 singletonObjects 缓存中。接着，Spring 会使用提前暴露的 Bean A 实例完成 Bean B 的初始化。

通过这种方式，Spring通过提前暴露和缓存机制解决了循环依赖的问题。但是需要注意，循环依赖可能导致代码设计不佳，因此最好避免过度的循环依赖。

### 结论

1. spring使用自动注入可以解决循环依赖，
2. 都是用构造方法无法解决。
3. 一个使用构造方法，一个使用自动注入，使用构造方法的必须先加载。


第一种情况，studentB使用构造方法依赖studentA

studentA
1、放入singletonFactories，而且放入registeredSingletons

2、放入earlySingletonObjects，singletonFactories中删除

studentB
3、放入singletonFactories，而且放入registeredSingletons

4、放入singletonObjects，放入registeredSingletons，singletonFactories中删除

studentA
5、放入singletonObjects，放入registeredSingletons,earlySingletonObjects中删除





## spring如何解决循环依赖的？

例子：两个Bean，StudentA，StudentB，互相依赖
1、如果两个bean都用构造方法注入，无法解决，会报错

```
The dependencies of some of the beans in the application context form a cycle:

┌─────┐
|  studentA defined in file [D:\work\features-research\target\classes\com\mcg\framwork\featuresresearch\beans\StudentA.class]
↑     ↓
|  studentB defined in file [D:\work\features-research\target\classes\com\mcg\framwork\featuresresearch\beans\StudentB.class]
└─────┘
```

2、一个使用@Autowired，一个使用构造方法，分两种情况，按照类加载顺序，StudentA在前，StudentA可以构造方法注入StudentB，可以正常启动。反之会启动失败。

3、都是用@Autowired注入，正常启动

