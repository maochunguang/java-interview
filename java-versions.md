# java发展历史

我们先来看看java成立到现在的所有版本。
1990年初，最初被命名为Oak；

1995年5月23日，Java语言诞生；

1996年1月，第一个JDK-JDK1.0诞生；

1996年4月，10个最主要的操作系统供应商申明将在其产品中嵌入Java技术；

1996年9月，约8.3万个网页应用了Java技术来制作；

1997年2月18日，JDK1.1发布；

1997年4月2日，JavaOne会议召开，参与者逾一万人，创当时全球同类会议纪录；

1997年9月，JavaDeveloperConnection社区成员超过十万；

1998年2月，JDK1.1被下载超过2,000,000次；

1998年12月8日，Java 2企业平台J2EE发布；

1999年6月，SUN公司发布Java三个版本：标准版（J2SE）、企业版（J2EE）和微型版（J2ME）；

2000年5月8日，JDK1.3发布；

2000年5月29日，JDK1.4发布；

2001年6月5日，Nokia宣布到2003年将出售1亿部支持Java的手机；

2001年9月24日，J2EE1.3发布；

2002年2月26日，J2SE1.4发布，此后Java的计算能力有了大幅提升；

2004年9月30日，J2SE1.5发布，成为Java语言发展史上的又一里程碑。为了表示该版本的重要性，J2SE1.5更名为Java SE 5.0；

2005年6月，JavaOne大会召开，SUN公司公开Java SE 6。此时，Java的各种版本已经更名，以取消其中的数字“2”：J2EE更名为Java EE，J2SE更名为Java SE，J2ME更名为Java ME；

2006年12月，SUN公司发布JRE6.0；

2009年4月20日，甲骨文以74亿美元的价格收购SUN公司，取得java的版权，业界传闻说这对Java程序员是个坏消息（其实恰恰相反）；

2010年11月，由于甲骨文对Java社区的不友善，因此Apache扬言将退出JCP；

2011年7月28日，甲骨文发布Java SE 7；

2014年3月18日，甲骨文发表Java SE 8；

2017年7月，甲骨文发表Java SE 9。

## jdk1.5
1. 自动装箱和拆箱
2. 枚举
3. 静态导入
4. 可变参数
5. 内省
6. 泛型
7. for-each循环


## jdk1.6
1. Desktop和systemtTray
2. 使用JAXB2来实现对象与XML之间的映射
3. 理解StAX
4. 使用Compiler API
5. 轻量级Http Server API
6. 插入式注解处理API(Pluggable Annotation Processing API)
7. 用Console开发控制台程序
8. 对脚本语言的支持如: ruby, groovy, javascript.
9. Common Annotations

## jdk1.7
1. switch中可以使用字串了
2. 泛型实例化类型自动推断
3. 自定义自动关闭类，自动关闭资源
4. 新增一些取环境信息的工具方法
5. Boolean类型反转，空指针安全,参与位运算
6. 两个char间的equals
7. 安全的加减乘除
8. 对Java集合（Collections）的增强支持
9. try catch异常扑捉中，一个catch可以写多个异常类型，用"|"隔开
10. 支持二进制文字

## jdk1.8
1. 接口的默认方法
2. Lambda 表达式
3. 函数式接口
4. 方法与构造函数引用
5. lambda访问局部变量
6. Date api更新



## jdk1.9
1. 实现模块化系统
2. HTTP/2支持,package:java.net.http
3. JShell
4. 不可变集合工厂方法
5. 私有接口方法
6. HTML5风格的Java帮助文档
7. 多版本兼容 JAR
8. 统一 JVM 日志
9. G1设置为默认垃圾回收
10. I/O 流新特性

## jdk10
1. 局部变量的类型推断
2. 将 JDK 的多个代码仓库合并到一个储存库中
3. 垃圾收集器接口
4. 向 G1 引入并行 Full GC
5. 应用类数据共享。
6. 线程局部管控。允许停止单个线程，而不是只能启用或停止所有线程
7. 移除 Native-Header Generation Tool (javah)
8. 额外的 Unicode 语言标签扩展。包括：cu (货币类型)、fw (每周第一天为星期几)、rg (区域覆盖)、tz (时区)
9. 在备用内存设备上分配堆内存。允许 HotSpot 虚拟机在备用内存设备上分配 Java 对象堆
10. 基于 Java 的 JIT 编译器（试验版本）
11. 根证书。开源 Java SE Root CA 程序中的根证书
12. 基于时间的版本发布模式。“Feature releases” 版本将包含新特性，“Update releases” 版本仅修复 Bug
