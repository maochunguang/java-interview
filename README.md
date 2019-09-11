# java架构师成长手册

![java面试](images/interview.gif)
## 致找工作的java程序员
当你怀着忐忑不安的心情开始自己的程序员生涯时，那就注定要不断学习新的技术和思想，否则你就被后辈拍在沙滩上了，不对，应该是办公桌上。

![书籍截图](images/book.png)


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
