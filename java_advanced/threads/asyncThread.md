# java异步

## java异步的发展

jdk1.5 之前
1. 缺少线程管理的原生支持
2. 缺少锁的支持
3. 缺少执行完成的支持
4. 执行结果获取困难
thread/runnable

java1.5
juc框架横空出世

Executor

jdk1.7
Fork/join
Forkjoinpool
forkjoinTask
recursiveAction

无法手动完成
阻塞式返回结果
无法处理多个future
无法合并多个future
缺少异常处理

jdk1.8
异步Fork/join

jdk1.9

guava异步实现

thrift异步实现

java 线程的使用。定时更新，