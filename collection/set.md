# set
HashSet与TreeSet都是基于Set接口的实现类。其中TreeSet是Set的子接口SortedSet的实现类。Set接口及其子接口、实现类的结构如下所示：


        |——SortedSet接口——TreeSet实现类
Set接口——|——HashSet实现类
        |——LinkedHashSet实现类

## HashSet
内部使用了hashMap作为容器实现，

## LinkedHashSet
内部使用LinkedHashMap作为容器实现

## TreeSet
内部使用TreeMap作为容器实现