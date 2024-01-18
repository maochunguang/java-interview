## java EE规范
Java EE（Java Platform, Enterprise Edition），现在更名为 Jakarta EE，是一套用于企业级Java应用开发的规范和技术标准。Java EE提供了一系列API和服务，以支持开发和部署分布式、可扩展、可安全访问的企业级应用程序。以下是Java EE规范的主要方面：

### 1. **Servlet API:**
   - Servlet是用于处理Web请求和生成动态内容的Java程序。Servlet API定义了与HTTP请求和响应相关的类和接口。

### 2. **JSP（JavaServer Pages）:**
   - JSP允许在Java代码中嵌入HTML，使得动态内容的生成更为简便。JSP是一种模板语言，可用于创建动态Web页面。

### 3. **JPA（Java Persistence API）:**
   - JPA是用于实现对象关系映射（ORM）的规范，用于将Java对象映射到数据库表。它简化了数据持久性的开发。

### 4. **EJB（Enterprise JavaBeans）:**
   - EJB提供了一种构建分布式、事务性企业级应用的标准方式。包括Session Bean、Message-Driven Bean和Entity Bean等组件。

### 5. **JMS（Java Message Service）:**
   - JMS定义了Java平台上的消息中间件的API，用于在分布式系统中进行异步消息通信。

### 6. **JTA（Java Transaction API）:**
   - JTA为Java应用程序提供了分布式事务处理的API。它允许开发者在多个资源上执行事务操作。

### 7. **JCA（Java Connector Architecture）:**
   - JCA定义了一套标准的架构，允许Java EE应用程序与企业信息系统（EIS）进行连接。

### 8. **JavaMail API:**
   - JavaMail提供了用于发送和接收电子邮件的API，是构建与电子邮件相关的企业级应用程序的标准。

### 9. **Java Authentication and Authorization Service (JAAS):**
   - JAAS提供了在Java应用程序中进行身份验证和授权的标准API。

### 10. **JAX-RS（Java API for RESTful Web Services）:**
    - JAX-RS是用于构建RESTful Web服务的规范，支持通过HTTP协议进行资源的创建、读取、更新和删除操作。

### 11. **JAXB（Java Architecture for XML Binding）:**
    - JAXB定义了将Java对象与XML文档进行相互转换的标准方式。

### 12. **WebSocket API:**
    - WebSocket API提供了一种在Web应用程序中进行全双工通信的标准方式。

### 13. **Java EE Connector Architecture:**
    - Java EE Connector Architecture提供了一种标准的方式，用于连接Java EE应用程序与企业信息系统（EIS）之间的连接器。

Java EE规范的每个版本都引入了新的特性、改进和对新技术的支持。然而，需要注意的是，随着Java EE的发展，Java EE规范已经转交给了Eclipse Foundation，并更名为 Jakarta EE。因此，后续的 Jakarta EE 规范将由 Eclipse Foundation 负责制定和推进。

## servlet的生命周期
1. 只有一个Servlet对象（要点）
2. 第一次请求的时候被初始化，只此一遍
3. 初始化后先调用init方法，只此一遍
4. 每个请求，调用一遍service -> service -> doGet/doPost。以多线程的方式运行
5. 卸载前调用destroy方法

## servlet规范
Servlet规范是一组Java API（Application Programming Interface，应用程序编程接口）规范，用于在Java EE（Enterprise Edition，企业版）平台上开发基于HTTP协议的Web应用程序。Servlet是一种Java程序，通过Servlet容器（如Tomcat）运行，用于处理Web请求和生成动态Web内容。

以下是一些关键的Servlet规范特性和概念：

1. **Servlet类：** Servlet是一个Java类，通常扩展自`javax.servlet.http.HttpServlet`类。开发人员需要实现Servlet类，并覆盖一些生命周期方法来处理请求和生成响应。

2. **生命周期方法：** Servlet生命周期由容器管理。Servlet容器在需要时会调用Servlet的生命周期方法，包括`init()`（初始化）、`service()`（处理请求）、`destroy()`（销毁）等。

3. **请求和响应对象：** Servlet使用`HttpServletRequest`和`HttpServletResponse`对象来处理HTTP请求和生成HTTP响应。这些对象封装了与请求和响应相关的信息，例如参数、头部、内容等。

4. **URL映射：** Servlet通过在`web.xml`配置文件或使用注解来定义URL模式，以便Servlet容器能够将特定URL请求映射到相应的Servlet类。

5. **Session管理：** Servlet规范提供了`HttpSession`接口，用于在客户端和服务器之间跟踪用户的会话状态。通过会话，可以在多个请求之间保持用户相关的信息。

6. **过滤器：** Servlet过滤器是一种机制，允许在请求到达Servlet之前或响应离开Servlet之后执行一些处理。过滤器可用于修改请求、响应，或者执行其他与请求和响应相关的任务。

7. **监听器：** Servlet规范定义了一些事件和监听器，用于在Web应用程序生命周期中捕获事件。例如，`ServletContextListener`可用于在Web应用程序启动和关闭时执行一些操作。

8. **异步处理：** Servlet规范支持异步处理，允许Servlet在处理请求时挂起线程，而不必等待操作完成。这有助于提高Web应用程序的性能和并发性。

Servlet规范的版本会随着时间推移而更新，每个版本都引入了新的功能和改进。开发人员可以根据Java EE平台的版本选择相应的Servlet规范版本。在现代的Java开发中，Servlet规范通常与Java EE的其他规范一起使用，如JSP（JavaServer Pages）、JNDI（Java Naming and Directory Interface）等。

## cookie和session
cookie:客户端保存，大小有限制，容易被篡改
session:服务器保存，大小无限制，
