### Implementation JdbcTemplate

It removes:

* manual connection handling
* try/catch/finally
* ResultSet mapping boilerplate

👉 You only write:

* SQL
* mapping logic

1. Dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

2. Database Configurations
```properties
spring.datasource.url=jdbc:oracle:thin:@//localhost:1521/ORCLPDB1
spring.datasource.username=system
spring.datasource.password=password
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
```
👉 Spring Boot auto-creates:

* DataSource
* JdbcTemplate

3. Model Class
```java 
public class User {
    private Long id;
    private String name;

    // getters & setters
} 
```
4. Repository Layer

--READ (single)  -> queryForObject
```java
public Users findById(String id) {

        String sql ="SELECT * FROM users WHERE id = ?";

        Object[] params = new Object[]{id};

        RowMapper<Users> rowMapper = (rs, rowNum) -> {
            Users u = new Users();
            u.setId(rs.getInt("id"));
            u.setName(rs.getString("name"));
            u.setEmail(rs.getString("email"));
            return u;
        };
    return jdbcTemplate.queryForObject(sql, params,rowMapper);

}
```
--READ (List)  -> query
```java
 public List<Users> findAll(){
        String sql ="Select * from Users";

        RowMapper<Users> rowMapper =(rs,rowNum)->{
            Users u= new Users();
            u.setId(rs.getInt("id"));
            u.setName(rs.getString("name"));
            u.setEmail(rs.getString("email"));
            return u;
        };

        return jdbcTemplate.query(sql,new Object[]{},rowMapper);
    }
```

--Create -> update()
```java
public int insertUser(String name,String email){
        String sql="insert into users(name,email) values(?,?)";
        return jdbcTemplate.update(sql,name,email);
    }
```

--Update -> update()
```java
public int updateUser(String name, String email){
        String sql ="update Users set email=? where ucase(name)=?";
        return jdbcTemplate.update(sql,email,name.toUpperCase());
    }
```

-Delete -> update()
```java
 public int deleteUser(String name){
        String sql="Delete fromUsers where ucase(name)=?";
        return jdbcTemplate.update(sql,name.toUpperCase());

    }
```
