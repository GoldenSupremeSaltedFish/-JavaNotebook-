## 生产者偏移量

将生产消息写入kafka中，kafka为该消息分配的在分区中的顺序编号

## 作用：

**消息的唯一标识**: 每条消息在其所属的分区中都有一个唯一的偏移量，生产者的偏移量通常由 Kafka 自动生成和分配，用于确保消息在分区中的顺序性和唯一性。

**消费消息的起点**: 生产者的偏移量影响消息在分区中的位置，消费者依赖这些偏移量来有序地读取消息。消费者的消费过程总是根据生产者生成的偏移量逐条读取消息

## 消费者偏移量（意义-提交方式）

消费者偏移量是指消费者在 Kafka 中消费到某个主题分区时，标识其当前消费位置的标记。

## 作用：

**消息跟踪**: 通过消费者偏移量，Kafka 能知道消费者已经消费到哪一条消息，便于消费者在断开连接或重启时，继续从上次消费的地方继续消费，保证消息不重复消费或丢失。

**消费管理**: Kafka 允许消费者手动或自动提交偏移量，以确保在消费过程中记录消费进度。消费者可以选择手动控制偏移量提交时间，或让 Kafka 自动完成。

**实现消息重新处理**: 消费者可以根据需要重置偏移量，从而重新消费某些消息。例如，在处理逻辑失败后，可以将偏移量重置到之前的位置重新消费消息。

## 存储（持久化和容灾）

消费者偏移量通常会被存储在 Kafka 内部的特殊主题（默认是 `__consumer_offsets`），也可以存储在外部如 Zookeeper 或自定义数据库中

## 提交方式

### 自动提交

缺点：

自动提交间隔时间会导致重复消费,比如自动提交时间100S，首次提交的偏移量是20，而消费者拉取了5条消息消费了  ，在50秒的时候broker突然宕机，发生了分区再均衡，这个时候之前分区的消息从之前的消费者转移到另外一个新消费者，新的消费者会读取50秒之前提交的偏移量，导致重复消费。

丢失消息
消费者批量推送消息时，消费之只消费了20条，如果broker宕机，发生分区再均衡时，会从上次提交的偏移量位置重新消费，导致批量的消息(80条)丢失
注意：在调用close方法之前，是会自动提交一次偏移量，但异常或者提前退出轮询（poll）是不会的，需要自己定义策略来保证。比如finally方法手动提交commitSync()




### 手动提交

### 异步提交

### 异步+同步提交

### 提交特定的偏移量



