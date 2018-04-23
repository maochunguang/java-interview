# java参考手册

![java面试](images/interview.gif)
## 致刚入行的java程序员
当你怀着忐忑不安的心情开始自己的程序员生涯时，那就注定要不断学习新的技术和思想，否则你就被后辈拍在沙滩上了，不对，应该是办公桌上。

![书籍截图](images/book.png)

## 分析问题从三个角度
why？为什么使用netty？
what？netty是什么？
how？如何使用netty？

## 原理分析分为几个纬度
线程模型，io模型
内存模型-
数据结构-架构的性能
数据一致性和安全性-保证高可用

## 写这个手册的目的
这些知识点，都是工作中最常用的。就算做代码搬运工，也不能什么都百度吧，有个参考手册，比百度至少高了一个档次。不仅查找方便，而且是装逼神器！

## 本地运行，或者发布到github的gh-pages
1. 全局安装gitbook-cli,gh-pages
`npm install gitbook gitbook-cli gh-pages -g`

2. 本地发布和运行
```bash
cd java-interview
gitbook build
gitbook serve ## 本地运行查看
gh-pages -d _book ## 发布到gh-pages分支
```
3. 在线阅读该文档
[java-inteview](https://maochunguang.github.io/java-interview/)
