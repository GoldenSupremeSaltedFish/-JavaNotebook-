## 基础的数据结构
### 说说 List, Set, Queue, Map 四者的区别？

#### list(linklist和arraylist)


#### arrylist和arry的区别

ArrayList 内部基于动态数组实现，比 Array（静态数组） 使用起来更加灵活：

ArrayList会根据实际存储的元素动态地扩容或缩容，而 Array 被创建之后就不能改变它的长度了。

ArrayList 允许你使用泛型来确保类型安全，Array 则不可以。

ArrayList 中只能存储对象。对于基本类型数据，需要使用其对应的包装类（如 Integer、Double 等）。Array 可以直接存储基本类型数据，也可以存储对象。

ArrayList 支持插入、删除、遍历等常见操作，并且提供了丰富的 API 操作方法，比如 add()、remove()等。Array 只是一个固定长度的数组，只能按照下标访问其中的元素，不具备动态添加、删除元素的能力。

ArrayList创建时不需要指定大小，而Array创建时必须指定大小。

#### 出发点：
#### 插入操作的时间复杂度

#### 线程安全

#### set

#### hashset和currenthashset

#### hashtable

数组+链表组成的，数组是 Hashtable 的主体，链表则是主要为了解决哈希冲突而存在的


#### quene

是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 先进先出（FIFO） 规则

#### ArrayDeque 与 LinkedList 的区别

ArrayDeque 和 LinkedList 都实现了 Deque 接口，两者都具有队列的功能，但两者有什么区别呢？

ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。

ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。

ArrayDeque 是在 JDK1.6 才被引入的，而LinkedList 早在 JDK1.2 时就已经存在。

ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

### string集合

#### string

####string为什么不可变

用final修饰了，防止修改导致基础包的不安全

#### string字符串的拼接

编译器不会创建单个 StringBuilder 以复用，会导致创建过多的 StringBuilder 对象（操作+）

如果直接使用 StringBuilder 对象进行字符串拼接的话，就不会存在这个问题了。

在 JDK 9 中，字符串相加“+”改为用动态方法 makeConcatWithConstants() 来实现，通过提前分配空间从而减少了部分临时对象的创建。然而这种优化主要针对简单的字符串拼接，如： a+b+c 。对于循环中的大量拼接操作，仍然会逐个动态分配内存（类似于两个两个 append 的概念），并不如手动使用 StringBuilder 来进行拼接效率高。

#### 字符串常量池

字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

#### 一个小例子

如果保存了的话直接返回，如果没有保存的话，会在堆中创建对应的字符串对象并将该字符串对象的引用保存到字符串常量池中。

如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”

#### string中的equal已经被重载，ieda会推荐切换

## 异常

### try catch final（意义，一定会被执行吗）

#### 关于exception

#### Checked Exception 和 Unchecked Exception 有什么区别？

Checked Exception 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 catch或者throws 关键字处理的话，就没办法通过编译。

除了RuntimeException及其子类以外，其他的Exception类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、ClassNotFoundException、SQLException

Unchecked Exception 即 不受检查异常 ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

#### 异常抛出的要有意义，减少无意义的异常

## IO流

### OutputStream（字节输出流）

OutputStream 常用方法：

write(int b)：将特定字节写入输出流。

write(byte b[ ]) : 将数组b 写入到输出流，等价于 write(b, 0, b.length) 。

write(byte[] b, int off, int len) : 在write(byte b[ ]) 方法的基础上增加了 off 参数（偏移量）和 len 参数（要读取的最大字节数）。

flush()：刷新此输出流并强制写出所有缓冲的输出字节。

close()：关闭输出流释放相关的系统资源。

#### 缓冲流

减少io的次数提高性能

#### BufferedInputStream（字节缓冲输入流）

BufferedInputStream 从源头（通常是文件）读取数据（字节信息）到内存的过程中不会一个字节一个字节的读取，而是会先将读取到的字节存放在缓存区，并从内部缓冲区中单独读取字节。这样大幅减少了 IO 次数，提高了读取效率。

#### 字符缓冲流

BufferedReader （字符缓冲输入流）和 BufferedWriter（字符缓冲输出流）类似于 BufferedInputStream（字节缓冲输入流）和BufferedOutputStream（字节缓冲输入流），内部都维护了一个字节数组作为缓冲区。不过，前者主要是用来操作字符信息。

#### 随机缓冲流

这里要介绍的随机访问流指的是支持随意跳转到文件的任意位置进行读写的 RandomAccessFile 。

RandomAccessFile 的构造方法如下，我们可以指定 mode（读写模式）
```java
// openAndDelete 参数默认为 false 表示打开文件并且这个文件不会被删除
public RandomAccessFile(File file, String mode)
    throws FileNotFoundException {
    this(file, mode, false);
}
// 私有方法
private RandomAccessFile(File file, String mode, boolean openAndDelete)  throws FileNotFoundException{

}

```

#### 面对大文件时（分片上传）

分片多线程
