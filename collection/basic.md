## Java的基本数据类型都有哪些各占几个字节?
* byte 1
* boolean 1（boolean 类型比较特别可能只占一个 bit，多个 boolean 可能共同占用一个字节）
* char 2
* short 2
* int 4
* float 4
* double 8
* long 8

## new Integer(8).equals(new Integer(10))是否为true？
是true，interger内部有缓存，-128-127之间的数直接走缓存，integer使用享元模式，共用了对象。
