### hash函数的设计，速度要快，均匀分布。

### 拉链法阻止冲突（长度超过多少64变成红黑树），拉链-》红黑树是为了防止恶意哈希

### 扩容，会导致某次的put方法时间延时十分的大（扩容也需要考虑，比如说第一次2倍，第二次三倍，目的是防止一直扩容）

### 在java中put和get函数的线程安全：参照currenthashmap

ConcurrentHashMap 是 Java 中用于在多线程环境下实现线程安全的哈希表。它通过以下几种机制来确保线程安全：

### 1. 分段锁（Segment Locking）
在 JDK 1.7 中，ConcurrentHashMap 使用了分段锁的机制。整个哈希表被分成多个段（Segment），每个段是一个独立的哈希表，并且有自己的锁。这样，多个线程可以并发地访问不同段的数据，而不会相互阻塞¹。

### 2. CAS 操作（Compare-And-Swap）
在 JDK 1.8 中，ConcurrentHashMap 摒弃了分段锁，改用 CAS 操作和 `synchronized` 关键字来实现线程安全。CAS 是一种无锁的线程安全机制，通过比较和交换操作来确保数据的一致性¹²。

### 3. Volatile 关键字
ConcurrentHashMap 中的一些关键变量使用了 `volatile` 关键字，确保变量的可见性。这样，当一个线程修改了这些变量的值，其他线程能够立即看到变化。

### 4. 细粒度锁
在 JDK 1.8 中，ConcurrentHashMap 使用了细粒度锁，即在需要修改某个桶（bucket）中的数据时，只对该桶加锁，而不是对整个哈希表加锁。这大大减少了锁的争用，提高了并发性能。

### 5. 红黑树
当桶中的元素过多时，ConcurrentHashMap 会将链表转换为红黑树，以提高查找和插入的效率。这种结构在高并发情况下表现更好¹。

### 示例代码
以下是一个简单的示例，展示了如何使用 ConcurrentHashMap：

```java
import java.util.concurrent.ConcurrentHashMap;

public class ConcurrentHashMapExample {
    public static void main(String[] args) {
        ConcurrentHashMap<String, String> map = new ConcurrentHashMap<>();

        // 添加元素
        map.put("key1", "value1");
        map.put("key2", "value2");

        // 获取元素
        String value = map.get("key1");
        System.out.println("Key1: " + value);

        // 删除元素
        map.remove("key2");

        // 遍历元素
        map.forEach((key, val) -> System.out.println(key + ": " + val));
    }
}
```

通过这些机制，ConcurrentHashMap 能够在高并发环境下提供高效的线程安全操作。
