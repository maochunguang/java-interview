## springboot原理



## springboot常用的注解

1. `@Configuration`
   1. 被@Bean定义的bean会被注册到IOC容器
2. `@EnableAutoConfiguration`
   1. 把所有符合@Configuration的bean加载到IOC容器
3. `@ComponentScan`
   1. 默认扫描当前package下所有注解的类到IOC容器
4. `@Import：AutoConfigurationImportSelector`  
   1. 可以配置普通bean和Configuration注入的bean
   2. 实现`ImportSelector`  接口进行动态注入
   3. 实现`ImportBeanDefinitionRegistrar`  接口动态注入
5. Conditional条件注入：

| Conditions                   | 描述                                     |
| ---------------------------- | ---------------------------------------- |
| @ConditionalOnBean           | 在存在某个 bean 的时候                   |
| @ConditionalOnMissingBean    | 不存在某个 bean 的时候                   |
| @ConditionalOnClass          | 当前 classpath 可以找到某个类型的类时    |
| @ConditionalOnMissingClass   | 当前 classpath 不可以找到某个类型的类 时 |
| @ConditionalOnResource       | 当前 classpath 是否存在某个资源文件      |
| @ConditionalOnProperty       | 当前 jvm 是否包含某个系统属性为某个值    |
| @ConditionalOnWebApplication | 当前 spring context 是否是 web 应用程序  |
| @AutoConfigureAfter          | 在某个bean配置后实例化                   |
| @AutoConfigureBefore         | 在某个bean配置前实例化                   |



## 如何实现一个starter

1. 创建一个maven工程，引入spring依赖
2. 使用@Configuration实现bean注入
3. 新建classpath/META-INF/spring.factories 文件
   1. ``org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.educore.HelloAutoConfig`

## springboot自动化配置

> 参考《springboot实战》，springboot官方文档。
