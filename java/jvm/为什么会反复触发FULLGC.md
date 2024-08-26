JVM 频繁触发 Full GC（Full Garbage Collection）通常是由于以下几个原因：

1. **老年代空间不足**：
   - 当老年代（Tenured Generation）空间不足时，JVM 会触发 Full GC 以尝试回收更多内存。如果 Full GC 后老年代仍然不足，可能会导致 `OutOfMemoryError`²。

2. **永久代（PermGen）或元空间（Metaspace）空间不足**：
   - 在 Java 8 之前，类元数据存储在永久代中；Java 8 之后，存储在元空间中。如果这些区域空间不足，也会触发 Full GC²。

3. **大对象直接进入老年代**：
   - 大对象（如大数组）直接分配到老年代，如果老年代空间不足，会频繁触发 Full GC²。

4. **System.gc() 调用**：
   - 显式调用 `System.gc()` 会建议 JVM 进行 Full GC，尽管只是建议，但在很多情况下会触发 Full GC²。

5. **晋升失败**：
   - 当新生代对象晋升到老年代时，如果老年代空间不足，会触发 Full GC²。

### 如何排查频繁 Full GC 的原因

1. **查看 GC 日志**：
   - 启用 GC 日志（使用 `-XX:+PrintGCDetails` 和 `-Xloggc:<file>` 参数），分析日志以确定 Full GC 的频率和原因。

2. **监控内存使用情况**：
   - 使用工具如 VisualVM、JConsole 或其他 APM 工具监控堆内存和非堆内存的使用情况，找出内存泄漏或不合理的内存分配。

3. **调整 JVM 参数**：
   - 根据应用的具体情况调整堆大小（`-Xmx` 和 `-Xms`）、新生代大小（`-Xmn`）和永久代或元空间大小（`-XX:MaxPermSize` 或 `-XX:MaxMetaspaceSize`）。

4. **优化代码**：
   - 检查代码中是否有不必要的大对象分配、长生命周期对象或频繁调用 `System.gc()` 的情况，进行优化。


