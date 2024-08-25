### 这种思路就是java的聚合思路，将java中数据类型中相似的地方抽离出来，在别的数据类型中只需要根据特性进行添加，而不需要再进行基础数据类型的实现。

### ArrayList（基于数组的实现）

支持快速的随机访问;插入以及删除效率低


线程安全性：ArrayList 不是线程安全的。如果需要线程安全，可以使用 Collections.synchronizedList(new ArrayList<>()) 或使用 CopyOnWriteArrayList

### LinkedList（基于双向链表实现）

特点：LinkedList 基于双向链表实现。适用于频繁插入和删除元素的场景，尤其是当涉及到中间位置的操作时。

优点：插入和删除操作较快，不需要像 ArrayList 那样移动元素。

缺点：随机访问速度较慢，因为必须从头或尾遍历链表。

线程安全性：LinkedList 也不是线程安全的。如果需要线程安全，可以考虑使用外部同步机制。

#### 外部同步机制

加锁的方法一般为插入和删除，

在使用 LinkedList 时，可以在操作前后使用 synchronized 关键字来对整个操作加锁（这个锁已经不是重量级锁了，锁的粒度还能接受）

自定义线程安全的包装类

需要对所有的操作都加锁
```java
public class SynchronizedLinkedList<E> {
    private final LinkedList<E> list = new LinkedList<>();

    public synchronized void add(E element) {
        list.add(element);
    }

    public synchronized E remove(int index) {
        return list.remove(index);
    }

    public synchronized E get(int index) {
        return list.get(index);
    }

    // 其他方法同样需要加锁...
}
```
#### 以上代码的问题，对所有的操作进行加锁，导致可以并发的查询操作也被过度阻塞

