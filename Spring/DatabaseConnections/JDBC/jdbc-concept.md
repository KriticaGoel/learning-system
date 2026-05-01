### Implementation JDBC

1. Dependency
```xml
<dependency>
    <groupId>com.oracle.database.jdbc</groupId>
    <artifactId>ojdbc11</artifactId>
    <scope>runtime</scope>
</dependency>
```

2. Configure Database Connection
```java
   String url = "jdbc:mysql://localhost:3306/sampleapi";
   String user = "root";
   String password = "password";
``` 

3. Get Connection
   Two ways:
- DriverManager
```java
Connection con = DriverManager.getConnection(
    "jdbc:mysql://localhost:3306/testdb",
    "root",
    "password");
```   
❌ Problem

Creates a new connection every time

Expensive (slow in production)

- DataSource (Correct Approach)
```java
DataSource dataSource; // injected or configured

Connection con = dataSource.getConnection();
```

4. Create Query
```java
String sql = "SELECT id, name FROM users WHERE id = ?";
PreparedStatement ps = con.prepareStatement(sql);
ps.setInt(1, 1);
```

5. Execute Query
```java
ResultSet rs = ps.executeQuery(); 
```
6. Map results manually
```java
 while (rs.next()) {
   int id = rs.getInt("id");
   String name = rs.getString("name");
   System.out.println(id + " " + name);
}
```
7. Close resources
```java
finally {
    if (rs != null) rs.close();
    if (ps != null) ps.close();
    if (con != null) con.close();
}
```

😵‍💫 Problems with Raw JDBC
1. Boilerplate code
   try/catch/finally everywhere
2. Manual resource management
   Forget to close → memory leak
3. Manual mapping
   user.setName(rs.getString("name"));
4. No transaction management
   You must handle commit/rollback
   con.setAutoCommit(false);
   con.commit();
   con.rollback();
   ⚡ 4. Why It Still Matters

Even if you use Spring:

Spring JdbcTemplate → wraps JDBC
Hibernate → uses JDBC internally

👉 So understanding JDBC = understanding everything beneath