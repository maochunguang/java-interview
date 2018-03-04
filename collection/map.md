## hashMap

## LinkedHashMap

## TreeMap

## map
HashMap(数组加链表结构): 没有hash冲突的时候，get时间复杂度o(1),有冲突，先根据hashcode获取数组的下标，然后循环链表，根据key值找到对应元素。

LinkedHashMap()

TreeMap(红黑树)，TreeMap是如何保证其迭代输出是有序的呢？其实从宏观上来讲，就相当于树的中序遍历(LDR)

map最高效的遍历方式：