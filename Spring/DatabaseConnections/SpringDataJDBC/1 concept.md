## Agenda

1. Dependency
```xml
<!-- Spring Data JDBC -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

👉 In Spring Data JDBC:

Parent = Aggregate Root (Users)

Child = part of aggregate (Address)

No join table needed for user-specific data
