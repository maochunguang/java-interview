# java并发中锁的使用

## 锁的原理
锁存在每个class对象上，位于对象头信息，也叫markword。只是相当与一个变量标识，标记一个对象是否已经加锁，或者释放锁。

## final
一个变量是final所修饰，那么在所有线程中看到的该变量是一致的。不可变意味着线程安全。

## volatile
java编程语言允许线程访问共享变量，为了确保共享变量能被准确和一致的更新，线程应该确保通过排他锁单独获得这个变量。
- [ ] 优点：读性能接近于普通读操作，写性能也非常高。
- [ ] 缺点：必须是原子操作，而且只能是一个变量。


## synchronized和lock
synchronized是jvm底层实现，而lock是java api级别的实现，底层依赖于CAS操作。
一般来说，lock的性能高于synchronized，在jdk1.6以后，synchronized性能有很大改善。两者性能已经很接近。


## happen-before原则
定义：如果操作a happens-before 操作b，那么b操作必须知道a的操作，或者说a的操作一定会通知到b，而不是a一定发生在b前。
### 程序次序规则（Program Order Rule）
在一个线程内，按照程序代码顺序，书写在前面的操作先行 发生于书写在后面的操作。准确地说，应该是控制流顺序而不是程序代码顺序，因为要考虑分支、循环 等结构。

### 管程锁定规则（Monitor Lock Rule）
一个unlock操作先行发生于后面对同一个锁的lock操作。这里 必须强调的是同一个锁，而“后面”是指时间上的先后顺序。

### volatile变量规则（Volatile Variable Rule）
对一个volatile变量的写操作先行发生于后面对这个变量的 读操作，这里的“后面”同样是指时间上的先后顺序。

### 线程启动规则（Thread Start Rule）
Thread对象的start（）方法先行发生于此线程的每一个动作。

### 线程终止规则（Thread Termination Rule）
线程中的所有操作都先行发生于对此线程的终止检测， 我们可以通过Thread.join（）方法结束、Thread.isAlive（）的返回值等手段检测到线程已经终止执行。

### 线程中断规则（Thread Interruption Rule）
对线程interrupt（）方法的调用先行发生于被中断线程 的代码检测到中断事件的发生，可以通过Thread.interrupted（）方法检测到是否有中断发生。

### 对象终结规则（Finalizer Rule）
一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize（）方法的开始。

### 传递性（Transitivity）
如果操作A先行发生于操作B，操作B先行发生于操作C，那就可以得出操 作A先行发生于操作C的结论。

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


> 参考《java并发编程的艺术》，《深入理解java虚拟机第二版》。
