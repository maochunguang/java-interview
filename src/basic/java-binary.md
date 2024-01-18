# java中二进制操作符号

## 移位操作符
* ">>" 是带符号右移，若左操作数是正数，则高位补“0”，若左操作数是负数，则高位补“1”.
* "<<" 将左操作数向左边移动，并且在低位补0.
* ">>>" 是无符号右移，无论左操作数是正数还是负数，在高位都补“0”

三种移位符号作用的左操作数有五种：long,int,short,byte,char但是在作用不同的操作数类型时，其具体过程不同, 遵循一下几个原则：
1. int移位时，左边的操作数是32位的，此时的移位符号作用在32位bit上。如：1 >> 3, 是将00000000 00000000 00000000 00000001这32位向右边移动3位。
2. long 移位时，左边的操作数是64位的，此时移位符号作用在64位bit上。如：1L >> 3。
3. short, byte,char 在移位之前首先将数据转换为int，然后再移位，此时移位符号作用在32为bit上。如：(byte)0xff >>> 7, 是将11111111 11111111 11111111 11111111向右边移动7位，得到00000001 11111111 11111111 11111111
4. 当左操作数是long时，移位之后得到的类型是long，当左操作数是其它四中类型时，移位之后得到的类型是int，所以如果做操作数是byte,char,short 时，你用　>>=,>>>=, <<= 其实是将得到的int 做低位截取得到的数值。
5. 三种移位符号除了对做操作数有操作规则外，其实对右操作数也有操作规则。如果左操作数（转换之后的）是int,那么右操作数只有低５位有效（因为int总共就32位，11111b = 31，所以规定移动32位相当于没有移动）。如：23 >> 33, 结果与23 >>1是一样的，都是11；同理，如果左边操作数是long，那么右边操作数只有低6位有效。

```java
System.out.println(0xff >>> 7);
/*
0xff 本身就是一个int，其bits为：
00000000 00000000 00000000 11111111
无符号向右移动7位， 得到的bits为：
00000000 00000000 00000000 00000001
*/

System.out.println( ((byte) 0xff) >>> 7 );
/*
(byte)0xff 是一个byte，bits为：
11111111
首先转换为int，其bits为：
11111111 11111111 11111111 11111111
向右边无符号移动7为，得到的结果bits是：
00000001 11111111 11111111 11111111
*/

System.out.println(  (byte) (((byte) 0xff) >>> 7)  );
/*
(byte) 0xff 是一个byte，bits为：
11111111
首先转换为int，其bits为：
11111111 11111111 11111111 11111111
向右边无符号移动7为，得到的结果bits是：
00000001 11111111 11111111 11111111
然后转换为byte，低位截取得到bits:11111111
<在输出的时候转换为int，其bits为：
11111111 11111111 11111111 11111111>
*/

```

## 位异或操作（^）
运算规则：两个数转为二进制，然后从高位比较，如果相同则为0，不相同为1。
```
比如：8^11.
8转为二进制是1000，11转为二进制是1011，从高位开始比较得到的是：0011.然后二进制转为十进制，就是`Integer.parseInt("0011",2)=3;`
```
## 位与运算符（&）
运算规则：两个数转为二进制，然后从高位比较，如果两个数都是1则为1，否则为0。
```
比如：129&128.
129转换成二进制就是10000001，128转换成二进制就是10000000。从高位开始比较得到，得到10000000，即128。
```
## 位或运算符（|）
运算规则：两个数都转为二进制，然后从高位开始比较，两个数只要有一个为1则为1，否则为0.
```
比如：129|128.
129转换成二进制就是10000001，128转换成二进制就是10000000。从高位开始比较得到，得到10000001，即129.
```
## 位非运算符（~）
运算规则：如果位为0，结果是1，如果位为1，结果是0.
```
比如：~37
在Java中，所有数据的表示方法都是以补码的形式表示，如果没有特殊说明，Java中的数据类型默认是int,int数据类型的长度是8位，一位是四个字节，就是32字节，32bit.
8转为二进制是100101.
补码后为： 00000000 00000000 00000000 00100101
取反为：   11111111 11111111 11111111 11011010
因为高位是1，所以原码为负数，负数的补码是其绝对值的原码取反，末尾再加1。
因此，我们可将这个二进制数的补码进行还原：
首先，末尾减1得反码： 11111111 11111111 11111111 11011001
其次，将各位取反得原码：00000000 00000000 00000000 00100110，
此时二进制转原码为38
所以~37 = -38.
```


## 参考链接：
> https://blog.csdn.net/xuejianbest/article/details/84792311
> https://www.cnblogs.com/yesiamhere/p/6675067.html