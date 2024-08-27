### 在联合索引中，最左匹配原则一般来说是必须先进行最左索引的命中，然后才能进行完整的索引。

比如说存在键a，b。并且组合为联合索引

在8.0之前，这个查询必须变成，
```
SELECT column1 FROM my_table WHERE column1 IS NOT NULL AND column2 > 4500;
```
但是在8.0之后,以下的语句会达到相同的目的，而不是走全表扫描
```
EXPLAIN select column1  from my_table where column2 >4500;
```
