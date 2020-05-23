# set
HashSet与TreeSet都是基于Set接口的实现类。其中TreeSet是Set的子接口SortedSet的实现类。Set接口及其子接口、实现类的结构如下所示：


        |——SortedSet接口——TreeSet实现类
Set接口——|——HashSet实现类
        |——LinkedHashSet实现类

## HashSet
HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。

## LinkedHashSet
内部使用LinkedHashMap作为容器实现

## TreeSet
TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。


## LinkedHashSet
LinkedHashSet：具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。

## HashMap 和 HashSet区别
如果你看过 HashSet 源码的话就应该知道：HashSet 底层就是基于 HashMap 实现的。（HashSet 的源码非常非常少，因为除了 clone()、writeObject()、readObject()是 HashSet 自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法。

HashMap	HashSet
实现了Map接口	实现Set接口
存储键值对	仅存储对象
调用 put（）向map中添加元素	调用 add（）方法向Set中添加元素
HashMap使用键（Key）计算Hashcode	HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，