### 这是对锁的补充（syn关键字）

对象结构分为对象头，实例数据以及对其填充

其中对象头分为：markword classpointer 数组

锁作用的地方是markword

其中markword的数据结构为下（是不是跟ipv6的报文格式一样，实际上都是很灵活的内存对齐）

![image](https://github.com/user-attachments/assets/b300a064-919e-4db9-9103-2193a11b1446)

