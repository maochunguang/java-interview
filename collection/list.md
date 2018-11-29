## 数组
数组性能最高，但是无法动态扩容，操作不方便。

## ArrayList(jdk1.8)
实现了List<E>
底层数据结构是一个动态数组。更适合随机读取数据。

初始化容量：10
当容量满的时候进行扩容：
```java
private static final int DEFAULT_CAPACITY = 10;
int newCapacity = oldCapacity + (oldCapacity >> 1);
```

## LinkedList
实现了List<E>, Deque<E>两个接口

底层数据结构是一个双向链表。可以在任何位置进行高效地插入和删除操作的有序序列。

## ArrayList和LinkedList区别
1. ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。
2. 对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针。
3. 对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。


