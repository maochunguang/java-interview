# java架构师面试手册

![java面试](https://blog-pics-1252092369.cos.ap-beijing.myqcloud.com/interview.jpeg)

## 致各位努力搬砖的程序员
小编本人是双非学校，非计算机专业。通过自学2014进入java编程行业，从刚入职场的惴惴不安，经历了各种困难，一步一步迈入高级开发工程师。从18线小公司，到腾讯，再到美团，目前就职于字节跳动，面试也是收到的offer很多。这其中的困难和曲折就不说了，这里只是把我面试的经验和知识总结分享给大家，希望大家早日拿到满意的offer！

![书籍截图](https://blog-pics-1252092369.cos.ap-beijing.myqcloud.com/book.png)

## 本地运行，或者发布到github的gh-pages
1. 全局安装mdbook,gh-pages
```bash
cargo install mdbook
cargo install mdbook-wordcount
cargo install mdbook-mermaid
```

1. 本地发布和运行
```bash
cd java-interview
mdbook build ## 本地build，生成_book
mdbook serve ## 本地运行查看
```
1. 在线阅读该文档
[github java-inteview](https://maochunguang.github.io/java-interview/)
[gitee java-inteview](https://mcg_dev.gitee.io/java-interview/)
[个人博客阅读](https://codingmore.site/interview/)


## 版权问题

由于早期个别文章都是从网上找的，没有写引用的出处，如果大家看到了，请指正，小编会第一时间更新出处。