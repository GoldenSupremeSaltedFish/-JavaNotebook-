### hashmap是一个哈希槽，每个哈希槽对应着的是一个链表，链表长度》64会转换成红黑树


#### 为什么是64，根据泊松分布得出

#### 节点<6需要反树化

线程不安全以及哈希碰撞

HashMap是基于数组来实现哈希表的，数组就好比内存储空间，数组的index就好比内存的地址；

HashMap的每个记录就是一个Entry<K, V>对象，数组中存储的就是这些对象； 

HashMap的哈希函数 = 计算出hashCode + 计算出数组的index；

HashMap解决冲突：使用链地址法，每个Entry对象都有一个引用next来指向链表的下一个Entry；

HashMap的负载因子：默认为0.75；

### Linkedhashmap

为hashmap的子类，存在一个前后列表来维护插入的顺序.jdk1.8before指针和after指针

accessorder参数确定是否可以进行排序（你可以实现一个lru的算法，内存不会无限大，可以淘汰after指针的内容（最后的内容）实现最少使用）

（LRU）手写一个LRU

### hashtable

Hashtable的负载因子：默认为0.75（内存*负载因子为扩容极限）

