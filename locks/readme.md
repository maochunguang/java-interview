# java并发中锁的使用

## final

## volitail

## synchronize

## lock

## 悲观锁和乐观锁
1. 悲观锁（synchronized）,定义：假设所有线程都会获得锁
    * 读锁和读锁互斥，性能低
    * 一个线程获得锁，其他线程只能等待
    * 自动释放锁
2. 乐观锁（ReentrantLock）,定义：假设所有线程都不会锁住
    * 读锁不互斥，性能高
    * 实现非公平锁和公平锁，读写锁
    * 手动控制释放锁
    * 可以中断等待线程

## happen-before原则
