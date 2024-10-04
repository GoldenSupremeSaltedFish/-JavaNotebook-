## 为什么要做ioc（将依赖管理交给容器）

这个牵扯到低耦合（这是老生常谈了）

第二个就是做切片测试，将每一个类都分开来。（这样会使得切片控制十分的简单）

## 怎样实现IOC的

BEAN对象。POJO对象。

配置数据

## 类的周期

对象的管理

注解：

single（单例模式）

proto（调用一次生成一个）

request（针对每个http请求生成一个新的对象）--针对与服务器进行的生成对象

session（根据session来生成一个新的对象）--和cookie的区别

## BEAN的管理

（@Component）（扫包标签）POJO类（就是完完全全不进行任何继承，只是你自己写的一个类）-反例：servlet-需要进行继承http

（@Configuration）（直接生成一个配置类）里面进行（@Bean）

## Bean的创建（BEANfactory/ApplicationContext（一般使用））

工厂设计模式

## spring中的ApplicationContext源码十分的重要

## 尽量避免循环依赖问题（@setter使用懒加载解决）

三级缓存

## BEAN实例化的步骤（生命周期）

实例化-》构造函数-》属性赋值（依赖注入）-》（对象ready，容器关闭，进行初始化）-》销毁方法（干完活之后）

## 为什么不用system.out在服务器中

因为服务器没法进行systemout，写到log文件中，才可以进行阅读

## SpringBoot

简化配置，依赖管理

## 为什么要用Starter

starter是一个依赖的集合（比如测试环境，或者说线上环境，将这些环境的依赖集合在一个starter中，进行统一的管理）

遇到依赖打架的问题（我使用的是depency analysier）

启动starter会将所有的依赖进行带入

## 配置文件（yaml和proprietary）使pom文件更加简洁

这些配置会覆盖之前的配置

## 打成jar包的话需要一个main函数（直接告诉默认的配置在哪）