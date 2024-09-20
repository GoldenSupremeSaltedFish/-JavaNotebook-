### 比较重要的还要属二进制日志 binlog（归档日志）和事务日志 redo log（重做日志）和 undo log（回滚日志）

redo log（重做日志）是InnoDB存储引擎独有的，它让MySQL拥有了崩溃恢复能力。

比如 MySQL 实例挂了或宕机了，重启时，InnoDB存储引擎会使用redo log恢复数据，保证数据的持久性与完整性。

MySQL 中数据是以页为单位，你查询一条记录，会从硬盘把一页的数据加载出来，加载出来的数据叫数据页，会放入到 Buffer Pool 中。

后续的查询都是先从 Buffer Pool 中找，没有命中再去硬盘加载，减少硬盘 IO 开销，提升性能。

更新表数据的时候，也是如此，发现 Buffer Pool 里存在要更新的数据，就直接在 Buffer Pool 里更新。

然后会把“在某个数据页上做了什么修改”记录到重做日志缓存（redo log buffer）里，接着刷盘到 redo log 文件里。

### InnoDB 存储引擎为 redo log 的刷盘策略提供了 innodb_flush_log_at_trx_commit 参数，它支持三种策略：

0 ：设置为 0 的时候，表示每次事务提交时不进行刷盘操作

1 ：设置为 1 的时候，表示每次事务提交时都将进行刷盘操作（默认值）  

2 ：设置为 2 的时候，表示每次事务提交时都只把 redo log buffer 内容写入 page cache

另外，InnoDB 存储引擎有一个后台线程，每隔1 秒，就会把 redo log buffer 中的内容写到文件系统缓存（page cache），然后调用 fsync 刷盘

## binlog(同步专用-三种模式statement，row，mixed)

redo log 它是物理日志，记录内容是“在某个数据页上做了什么修改”，属于 InnoDB 存储引擎。

而 binlog 是逻辑日志，记录内容是语句的原始逻辑，类似于“给 ID=2 这一行的 c 字段加 1”，属于MySQL Server 层。

不管用什么存储引擎，只要发生了表数据更新，都会产生 binlog 日志。

### 两个日志可能导致的问题（redo log与binlog两份日志之间的逻辑不一致）--两阶段提交

写完redo log日志后，binlog日志写期间发生了异常（从库：喵喵喵？）

#### 所以就出现了两阶段提交

<img width="395" alt="image" src="https://github.com/user-attachments/assets/166b490f-2e1f-4e5c-8514-6f0f46bdb90e">


该方法将redo log 的写入拆解城了两个步骤prepare和commit

## 总结

MySQL InnoDB 引擎使用 redo log(重做日志) 保证事务的持久性，使用 undo log(回滚日志) 来保证事务的原子性。

MySQL数据库的数据备份、主备、主主、主从都离不开binlog，需要依靠binlog来同步数据，保证数据一致性。
