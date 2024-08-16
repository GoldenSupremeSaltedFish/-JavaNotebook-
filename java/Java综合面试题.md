### 1.Java面向什么？

面向对象，面向对象和面向过程的区别

### 2.jvm内存区域有哪些
线程私有区域【程序计数器、 虚拟机 栈、本地方法区】、线程共享区域【JAVA 堆、方法区】、直接内存

### 3.jvm运行时内存

### 4.垃圾回收的算法有哪些

标记-回收，复制算法，以及标记-整理算法

优缺点

复制较慢但是
分代回收机制，老年代和年轻代（这是机制）

full gc
### 5.Java中四种引用类型
在Java中，有四种引用类型，它们分别是强引用、软引用、弱引用和虚引用。每种引用类型在垃圾回收机制中有不同的行为和使用场景：

 **强引用（Strong Reference）**：
   - 默认的引用类型，例如 `Object obj = new Object();`。
   - 只要强引用存在，垃圾回收器就不会回收被引用的对象，即使内存不足也不会回收¹。

 **软引用（Soft Reference）**：
   - 用于描述一些非必需但仍有用的对象，例如缓存。
   - 当内存不足时，垃圾回收器会回收这些对象，以避免内存溢出错误²。
   - 使用方式：`SoftReference<Object> softRef = new SoftReference<>(obj);`

 **弱引用（Weak Reference）**：
   - 用于描述非必需对象，生命周期比软引用更短。
   - 当垃圾回收器扫描到弱引用对象时，无论内存是否充足，都会回收这些对象²。
   - 使用方式：`WeakReference<Object> weakRef = new WeakReference<>(obj);`

 **虚引用（Phantom Reference）**：
   - 最弱的引用类型，无法通过虚引用访问对象。
   - 主要用于跟踪对象被垃圾回收的时间，必须与引用队列（ReferenceQueue）联合使用²。
   - 使用方式：`PhantomReference<Object> phantomRef = new PhantomReference<>(obj, refQueue);`



### 6.gc垃圾回收器有哪些
不知道

### 7. io与nio的不同之处
 面向流 vs 面向缓冲区
IO：面向流，每次从流中读取或写入一个字节或字符，数据没有被缓存在任何地方。
NIO：面向缓冲区，数据读取到一个缓冲区中，稍后进行处理。可以在缓冲区中前后移动数据，提高了处理的灵活性12.
2. 阻塞 IO vs 非阻塞 IO
IO：阻塞 IO，线程在执行读写操作时会被阻塞，直到操作完成。
NIO：非阻塞 IO，线程可以在等待数据时执行其他任务，不会被阻塞12.
3. 单向 vs 双向
IO：单向，输入和输出流是独立的。
NIO：双向，通道（Channel）可以同时进行读写操作12.
4. 选择器（Selectors）
IO：没有选择器机制。
NIO：引入了选择器，可以使用一个线程管理多个通道，提高了并发处理能力12.
5. 应用场景
IO：适用于处理少量连接但数据量大的场景，如文件读写。
NIO：适用于处理大量连接但数据量小的场景，如聊天服务器、网络编程

### 8.Java的类加载机制
加载，链接，初始化，使用和卸载

### 9. 详细说明双亲委派机制
类的加载往往会被委派给父类，作用是防止对基类的恶意篡改
### 10.tomcat是如何打破双亲委派机制的

Tomcat是一个Web框架，并且可以支持部署多个web项目，web项目在Tomcat被抽象的称为Context，即每一个web项目是一个Context。而每个Context是独立的，比如项目A可以引用spring1.0，而项目B可以引用spring2.0版本，如果是双亲委派机制，那么只能存在一个spring版本。Tomcat是如何打破双亲委派机制的呢？



### 11.List中Vector ArrayList LinkedList底层实现分别是什么

arraylist（基于动态数组实现）

查询速度快：由于底层是数组，可以通过索引快速访问元素。
增删速度慢：在数组中间插入或删除元素时，需要移动大量元素。
线程不安全：ArrayList 不是线程安全的，需要手动同步。

vector 也是动态数组

线程安全：大部分方法都使用了 synchronized 关键字，保证线程安全。
性能较低：由于线程安全的开销，性能比 ArrayList 略低。
查询速度快：和 ArrayList 一样，查询速度快

linklist 双向链表实现

增删速度快：在链表中间插入或删除元素时，只需要调整引用，不需要移动大量元素。
查询速度慢：由于需要遍历链表，查询速度比 ArrayList 慢。
双向链表：可以用作栈、队列和双向队列。

### 12.HashSet TreeSet LinkedHashSet的底层是是什么

### 13.CurrentHashMap 是如何实现线程安全的
在每次插入时都会引入cas机制来保持数据的一致性
### 14. 有哪些创建线程的方式
new一个线程，
### 15.四种线程池分别是什么

Java 中常用的四种线程池分别是：

##### newCachedThreadPool（可缓存线程池）：
特点：如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。适用于执行很多短期的异步任务。
实现：ThreadPoolExecutor 的核心线程数为 0，最大线程数为 Integer.MAX_VALUE，空闲线程存活时间为 60 秒，使用 SynchronousQueue 作为工作队列12.
##### newFixedThreadPool（固定大小线程池）：
特点：创建一个固定大小的线程池，可控制线程最大并发数，超出的线程会在队列中等待。适用于负载较重的服务器。
实现：ThreadPoolExecutor 的核心线程数和最大线程数相等，空闲线程不会被回收，使用 LinkedBlockingQueue 作为工作队列12.
##### newSingleThreadExecutor（单线程线程池）：
特点：创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序（FIFO, LIFO, 优先级）执行。适用于需要保证顺序执行任务的场景。
实现：ThreadPoolExecutor 的核心线程数和最大线程数都为 1，使用 LinkedBlockingQueue 作为工作队列12.
##### newScheduledThreadPool（定时线程池）：
特点：创建一个定长线程池，支持定时及周期性任务执行。适用于需要周期性执行任务的场景。
实现：ScheduledThreadPoolExecutor，核心线程数固定，最大线程数为 Integer.MAX_VALUE，使用 DelayedWorkQueue 作为工作队列12.

### 16.线程的生命周期是什么

新建-就绪-运行-阻塞-等待-超时等待-终止

### 17.如何终止线程

使用标志位来进行检测
```java
public class MyThread extends Thread {
    private volatile boolean exit = false;

    public void run() {
        while (!exit) {
            // 执行任务
        }
    }

    public void stopThread() {
        exit = true;
    }
}
```
interrupt() 方法：
通过调用线程的 interrupt() 方法来中断线程。线程需要在运行过程中定期检查中断状态
```java
public class MyThread extends Thread {
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            // 执行任务
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt(); // 重新设置中断状态
                break;
            }
        }
    }
}


```
stop方法（不推荐，强行终止线程）

### 18.sleep和wait有何区别
所属类和调用方式：

sleep 方法属于 Thread 类，可以在任何地方调用。

wait 方法属于 Object 类，必须在同步块或同步方法中调用。

对锁的处理机制：

sleep 方法不会释放锁，线程只是暂停执行一段时间，但仍然持有锁
。
wait 方法会释放锁，让出 CPU 资源，并且线程进入等待状态，直到被其他线程唤醒。

唤醒机制：

sleep 方法在指定的时间后会自动苏醒，不需要其他线程唤醒。

wait 方法需要被 notify 或 notifyAll 方法唤醒。

### 19.start和run 有何区别（start会生成一个新的线程，而run不会，也不会导致上下文的切换）
run 方法：

run 方法是 Thread 类的一个普通方法，用于定义线程的主体逻辑。

当直接调用 run 方法时，它会在当前线程的上下文中执行，而不会创建新的线程。

直接调用 run 方法不会实现多线程的并发执行，只是在当前线程中按顺序执行 run 方法的内容。

### 20.Java中有哪些锁机制
自旋锁，轻量级锁，重量级锁
##### synchronized 锁：
隐式锁：通过 synchronized 关键字实现，分为方法锁和代码块锁。它是 JVM 层面实现的锁，使用方便，但性能相对较低。

对象锁：锁住的是具体的对象实例。

类锁：锁住的是类对象，通常用于静态方法。

显式锁（Lock）：

ReentrantLock：可重入锁，支持公平锁和非公平锁，提供了更灵活的锁机制。

ReentrantReadWriteLock：读写锁，允许多个读线程同时访问，但写线程独占。

StampedLock：改进的读写锁，提供了乐观读锁，性能更高。

### 21.synchronize的作用和核心实现分别是什么

### 22.线程中有什么方法

start，run，sleep，wait，isAlive()：检查线程是否处于活动状态。

interrupt()：中断线程。

setName(String name) 和 getName()

etPriority(int priority) 和 getPriority()：设置和获取线程的优先级。

yield()：让出 CPU 使用权，允许其他线程执行。

currentThread()：获取当前正在执行的线程。
### 23.线程上下文是如何切换的

### 24.什么是CAS，什么是AQS

cas是通过判断事务提交之前之后的预期值和新值来辨别信息是否经过修改

缺点ABA 问题：如果一个值从 A 变为 B 再变回 A，CAS 操作无法检测到这种变化。
自旋等待：在高竞争情况下，CAS 操作可能会导致大量的自旋等待，浪费 CPU 资源。

AQS：用于构建锁和同步器，通过 FIFO 队列管理线程的访问，提供了独占锁和共享锁两种模式
同步状态（state）：一个 volatile int 变量，表示共享资源的状态。
FIFO 队列：一个双向链表，用于存储等待获取锁的线程。
独占模式：每次只有一个线程能获取锁，例如 ReentrantLock。
共享模式：多个线程可以同时获取锁，例如 Semaphore 和 CountDownLatch。

### 25.JAVA异常的分类，分别举出有哪些常见的异常

#### 系统錯误（system error) 是由 Java 虚拟机抛出的，用 Error 类表示。Error 类描述的是内部系统错误。这样的错误很少发生。如果发生，除了通知用户以及尽量稳妥地终止程序外，几乎什么也不能做

#### Exception 则是编译时异常，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

NumberFormatException ：数值类型的格式错误；

FileNotFoundException ：未找到文件；

SocketException ：读取网络失败。

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

NullPointerException ：对某个 null 的对象调用方法或字段；

IndexOutOfBoundsException ：数组索引越界。

 RuntimeException是那些可能在 Java 虚拟机正常运行期间抛出的异常的超类，可能在执行方法期间抛出但未被捕获的 RuntimeException 的任何子类都无需在 throws 子句中进行声明，指的就是这些问题不需要提前被预防（本质上也可以的，只不过没必要），因为只有在真正运行的时候才能发现是否发生问题，一旦在运行期间发生了问题，则一般不会修正，程序直接终端

可捕获异常和不可捕获异常（exception）

可捕获异常（Checked Exception）


定义：必须在编译时处理的异常。

处理方式：必须使用try-catch块捕获或在方法签名中使用throws声明。

常见例子：IOException、SQLException。

用途：通常用于表示程序外部的错误，如文件未找到、数据库连接失败等。

不可捕获异常（Unchecked Exception）

定义：在编译时不强制处理的异常。

处理方式：可以选择捕获，也可以不处理。

常见例子：NullPointerException、ArrayIndexOutOfBoundsException。

用途：通常用于表示程序内部的逻辑错误，如空指针访问、数组越界等。

error（出现了则编译都过不去）

### 26. Java中反射有什么作用，好处是什么

可以动态的获取类内部的内容，好处是可以减少代码量以及

### 27.Java中注解是如何使用的，自己写过注解吗
写过，自动装配注解

自定义注解的声明@interface

注解的使用
### 28.内部类是什么
类中有类

成员内部类：定义在另一个类的内部，作为该类的成员。成员内部类可以访问外部类的所有成员变量和方法，包括私有成员。

局部内部类：定义在方法或代码块内部，仅在该方法或代码块中可见。

静态内部类：使用 static 修饰的内部类。静态内部类不依赖于外部类的实例，可以直接创建对象。

匿名内部类：没有名字的内部类，通常用于简化代码，特别是在实现接口或继承类时。

### 29.泛型是什么

类型参数化：泛型允许在类、接口和方法中使用类型参数。例如，ArrayList<T> 中的 T 就是一个类型参数，可以在实例化时指定具体的类型，如 ArrayList<String>。
类型安全：使用泛型可以在编译时进行类型检查，避免了运行时的类型转换异常。例如，ArrayList<String> 只能存储字符串，编译器会在添加非字符串元素时报错。

### 30.Java序列化的目的
序列化是什么，类toio流，目的是进行网络传输，保存在磁盘

序列化的方法
### 31.cookie和session是什么意思，分别有什么作用
cookie和session都是一种客户端和服务器传递信息的机制，是一小段文本信息，cookie是在浏览器中保存用户登录信息，而session存储在服务器端（安全性和内存大小不同）

生命周期：
Cookie：可以设置过期时间，持久化存储。
Session：通常在用户会话结束或浏览器关闭时失效


### 32.转发和重定向有什么区别
不知道

### 34.spring有什么特点
工厂化，将类的创建和创建方法进行分离
依赖的统一管理
### 35.spring的核心组件有哪些，你用过哪些常用模块，包，和注解



### 36.spring ioc的原理





### 37.spring bean的作用域，生命周期以及依赖注入的四种方式



### 38.spring aop是神马意思
面向切面编程
提供面向切面编程功能，允许将横切关注点（如事务管理、日志记录）与业务逻辑分离。

39.springmvc的工作流程


40.springboot的原理（？？？我有点蒙蔽或许这题想问什么是springboot？）


### 41.java中如何启动多线程
1，继承thread类
```java

public class Thread1 extends Thread{
 
	@Override
	public void run() {
		
		for (int i = 0; i < 50; i++) {
			System.out.println(Thread.currentThread().getName() + "执行" + i);
		}
	}
```
2。实现runnable接口
```java

public class Thread2 implements Runnable{
 
	public void run() {
		
		for (int i = 0; i < 50; i++) {
			System.out.println(Thread.currentThread().getName() + "执行" + i);
		}
	}
 
}

```

2,匿名内部类
```java
public class Main {
	
	public static void main(String[] args) {
		new Thread(new Runnable() {
			
			public void run() {
				for (int i = 0; i < 50; i++) {
					System.out.println(Thread.currentThread().getName() + "执行" + i);
				}
			}
		}).start();
		for (int i = 0; i < 50; i++) {
			System.out.println(Thread.currentThread().getName() + "执行" + i);
		}
	}
}
```
### 39.判断Java多线程出现故障可以从以下几个方面入手：

1. **线程状态分析**：
   - 使用工具如 **JConsole** 或 **VisualVM** 实时监控线程状态，查看是否有线程处于阻塞或等待状态。
   - 生成 **Thread Dump** 并分析，查看线程是否存在死锁、活锁或资源饥饿等问。

2. **堆栈跟踪**：
   - 当线程出现问题时，打印堆栈跟踪信息，了解线程在做什么以及它们被阻塞在哪里。
   - 使用 **jstack** 命令生成线程堆栈信息，并分析其中的线程状态。

3. **日志分析**：
   - 记录详细的日志，追踪线程的行为和事件顺序，特别是在分布式系统中。

4. **代码审查**：
   - 仔细检查同步代码，确保所有必要的同步措施都已到位，没有遗漏。
   - 使用静态代码分析工具如 **FindBugs** 或 **PMD** 来发现潜在的并发问题。

5. **资源监控**：
   - 使用 **top**、**jps** 以及 **ps** 等命令定位出哪个进程占用资源过高¹。
   - 分析CPU、内存、磁盘和网络的使用情况，找出可能的瓶颈⁴。

