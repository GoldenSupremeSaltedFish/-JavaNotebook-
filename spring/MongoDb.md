## MongoDb的特性

以一个集合作为存储单元

（没有结构的一个数据库）

## 最基础的访问，存储方式（存储非结构化数据）

直接用主键进行访问

存储方法（Json数据格式）=》变成二进制来节省空间，真正存贮的格式的Binary JSON（这是一个二进制文件）

## 关系型/非关系型

对象的存储交给mongo

关系的查找交给Mysql

Mongo的各种查询

## Mongo的存储

b-Tree（这个跟mysql十分不一样）

树会变得很高（导致不适合做区间查询-因为b树的物理结构）-区间查询肯定会导致回表

## 使用springdata访问mongodb



## 在线业务AND离线业务（在线不进行业务报表的分析）-》

而是将所有的数据放置到离线的数据仓库内进行分析（不同数据库的同步机制）

## 使用docker创建MongodB

use--（可以使用一个没有creat的database）等你用的时候再进行创建

mysql如果没有创建数据库的话是不能进行创建的

mongoDb同样也有用户管理

