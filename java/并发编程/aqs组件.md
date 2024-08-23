
### AQS是AbstractQueuedSynchronizer的简称，即抽象队列同步器，从字面上可以这样理解:

抽象：抽象类，只实现一些主要逻辑，有些方法由子类实现；

队列：使用先进先出（FIFO）的队列存储数据；

同步：实现了同步的功能

只进行节点的存储

AQS 是一个用来构建锁和同步器的框架，使用 AQS 能简单且高效地构造出应用广泛的同步器，比如我们提到的 ReentrantLock，Semaphore，ReentrantReadWriteLock，SynchronousQueue，FutureTask 等等皆是基于 AQS 的

当然了，我们也可以利用 AQS 轻松定制专属的同步器，只要实现它的几个protected方法就可以了(以下的内容)

如果你的需求可以通过现有的 JUC 类实现，则不需要重写 AQS。

如果你需要创建一个全新的、特殊的同步器，才可能需要重写 AQS。

getState()

setState()

compareAndSetState()


