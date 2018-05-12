# java集合总结：集合的数据结构和性能对比
相比c或者c++而言，java提供了丰富的集合工具类，开发者几乎开箱即用。不需要自定义一堆集合类。


## 集合遍历方式
1. 普通for循环
2. for Each循环
3. Iterator迭代
4. lam 表达式

## list
功能：动态的数组，存普通的列表数据，有序。
ArrayList(数据结构)
LinkedList(链表结构)

循环删除元素：Iterator(迭代器循环)

## set
功能：用于去重，存储不同的元素，没有顺序。
HashSet
LinkedHashSet

## map
功能：存储key-value类型的数据，key不允许重复，没有顺序
HashMap
LinkedHashMap（有序）
TreeMap（能排序）

## 并发容器
功能：用于多线程场景下并发安全的容器。
ConrrentHashMap
HashTable（悲观锁）
Vector（悲观锁）


