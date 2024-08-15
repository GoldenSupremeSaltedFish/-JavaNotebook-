### 1.spring框架
### 2，spring的循环依赖
有端联想，jvm在依赖标记而非根节点可达的情况下循环依赖无法进行回收。
（autowrie的自动装载依赖导致依赖的bean会被循环创建）->导致栈被挤爆了(jvm的栈->有段联想-jvm的类加载机制以及jvm的分区
解决方案：
Spring的三级缓存机制包括三个不同的缓存层次，每个层次都有不同的作用--三级缓存对应着函bean的三个状态
一级缓存（singletonObjects）
二级缓存（earlySingletonObjects）
三级缓存（singletonFactories）
三级缓存的具体工作流程
创建Bean A：
Spring开始创建Bean A，并将其标记为“正在创建”。
Bean A的工厂对象被放入三级缓存中。
创建Bean B：
Spring开始创建Bean B，并将其标记为“正在创建”。
Bean B的工厂对象被放入三级缓存中。
注入依赖：
当Bean B需要注入Bean A时，Spring会从三级缓存中获取Bean A的工厂对象，并创建一个早期引用（early reference）。
这个早期引用被放入二级缓存中，以便后续使用。
完成创建：
Bean B完成创建，并被放入一级缓存中。
Bean A完成创建，并被放入一级缓存中。
### 什么是循环依赖问题？
循环依赖有哪些类型？
在Spring中支持哪种循环依赖？
列举几种Spring不支持的循环依赖的场景，为什么不支持？
Spring解决循环依赖的流程是什么？
Spring解决缓存依赖时，二级缓存的作用是什么？
Spring只用二级缓存能否解决循环依赖？为什么一定要用三级缓存来解决循环依赖呢？
你从Spring解决循环依赖的设计中得到了哪些启发？
### 3，bean的生命周期
### 4，动态代理
