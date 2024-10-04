## SpringData（基本上都有对应的数据源的服务类）

使用这个存什么：关系型数据库（mysql）

非关系型数据库（mongo）

### 切换数据源的有感和无感

### SpringDataJPA

建议加的注解@AllArgsConstructor（自动生成构造函数）

JPA（entity）

## springData（JPA）（自动生成Sql）

Spring Data JPA 可以根据方法名和参数自动生成 SQL 查询。这种功能被称为“查询方法”或“方法名查询”。通过定义符合特定命名规则的方法，Spring Data JPA 可以解析方法名并生成相应的 SQL 查询。

例如，假设你有一个 `User` 实体类和一个对应的 `UserRepository` 接口：

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    // getters and setters
}

public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);//这里就是方法名作为
    List<User> findByEmail(String email);
}
```

在这个例子中，Spring Data JPA 会根据 `findByName` 和 `findByEmail` 方法名自动生成相应的 SQL 查询：

- `findByName(String name)` 会生成类似 `SELECT u FROM User u WHERE u.name = ?1` 的查询。

- `findByEmail(String email)` 会生成类似 `SELECT u FROM User u WHERE u.email = ?1` 的查询。

  ## 

### 这么做的缺点（会导致复杂情况下的sql优化出现问题，你无法针对性的进行sql优化）              

### jpa也提供了对于复杂查询的定义机制（但是十分不方便，不支持xml进行书写）

## springData的意义

代码量进行降低（少写少犯错）

