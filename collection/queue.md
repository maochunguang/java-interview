# queue
队列是限制结点插入操作固定在一端进行,而结点的删除操作固定在另一端进行的线性表.
队列犹如一个两端开口的管道.允许插入的一端称为队头,允许删除的一端称为队尾.队头和队尾各用一个”指针”指示,称为队头指针和队尾指针.不含任何结点的队列称为”空队列”.队列的特点是结点在队列中的排队次序和出队次序按进队时间先后确定,即先进队者先出队.因此,队列又称先进先出表.简称FIFO(first in first out)表.

## AQS(AbstractQueuedSynchronizer)
类如其名，抽象的队列式的同步器，AQS定义了一套多线程访问共享资源的同步器框架，许多同步类实现都依赖于它，如常用的ReentrantLock/Semaphore/CountDownLatch...。

[aqs详解](https://www.cnblogs.com/waterystone/p/4920797.html)

## ArrayBlockingQueue

## LinkedBlockingQueue

