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
