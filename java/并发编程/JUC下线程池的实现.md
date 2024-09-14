### 线程池实现（Executors）位于juc包下

### 如果你想动态的修改线程池的核心线程数


方法：threadpoolexcutor。setcoresize（int size)

引出的话题；核心线程数和最大线程数（最大就是塞不下辣，核心线程数：就是跟银行的柜员一样，会确定柜员的数量（他们一直存在，即使没有顾客需要他们服务），如果顾客到来时线程数量不到核心线程数，会直接创建线程）

一般来说，这个东西是不过期（gc的）如果你想要他进行回收，可以将allow Corethreadtimeout（true）


## 其提供了五个静态工厂方法（设计模式）（工厂设计模式）

 ### newFixedThreadPool(int nThreads)
创建一个固定大小的线程池。线程池中始终保持指定数量的线程。如果线程在执行任务时空闲，那么它们会等待新的任务。
```java

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class FixedThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 5; i++) {
            final int task = i;
            executor.submit(() -> {
                System.out.println("Executing Task " + task + " by " + Thread.currentThread().getName());
            });
        }
        executor.shutdown();
    }
}

```

### newCachedThreadPool()
创建一个可以根据需要创建新线程的线程池。如果线程空闲并且超过60秒未使用，则会被终止并从池中移除。当需要大量短时间任务时比较合适。

```
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CachedThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newCachedThreadPool();
        for (int i = 0; i < 5; i++) {
            final int task = i;
            executor.submit(() -> {
                System.out.println("Executing Task " + task + " by " + Thread.currentThread().getName());
            });
        }
        executor.shutdown();
    }
}

```
### singleThreadExcutor

单线程（将任务顺序提交，你对任务的排列要求很高的情况下才使用）

### newScheduledThreadPool(int corePoolSize)
创建一个可以调度任务在给定延迟后运行或定期执行任务的线程池。线程池中保持一定数量的线程来处理定时任务。

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class ScheduledThreadPoolExample {
    public static void main(String[] args) {
        ScheduledExecutorService executor = Executors.newScheduledThreadPool(2);

        executor.schedule(() -> {
            System.out.println("Task executed after 3 seconds delay");
        }, 3, TimeUnit.SECONDS);

        executor.scheduleAtFixedRate(() -> {
            System.out.println("Repeating task executed every 2 seconds");
        }, 1, 2, TimeUnit.SECONDS);

        // Shutdown the executor after some delay for demonstration purposes
        executor.schedule(() -> executor.shutdown(), 10, TimeUnit.SECONDS);
    }
}

```

### newWorkStealingPool()：
创建一个支持工作窃取的并行线程池，适合需要高并发任务的场景。

什么是窃取？

任务窃取：

每个线程在处理自己的任务队列时，会先尝试从自己的任务队列中获取任务进行执行。

如果一个线程完成了自己任务队列中的所有任务（即队列为空），它会尝试从其他线程的任务队列中“窃取”任务来继续工作。

这种窃取操作是为了避免某些线程因为任务短缺而处于空闲状态，而其他线程却还在处理大量任务。

窃取策略：

窃取通常是以“随机窃取”的方式进行，即一个空闲线程会随机选择一个忙碌线程的任务队列进行任务窃取。

这可以有效地平衡负载，提高并发性能，因为它可以将任务从繁忙的线程转移到空闲线程上，从而减少线程空闲时间

```java

import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class WorkStealingPoolExample {
    public static void main(String[] args) {
        // 创建一个支持工作窃取的线程池
        ExecutorService executor = Executors.newWorkStealingPool();

        // 提交一些任务到线程池
        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " is being executed by " + Thread.currentThread().getName());
                try {
                    Thread.sleep(1000); // 模拟任务执行
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            });
        }

        // 关闭线程池
        executor.shutdown();
    }
}
```
输出的结果为
```
Task 0 is being executed by pool-1-thread-1
Task 1 is being executed by pool-1-thread-2
Task 2 is being executed by pool-1-thread-3
Task 3 is being executed by pool-1-thread-4
Task 4 is being executed by pool-1-thread-5
Task 5 is being executed by pool-1-thread-6
Task 6 is being executed by pool-1-thread-7
Task 7 is being executed by pool-1-thread-8
Task 8 is being executed by pool-1-thread-9
Task 9 is being executed by pool-1-thread-10
(但是在这期间存在窃取，比如线程2现在很闲，他就会从线程1的队列中进行获取，用来负载均衡)

```
