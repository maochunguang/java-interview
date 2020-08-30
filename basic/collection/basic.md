## Java的基本数据类型都有哪些各占几个字节?
* byte 1
* boolean 1（boolean 类型比较特别可能只占一个 bit，多个 boolean 可能共同占用一个字节）
* char 2
* short 2
* int 4
* float 4
* double 8
* long 8

## java集合类图
![java集合类图](../../images/collection.png)

所有的集合类，都实现了Iterator接口，这是一个用于遍历集合中元素的接口，主要包含 hashNext(),next(),remove()三种方法。它的一个子接口LinkedIterator在它的基础上又添加了三种方法，分别是 add(),previous(),hasPrevious()。也就是说如果是先Iterator接口，那么在遍历集合中元素的时候，只能往后遍历，被 遍历后的元素不会在遍历到，通常无序集合实现的都是这个接口，比如HashSet，HashMap；

而那些元素有序的集合，实现的一般都是 LinkedIterator接口，实现这个接口的集合可以双向遍历，既可以通过next()访问下一个元素，又可以通过previous()访问前一个 元素，比如ArrayList。
