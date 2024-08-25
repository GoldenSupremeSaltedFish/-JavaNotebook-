### WeakHashMap(通过弱引用来存取键的map)

键的弱引用：在 WeakHashMap 中，键是使用弱引用来存储的。这意味着，如果没有其他地方持有对这个键的强引用，那么这个键可以被 GC 回收。回收后，WeakHashMap 会在下一次操作时自动移除这个键值对。

值的强引用：与键不同，WeakHashMap 中的值是通过强引用存储的。只要对应的键仍然存在（即没有被回收），值就会一直存在并可供访问。

#### 举个栗子吧

```java

Map<Object, String> weakMap = new WeakHashMap<>();
        Object key1 = new Object();
        Object key2 = new Object();

        weakMap.put(key1, "value1");
        weakMap.put(key2, "value2");

        key1 = null; // key1 不再有强引用

        System.gc(); // 触发 GC，key1 及其对应的键值对应当会被移除

        // 此时 weakMap 可能只剩下 key2 和 "value2" 这个键值对
        System.out.println(weakMap);

```
