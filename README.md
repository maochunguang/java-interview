# java面试大全

## 围绕java程序员面试的知识点，逐一总结归纳。希望每一个java程序员都能找到一个好工作，成为有价值的java程序员！

## 本地运行，或者发布到github的gh-pages
1. 全局安装gitbook-cli,gh-pages
`npm install gitbook-cli gh-pages -g`

2. 本地发布和运行
```bash
cd java-interview
gitbook build
gitbook serve ## 本地运行查看
gh-pages -d _book ## 发布到gh-pages分支
```