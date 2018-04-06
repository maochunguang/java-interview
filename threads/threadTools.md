## threadLocal
### 原理
线程
### 优缺点

### 使用场景
上下文传参，线程私有变量，


## CountDownLatch
### 原理
一个线程调用countDown方法 happen-before 另外一个线程调用await方法。底层使用AQS队列来实现。
### 场景
假如有这样一个需求，当我们需要解析一个Excel里多个sheet的数据时，可以考虑使用多线程，每个线程解析一个sheet里的数据，等到所有的sheet都解析完之后，程序需要提示解析完成。在这个需求中，要实现主线程等待所有线程完成sheet的解析操作，最简单的做法是使用join,这里使用CountDownLatch。

```java
public class CountDownLatchTest {
	static CountDownLatch c = new CountDownLatch(2);

	public static void main(String[] args) throws InterruptedException {
		new Thread(new Runnable() {
			@Override
			public void run() {
				System.out.println(1);
				c.countDown();
			}
		}).start();
		c.await();
		System.out.println("3");
	}

}
```
## CyclicBarrier
CyclicBarrier 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。CyclicBarrier默认的构造方法是CyclicBarrier(int parties)，其参数表示屏障拦截的线程数量，每个线程调用await方法告诉CyclicBarrier我已经到达了屏障，然后当前线程被阻塞

```java
public class CyclicBarrierTest {
	static CyclicBarrier c = new CyclicBarrier(2);
	public static void main(String[] args) {
		new Thread(new Runnable() {
			@Override
			public void run() {
				try {
					c.await();
				} catch (Exception e) {
				}
				System.out.println(1);
			}
		}).start();

		try {
			c.await();
		} catch (Exception e) {

		}
		System.out.println(2);
	}
}
```

## CyclicBarrier和CountDownLatch的区别
* CountDownLatch的计数器只能使用一次。而CyclicBarrier的计数器可以使用reset() 方法重置。所以     CyclicBarrier能处理更为复杂的业务场景，比如如果计算发生错误，可以重置计数器，并让线程们重新执行一次。
* CyclicBarrier还提供其他有用的方法，比如getNumberWaiting方法可以获得CyclicBarrier阻塞的线程数量。isBroken方法用来知道阻塞的线程是否被中断。比如以下代码执行完之后会返回true

## Semaphore
Semaphore（信号量）是用来控制同时访问特定资源的线程数量，它通过协调各个线程，以保证合理的使用公共资源。
```java
public class SemaphoreTest {
	private static final int THREAD_COUNT = 30;
	private static ExecutorService threadPool = Executors
			.newFixedThreadPool(THREAD_COUNT);
	private static Semaphore s = new Semaphore(10);

	public static void main(String[] args) {
		for (int i = 0; i < THREAD_COUNT; i++) {
			threadPool.execute(new Runnable() {
				@Override
				public void run() {
					try {
						s.acquire();
						System.out.println("save data");
						s.release();
					} catch (InterruptedException e) {
					}
				}
			});
		}
		threadPool.shutdown();
	}
}
```

## Exchanger
Exchanger（交换者）是一个用于线程间协作的工具类。Exchanger用于进行线程间的数据交换。它提供一个同步点，在这个同步点两个线程可以交换彼此的数据。这两个线程通过exchange方法交换数据， 如果第一个线程先执行exchange方法，它会一直等待第二个线程也执行exchange，当两个线程都到达同步点时，这两个线程就可以交换数据，将本线程生产出来的数据传递给对方。

```java
public class ExchangerTest {
	private static final Exchanger<String> exgr = new Exchanger<String>();
	private static ExecutorService threadPool = Executors.newFixedThreadPool(2);

	public static void main(String[] args) {
		threadPool.execute(new Runnable() {
			@Override
			public void run() {
				try {
					String A = "银行流水A";// A录入银行流水数据
					exgr.exchange(A);
				} catch (InterruptedException e) {
				}
			}
		});

		threadPool.execute(new Runnable() {
			@Override
			public void run() {
				try {
					String B = "银行流水B";// B录入银行流水数据
					String A = exgr.exchange("B");
					System.out.println("A和B数据是否一致：" + A.equals(B) + ",A录入的是："
							+ A + ",B录入是：" + B);
				} catch (InterruptedException e) {
				}
			}
		});
		threadPool.shutdown();
	}
}
```