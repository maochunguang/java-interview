# java多线程
## 线程和进程
1. 概念
    * 进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位。
    * 线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位.线程自己基本上不拥有系统资源,只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈),但是它可与同属一个进程的其他的线程共享进程所拥有的全部资源。

2. 区别
    1) 一个程序至少有一个进程,一个进程至少有一个线程.
    2) 线程的划分尺度小于进程，线程必进程更轻量级，使得多线程程序的并发性高。
    3) 进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率。
    4) 线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。
    5) 从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别。
## 线程的生命周期
![线程状态图](images/thread-status.png)
1. 新建状态
2. 就绪状态
3. 运行状态
4. 阻塞状态
5. 死亡状态

## 如何启动和销毁线程
1. 使用start方法启动线程
2. 销毁线程有三种
    * 使用变量标志
    * 使用stop（不推荐使用）
    * 使用interrupt

## 如何判断线程的状态


## interrupt、interrupted 、isInterrupted 区别
interrupt()进行线程中断，调用该方法的线程的状态为将被置为"中断"状态
interrupted 是作用于当前线程，会清除中断状态
isInterrupted 是作用于调用该方法的线程对象所对应的线程
## 多线程的实现方式
1. 继承Thread类
2. 实现Runnable接口
## 同步的方式
1. syncronized方法



## 死锁
死锁是指多个进程循环等待它方占有的资源而无限期地僵持下去的局面。
## 线程池的使用和原理
1. corePoolSize：线程池中的核心线程数，当提交一个任务时，线程池创建一个新线程执行任务，直到当前线程数等于corePoolSize；
如果当前线程数为corePoolSize，继续提交的任务被保存到阻塞队列中，等待被执行；
如果执行了线程池的prestartAllCoreThreads()方法，线程池会提前创建并启动所有核心线程。

2. maximumPoolSize：线程池中允许的最大线程数。如果当前阻塞队列满了，且继续提交任务，则创建新的线程执行任务，前提是当前线程数小于maximumPoolSize

3. keepAliveTime：线程空闲时的存活时间，即当线程没有任务执行时，继续存活的时间。默认情况下，该参数只在线程数大于corePoolSize时才有用

4. workQueue：必须是BlockingQueue阻塞队列。当线程池中的线程数超过它的corePoolSize的时候，线程会进入阻塞队列进行阻塞等待。通过workQueue，线程池实现了阻塞功能
5. RejectedExecutionHandler（饱和策略）
线程池的饱和策略，当阻塞队列满了，且没有空闲的工作线程，如果继续提交任务，必须采取一种策略处理该任务，线程池提供了5种策略：
    * （1）AbortPolicy：直接抛出异常，默认策略；
    * （2）CallerRunsPolicy：用调用者所在的线程来执行任务；
    * （3）DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；
    * （4）DiscardPolicy：直接丢弃任务；
    * （5）实现RejectedExecutionHandler接口，自定义饱和策略

### 几种排队的策略：
1. 不排队，直接提交
将任务直接交给线程处理而不保持它们，可使用SynchronousQueue
如果不存在可用于立即运行任务的线程（即线程池中的线程都在工作），则试图把任务加入缓冲队列将会失败，因此会构造一个新的线程来处理新添加的任务，并将其加入到线程池中（corePoolSize-->maximumPoolSize扩容）
Executors.newCachedThreadPool()采用的便是这种策略

2. 无界队列
可以使用LinkedBlockingQueue（基于链表的有界队列，FIFO），理论上是该队列可以对无限多的任务排队，将导致在所有corePoolSize线程都工作的情况下将新任务加入到队列中。这样，创建的线程就不会超过corePoolSize，也因此，maximumPoolSize的值也就无效了

3. 有界队列
可以使用ArrayBlockingQueue（基于数组结构的有界队列，FIFO），并指定队列的最大长度
使用有界队列可以防止资源耗尽，但也会造成超过队列大小和maximumPoolSize后，提交的任务被拒绝的问题，比较难调整和控制。

### execute 和 submit接口的区别
1. 接受参数，前者是Runnable接口，后者是Callable接口
2. 返回值，前者没有返回值，后者返回Futrue对象
3. 处理异常

### 线程池终止的方法
1. shutdown， 当线程池调用该方法时,线程池的状态则立刻变成SHUTDOWN状态。此时，则不能再往线程池中添加任何任务，否则将会抛出RejectedExecutionException异常。但是，此时线程池不会立刻退出，直到添加到线程池中的任务都已经处理完成，才会退出。 
2. shutdownNow，执行该方法，线程池的状态立刻变成STOP状态，并试图停止所有正在执行的线程，不再处理还在池队列中等待的任务，当然，它会返回那些未执行的任务。 它试图终止线程的方法是通过调用Thread.interrupt()方法来实现的，如果线程中没有sleep 、wait、Condition、定时锁等应用, interrupt()方法是无法中断当前的线程的。所以，ShutdownNow()并不代表线程池就一定立即就能退出，它可能必须要等待所有正在执行的任务都执行完成了才能退出。

## FixedThreadPool
固定线程池大小的，队列可以是有界也可以无界
## CachedThreadPool
大小无界的线程池，队列是无界的

## SingleThreadPool
线程池大小为1，

## ScheduledThreadPool
执行定时任务的线程池

## 实现生产者消费者
```java
public class Restaurant {
    Meal meal;
    WaitPerson waitPerson = new WaitPerson(this);
    ExecutorService exec = Executors.newCachedThreadPool();
    Chef chef = new Chef(this);
    public Restaurant(){
        exec.execute(waitPerson);
        exec.execute(chef);
    }
    public static void main(String[] args) {
        new Restaurant();
    }
}
class Meal{
    private final int orderNum;
    public Meal(int orderNum){
        this.orderNum = orderNum;
    }
    @Override
    public String toString() {
        return "Meal "+ orderNum;
    }
}
class WaitPerson implements Runnable{
    private Restaurant restaurant;
    public WaitPerson(Restaurant r){
        this.restaurant = r;
    }
    @Override
    public void run() {
        try {
            while(!Thread.interrupted()){
                synchronized (this){
                    while (restaurant.meal==null){
                        wait();
                    }
                    System.out.println("waitPerson get"+ restaurant.meal);
                }
                synchronized (restaurant.chef){
                    restaurant.meal = null;
                    restaurant.chef.notifyAll();
                }
            }
        }catch (InterruptedException e){
            System.out.println(" waitPerson interrupted");
        }
    }
}
class Chef implements Runnable{
    private Restaurant restaurant;
    private int count = 0;
    public Chef(Restaurant r){
        this.restaurant = r;
    }
    @Override
    public void run() {
        try {
            while(!Thread.interrupted()){
                synchronized (this){
                    while (restaurant.meal!=null){
                        wait();
                    }
                }
                if(++count == 10){
                    System.out.println("out of food, closing");
                    restaurant.exec.shutdownNow();
                }
                System.out.println("Order up!");
                synchronized (restaurant.waitPerson){
                    restaurant.meal = new Meal(count);
                    restaurant.waitPerson.notifyAll();
                }
                TimeUnit.MILLISECONDS.sleep(100);
            }
        }catch (InterruptedException e){
            System.out.println(" chef interrupted");
        }
    }
}
```

## CAS操作与ABA问题
CAS操作是jdk新的cpu指令，
