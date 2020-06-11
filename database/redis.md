# redis总结

## redis的特性
1. 单线程，避免线程切换和锁操作
2. 高性能，单机并发量达到万级
3. 支持集群模式

## redis单线程高性能的原因
1. 纯内存访问，内存的响应时间是100ns，redis并发达到每秒w级
2. 非阻塞io，redis使用epoll作为多路复用的实现，redis基于epoll实现了自己的事件处理模型。将epoll中的连接、读写、关闭都转换为事件，不在网络I/O上浪费过多的时间。
3. 单线程避免了锁竞争和线程切换，以及线程创建。
4. 单线程有利于高性能数据结构的实现。

## redis单线程的问题
对于每个命令都是有执行时间的，如果某个命令执行时间过长，会造成其它命令阻塞，对高性能来说是致命的。

## redis应用场景
1. 消息队列系统，利用redis的发布订阅和阻塞功能
2. 分布式锁，利用setnx或者lua脚本实现原子操作
3. 缓存，分布式缓存，提高系统的响应速度
4. 排行榜系统，使用redis的列表和有序集合
5. 计数器应用

## redis不适合的场景
1. 从数据规模，过大的数据不适合放在redis中
2. 从数据类型，分为冷数据和热数据（频繁操作的），reids存冷数据有些浪费资源


## redis的基本数据类型
### 1. string类型
数据结构：自定义SDS，不是c语言字符串
```c++
struct sdshdr {
//记录buf数组中已使⽤字节的数量
//等于SDS所保存字符串的⻓度
int len;
//记录buf
数组中未使⽤字节的数量
int free;
//字节数组， ⽤于保存字符串
char buf[];
}
```
**和c字符串相比优缺点如下：**
c字符串 | SDS字符串 | 
---------|----------|
 获取长度复杂度O(N) | 获取长度复杂度O(N)  |
 API不安全，缓冲区可能溢出| API安全，缓冲区不会溢出 | 
 修改字符串N次内存分配N次 | 内存分配次数最多为N次 | 
 只能保持文本书籍 | 可以保存文本和二进制数据 |
 可以使用所有<string.h>库函数|可以使用部分<string.h>库函数| 

### SDS内存分配
如果对SDS进⾏修改之后， SDS的长度（也即是len属性的值） 将小于1MB， 那么程序分配和len属性同样⼤小的未使用空间， 这时SDS len属性的值将和free属性的值相同，大于1M直接分配1M的未使用空间。
在扩展SDS空间之前， SDS API会先检查未使⽤空间是否⾜够， 如果⾜够的话， API就会直接使⽤未使⽤空间， ⽽⽆须执⾏内存重分配。
用途：1.字符串存储，数字，二进制数据也可以，大小（512m以内）

惰性空间释放⽤于优化SDS的字符串缩短操作： 当SDS的API需要缩短SDS保存的字符串时，
程序并不⽴即使⽤内存重分配来回收缩短后多出来的字节， ⽽是使⽤free属性将这些字节的数量
记录起来， 并等待将来使⽤

### 2. 字典（hash表）
数据结构：hash表，hash节点，字典（Redis使⽤MurmurHash2算法来计算键的哈希值。）
```c++
typedef struct dict {
    //类型特定函数
    dictType *type;
    //私有数据
    void *privdata;
    //哈希表
    dictht ht[2];
    // rehash索引
    //当rehash不在进⾏时， 值为-1
    in trehashidx; 
} dict;


typedef struct dictht {
    //哈希表数组
    dictEntry **table;
    //哈希表⼤⼩
    unsigned long size;
    //哈希表⼤⼩掩码， ⽤于计算索引值
    //总是等于size-1
    unsigned long sizemask;
    //该哈希表已有节点的数量
    unsigned long used;
} dictht;

typedef struct dictEntry {
    //键
    void *key;
    //值
    union{
        void *val;
        uint64_tu64;
        int64_ts64;
} v;
//指向下个哈希表节点， 形成链表
struct dictEntry *next;
} dictEntry;
```
### redis如何解决hash表问题
**1、redis如何计算hashcode**
```
Redis使⽤MurmurHash2算法来计算键的哈希值。
```
**2、如何解决hash冲突**
```
Redis的哈希表使⽤链地址法（separate chaining） 来解决键冲突， 每个哈希表节点都有⼀个next指针，多个哈希表节点可以⽤next指针构成⼀个单向链表，被分配到同⼀个索引上的多个节
点可以⽤这个单向链表连接起来，这就解决了键冲突的问题。
```
**3、rehash**
![redis rehash流程](../images/database/redis-rehash.png)
* 1） 为字典的ht[1]哈希表分配空间， 这个哈希表的空间⼤⼩取决于要执⾏的操作， 以及ht[0]当前包含的键值对数量（也即是ht[0].used属性的值） ：
    * ·如果执⾏的是扩展操作， 那么ht[1]的⼤小为第⼀个⼤于等于ht[0].used*2的2 n（2的n次⽅幂） ；
    * ·如果执⾏的是收缩操作， 那么ht[1]的⼤小为第⼀个⼤于等于ht[0].used的2 n。
* 2） 将保存在ht[0]中的所有键值对rehash到ht[1]上⾯：rehash指的是重新计算键的哈希值和索引值， 然后将键值对放置到ht[1]哈希表的指定位置上。
* 3） 当ht[0]包含的所有键值对都迁移到了ht[1]之后（ht[0]变为空表） ， 释放ht[0]， 将ht[1]设置为ht[0]， 并在ht[1]新创建⼀个空⽩哈希表， 为下⼀次rehash做准备。
**4、渐进式rehash**
因为在进⾏渐进式rehash的过程中，字典会同时使⽤ht[0]和ht[1]两个哈希表，所以在渐进式rehash进⾏期间，字典的删除（delete）、查找（find）、更新（update）等操作会在两个哈希表上进⾏。例如，要在字典⾥⾯查找⼀个键的话，程序会先在ht[0]⾥⾯进⾏查找， 如果没找到的话，就会继续到ht[1]⾥⾯进⾏查找，诸如此类。
另外，在渐进式rehash执⾏期间，新添加到字典的键值对⼀律会被保存到ht[1]⾥⾯， ⽽ht[0]则不再进⾏任何添加操作， 这⼀措施保证了ht[0]包含的键值对数量会只减不增， 并随着rehash操
作的执⾏⽽最终变成空表。

### 3. list
数据结构：自定义的双向链表，listNode，list
```c++
typedef struct listNode {
//前置节点
struct listNode * prev;
//后置节点
struct listNode * next;
//节点的值
void * value;
}listNode;

typedef struct list {
//表头节点
listNode * head;
//表尾节点
listNode * tail;
//链表所包含的节点数量
unsigned long len;
//节点值复制函数
void *(*dup)(void *ptr);
//节点值释放函数
void (*free)(void *ptr);
//节点值对⽐函数
int (*match)(void *ptr,void *key);} list
```
* ·链表被⼴泛用于实现Redis的各种功能， 比如列表键、 发布与订阅、 慢查询、 监视器等。
* ·每个链表节点由⼀个listNode结构来表示， 每个节点都有⼀个指向前置节点和后置节点的指针， 所以Redis的链表实现是双端链表。
* ·每个链表使用⼀个list结构来表示， 这个结构带有表头节点指针、 表尾节点指针， 以及链表长度等信息。
* ·因为链表表头节点的前置节点和表尾节点的后置节点都指向NULL， 所以Redis的链表实现是⽆环链表。
* ·通过为链表设置不同的类型特定函数， Redis的链表可以用于保存各种不同类型的值。

### 4. set
数据结构：set

特性：元素是无序的，元素不可以重复

用途：

### 5. sortedset
数据结构：ziplist，skiplist，intset
#### skipList
特性：元素是有序的，元素不可以重复
```c++
typedef struct zskiplist {
    //表头节点和表尾节点
    struct zskiplistNode *header, *tail;
    //表中节点的数量
    unsigned long length;
    //表中层数最⼤的节点的层数
    int level;
} zskiplist;

typedef struct zskiplistNode {
    //层
    struct zskiplistLevel {
        //前进指针
        struct zskiplistNode *forward;
        //跨度
        unsigned int span;
    } level[];
    //后退指针
    struct zskiplistNode *backward;
    //分值
    double score;
    //成员对象
    robj *obj;
} zskiplistNode;
```
#### intset
整数集合（intset） 是集合键的底层实现之⼀， 当⼀个集合只包含整数值元素， 并且这个集合的元素数量不多时， Redis就会使⽤整数集合作为集合键的底层实现

```c++
typedef struct intset {
    //编码⽅式
    uint32_t encoding;
    //集合包含的元素数量
    uint32_t length;
    //保存元素的数组
    int8_t contents[];
} intset;
```
虽然intset结构将contents属性声明为int8_t类型的数组， 但实际上contents数组并不保存任何int8_t类型的值， contents数组的真正类型取决于encoding属性的值
* 如果encoding属性的值为INTSET_ENC_INT16， 那么contents就是⼀个int16_t类型的数组，数组里的每个项都是⼀个int16_t类型的整数值（最小值为-32768， 最⼤值为32767） 
* 如果encoding属性的值为INTSET_ENC_INT32， 那么contents就是⼀个int32_t类型的数组，数组里的每个项都是⼀个int32_t类型的整数值（最小值为-2147483648， 最⼤值为2147483647） 。
* 如果encoding属性的值为INTSET_ENC_INT64， 那么contents就是⼀个int64_t类型的数组，数组里的每个项都是⼀个int64_t类型的整数值（最小值为-9223372036854775808， 最⼤值为
9223372036854775807）

**intset类型升级操作**
1） 根据新元素的类型，扩展整数集合底层数组的空间⼤⼩，并为新元素分配空间。
2） 将底层数组现有的所有元素都转换成与新元素相同的类型， 并将类型转换后的元素放置到正确的位上， ⽽且在放置元素的过程中， 需要继续维持底层数组的有序性质不变。
3） 将新元素添加到底层数组⾥⾯。

**intset优缺点**
1. 整数集合的升级策略有两个好处，⼀个是提升整数集合的灵活性， 另⼀个是尽可能地节约内存
2. 整数集合不⽀持降级操作，⼀旦对数组进⾏了升级，编码就会⼀直保持升级后的状态。

### ziplist
压缩列表（ziplist） 是列表键和哈希键的底层实现之⼀。 当⼀个列表键只包含少量列表项，并且每个列表项要么就是⼩整数值， 要么就是长度⽐较短的字符串， 那么Redis就会使⽤压缩列表来做列表键的底层实现。

压缩列表是Redis为了节约内存⽽开发的， 是由⼀系列特殊编码的连续内存块组成的顺序型（sequential） 数据结构。 ⼀个压缩列表可以包含任意多个节点（entry） ， 每个节点可以保存⼀个字节数组或者⼀个整数值。

### 数据存储对象
Redis使⽤对象来表⽰数据库中的键和值， 每次当我们在Redis的数据库中新创建⼀个键值对时，我们⾄少会创建两个对象，⼀个对象⽤作键值对的键（键对象），另⼀个对象⽤作键值对的
值（值对象）。
```c++
typedef struct redisObject {
    //类型
    unsigned type:4;
    //编码
    unsigned encoding:4;
    //指向底层实现数据结构的指针
    void *ptr;
    // ...
} robj;
```


## redis过期删除策略
### 过期删除的三种策略：
* ·定时删除：在设置键的过期时间的同时，创建⼀个定时器（timer），让定时器在键的过期时间来临时，立即执⾏对键的删除操作。
  * 优点：内存释放效率高（内存），
  * 缺点：容易造成cpu负载大（cpu），时间复杂度高
* ·惰性删除： 放任键过期不管， 但是每次从键空间中获取键时， 都检查取得的键是否过期，如果过期的话， 就删除该键； 如果没有过期， 就返回该键。
  * 优点：cpu资源占用少，
  * 缺点：内存释放不及时，
* ·定期删除： 每隔⼀段时间， 程序就对数据库进⾏⼀次检查， 删除里面的过期键。 ⾄于要删除多少过期键， 以及要检查多少个数据库， 则由算法决定。
  * 优点：内存释放效率高，cpu占用适中
  * 缺点：确定执行时长和频率比较困难

### redis使用的删除策略
Redis服务器实际使⽤的是惰性删除和定期删除两种策略：通过配合使⽤这两种删除策略，服务器可以很好地在合理使⽤CPU时间和避免浪费内存空间之间取得平衡

#### redis惰性删除实现
过期键的惰性删除策略由db.c/expireIfNeeded函数实现，所有读写数据库的Redis命令在执⾏之前都会调⽤expireIfNeeded函数对输⼊键进⾏检查：
* ·如果输⼊键已经过期，那么expireIfNeeded函数将输⼊键从数据库中删除。
* ·如果输⼊键未过期，那么expireIfNeeded函数不做动作

#### redis定期删除实现
过期键的定期删除策略由redis.c/activeExpireCycle函数实现， 每当Redis的服务器周期性操作redis.c/serverCron函数执⾏时， activeExpireCycle函数就会被调⽤，它在规定的时间内，分多次遍历服务器中的各个数据库，从数据库的expires字典中随机检查⼀部分键的过期时间，并删除其中
的过期键

#### 持久化对过期的处理
RDB文件生成时，如果K2键过期，不会写入到文件里。
RDB文件加载时，如果数据库中包含三个键k1、 k2、 k3， 并且k2已经过期， 那么当服务器启动时：
* ·如果服务器以主服务器模式运⾏， 那么程序只会将k1和k3载⼊到数据库， k2会被忽略。
* ·如果服务器以从服务器模式运⾏， 那么k1、 k2和k3都会被载⼊到数据库。

AOF文件写入时：如果数据库中的某个键已经过期， 但它还没有被惰性删除或者定期删除， 那么AOF⽂件不会因为这个过期键⽽产⽣任何影响。当过期键被惰性删除或者定期删除之后， 程序会向AOF⽂件追加（append） ⼀条DEL命令，来显式地记录该键已被删除。

AOF文件重写时：如果数据库中包含三个键k1、k2、k3，并且k2已经过期，那么在进⾏重写⼯作时，程序只会对k1和k3进⾏重写， ⽽k2则会被忽略。

当服务器运⾏在复制模式下时， 从服务器的过期键删除动作由主服务器控制：
1. ·主服务器在删除⼀个过期键之后， 会显式地向所有从服务器发送⼀个DEL命令， 告知从服务器删除这个过期键。
2. ·从服务器在执⾏客户端发送的读命令时， 即使碰到过期键也不会将过期键删除， ⽽是继续像处理未过期的键⼀样来处理过期键。
3. ·从服务器只有在接到主服务器发来的DEL命令之后， 才会删除过期键。

## redis持久化策略
### 1、rdb持久化
1. 定时刷新内存，进行持久化。
   ```shell
   # 服务器在900秒之内， 对数据库进⾏了⾄少1次修改。
   # 服务器在300秒之内， 对数据库进⾏了⾄少10次修改。
   # 服务器在60秒之内， 对数据库进⾏了⾄少10000次修改
    save 900 1
    save 300 10
    save 60 10000
   ```
2. 手动触发SAVE命令和BGSAVE命令进行持久化。
   1. SAVE命令会阻塞Redis服务器进程，直到RDB⽂件创建完毕为⽌， 在服务器进程阻塞期间，服务器不能处理任何命令请求：
   2. BGSAVE命令会派⽣出⼀个⼦进程，然后由⼦进程负责创建RDB⽂件，服务器进程（⽗进程） 继续处理命令请求

服务器在载⼊RDB⽂件期间， 会⼀直处于阻塞状态， 直到载⼊⼯作完成为止。
### 2、aof持久化
与RDB持久化通过保存数据库中的键值对来记录数据库状态不同， AOF持久化是通过保存Redis服务器所执行的写命令来记录数据库状态的。
服务器配置appendfsync选项的值直接决定AOF持久化功能的效率和安全性。
* 当appendfsync的值为always时，服务器在每个事件循环都要将aof_buf缓冲区中的所有内容写⼊到AOF⽂件，并且同步AOF⽂件
* 当appendfsync的值为everysec时，服务器在每个事件循环都要将aof_buf缓冲区中的所有内容写⼊到AOF⽂件，并且每隔⼀秒就要在⼦线程中对AOF⽂件进⾏⼀次同步。
* 当appendfsync的值为no时，服务器在每个事件循环都要将aof_buf缓冲区中的所有内容写⼊到AOF⽂件，⾄于何时对AOF⽂件进⾏同步，则由操作系统控制


## redis事务处理
1. watch



## redis使用lua脚本的优势？


## redis的多路复用机制


## 快照的实现原理


## redis内存优化策略



## redis的集群模式
### 1. master-slave
主从复制的原理：

### 2. master-slave-sentinel
哨兵的原理：

### 3. redis-cluster
redis集群的原理：

增加节点：

删除节点：

## 其它集群方案
redis shardding

codis

twemproxy


> 参考《redis开发与运维》，《redis设计与实现》，两本书。
