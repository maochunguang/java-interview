## spring循环依赖如何解决

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

