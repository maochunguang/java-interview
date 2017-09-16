# java集合总结：集合的数据结构和性能对比

## 集合遍历方式
1. 普通for循环
2. for Each循环
4. Iterator迭代

## list,
ArrayList(数据结构)
LinkedList(链表结构)

循环删除元素：Iterator(迭代器循环)

## set
HashSet

## map
HashMap(数组加链表结构): 没有hash冲突的时候，get时间复杂度o(1),有冲突，先根据hashcode获取数组的下标，然后循环链表，根据key值找到对应元素。  

LinkedHashMap()

TreeMap(红黑树)，TreeMap是如何保证其迭代输出是有序的呢？其实从宏观上来讲，就相当于树的中序遍历(LDR)

## queue
ArrayBlockingQueue

## 并发容器
ConrrentHashMap
HashTable
Vector


