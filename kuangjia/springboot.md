## springboot原理

### springboot的自动装配是如何实现的？
Spring Boot的自动配置是通过`@EnableAutoConfiguration`注解和条件化配置（Conditional Configuration）机制来实现的。下面简要介绍一下这两个关键的组成部分：

1. **@EnableAutoConfiguration注解：** 

   - 在Spring Boot应用中，通常会在主配置类上使用`@SpringBootApplication`注解，该注解包含了`@EnableAutoConfiguration`。
   
   - `@EnableAutoConfiguration`注解启用了Spring Boot的自动配置机制。它使用`@Import`注解引入了`AutoConfigurationImportSelector`类，该类会扫描classpath下的`META-INF/spring.factories`文件中定义的自动配置类，并将其加载到应用上下文中。

2. **条件化配置（Conditional Configuration）：**

   - Spring Boot利用条件化配置来判断是否需要应用某个自动配置类。条件注解，如`@ConditionalOnClass`、`@ConditionalOnMissingClass`、`@ConditionalOnProperty`等，会根据一定的条件决定是否启用某个配置类。
   
   - 例如，`@ConditionalOnClass`注解表示只有在类路径上存在指定的类时，才会启用该配置类。`@ConditionalOnProperty`注解表示只有当指定的属性存在并满足条件时，才会启用该配置类。

通过这两个机制，Spring Boot实现了一种智能的、基于条件的自动配置。在应用启动时，自动配置会根据类路径、存在的类、属性等信息判断是否需要自动配置某些功能，从而简化了开发者的配置工作。如果你对具体的自动配置类感兴趣，可以查看Spring Boot的源码或文档，了解每个自动配置类的条件和配置细节。


### springboot常用的注解

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



### 如何实现一个starter

1. 创建一个maven工程，引入spring依赖
2. 使用@Configuration实现bean注入
3. 新建classpath/META-INF/spring.factories 文件
   1. ``org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.educore.HelloAutoConfig`

### springboot自动化配置

> 参考《springboot实战》，springboot官方文档。
