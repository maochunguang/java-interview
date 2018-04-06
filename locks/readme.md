# java并发中锁的使用

## final

## volatile
java编程语言允许线程访问共享变量，为了确保共享变量能被准确和一致的更新，线程应该确保通过排他锁单独获得这个变量。
- [ ] 优点：读性能接近于普通读操作，写性能也非常高。
- [ ] 缺点：必须是原子操作，而且只能是一个变量。


## synchronized和lock
synchronized是jvm底层实现，而lock是java api级别的实现。



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

## 公平锁和非公平锁
* 公平锁：按线程申请锁的时间顺序来进行。
* 非公平锁：不管线程申请锁的时间先后顺序，都是随机线程获得锁。

synchronized中的锁是非公平的，ReentrantLock默认情况下也是非公平的，但可以通过带布尔值的构造函数要求使用公平锁。

## 重入锁和不可重入锁
可重入和不可重入的概念是这样的：当一个线程获得了当前实例的锁，并进入方法A，这个线程在没有释放这把锁的时候，能否再次进入方法A呢？
* 可重入锁：可以再次进入方法A，就是说在释放锁前此线程可以再次进入方法A（方法A递归）。
* 不可重入锁（自旋锁）：不可以再次进入方法A，也就是说获得锁进入方法A是此线程在释放锁钱唯一的一次进入方法A。
* 在java 中，synchronized和java.util.concurrent.locks.ReentrantLock是可重入锁

### 不可重入锁代码实现
```java
public class Lock{
    private boolean isLocked = false;
    public synchronized void lock()
        throws InterruptedException{
        while(isLocked){
            wait();
        }
        isLocked = true;
    }
    public synchronized void unlock(){
        isLocked = false;
        notify();
    }
}
```

### 可重入锁代码实现
```java
public class Lock{
    boolean isLocked = false;
    Thread  lockedBy = null;
    int lockedCount = 0;
    public synchronized void lock() throws InterruptedException{
        Thread callingThread = Thread.currentThread();
        while(isLocked && lockedBy != callingThread){
            wait();
        }
        isLocked = true;
        lockedCount++;
        lockedBy = callingThread;
    }
    public synchronized void unlock(){
        if(Thread.curentThread() == this.lockedBy){
            lockedCount--;
            if(lockedCount == 0){
                isLocked = false;
                notify();
            }
        }
    }
}
```

## happen-before原则
定义：如果操作a happens-before 操作b，那么b操作必须知道a的操作，或者说a的操作一定会通知到b，而不是a一定发生在b前。
