## Agenda
1. [Create using Map](#create-using-map)
2. [Create using BeanPropertySqlParameterSource](#create-using-beanpropertysqlparametersource)
3. [Create using Custom Parameter Source](#create-using-custom-parametersource) 
4. [Read Single Row](#read-single-row)
5. [Read Multiple Row](#read-multiple-rows)
6. [Update Row](#update-row)
7. [Delete Row](#delete-row)
8. Nested Create
9. Nested Read 
10. Nested Update
11. Nested Delete
12. Dynamic Query (Very Powerful)

### Implementation NamedParameterJdbcTemplate

### Create

          SQL (String)
              +
    Params (Map<String,Object> / DTO/)
              +
    update(sql, map/BeanPropertySqlParameterSource(DTO)/MapSqlParameterSource)


* Powerful if we have model/DTO
* Spring uses: BeanPropertySqlParameterSource Its maps ->Java field → SQL named parameter
* Example : user.getName() → :name
* Important Rules
    * Field names must match SQL params
    * keep everything lowercase in SQL
    * In case of missing field :age -> Run time Error
    * Use Map for Dynamic Query or SImple static Query
    * **BeanPropertySqlParameterSource does NOT automatically flatten nested objects like address.city use Custom ParameterSource**

Flatten DTO means no nested DTO mention all field in single class
```java 
public class UserDTO {
    private Long id;
    private String name;
    private String city;
    private String country;
}
```

Repeated logic ->	Custom ParameterSource

Clean architecture ->	Flatten DTO ✅

Dependency, Database Configuration and Model class, Controller, Service remain same as JDBC Template

Repository

SQL (String) -> MAP/BEanPropertySqlParameterSource/Customer/  --> update()

#### Create using Map

Manual Mapping

```java
    public int createUserMap(UserRequest user) {
        String sql = "insert into users(name,email) values (:name,:email)";

        Map<String, Object> params = new HashMap<String, Object>();
        params.put("name", user.getName());
        params.put("email", user.getEmail());

        return namedParameterJdbcTemplate.update(sql, params);

    }
```
####  Create using BeanPropertySqlParameterSource
  
   BeanPropertySqlParameterSource -> Map Java field → SQL named parameter

   Create DTO
   ```java
   public class UserRequest {

    private String name;
    private String email;
    }
   ```
   
```java 
   public int createUser(UserRequest user) {
        String sql = "Insert into Users(name,email) values(:name,:email)";
        return namedParameterJdbcTemplate.update(sql, new BeanPropertySqlParameterSource(user));
    } 
   ```

#### Create using Custom ParameterSource

Create DTO
```java
public class UserRequest {

    private String name;
    private String email;
```
Create MapSQLParameterSource
```java
public class UserCustomSqlParameterSource extends MapSqlParameterSource {

    public UserCustomSqlParameterSource(UserRequest user){
        addValue("name",user.getName());
        addValue("email",user.getEmail());
    }
}
```
Repository
```java 
public int createUserUsingMapSqlParameterSource(UserRequest user) {

    //get user id
    KeyHolder userKey = new GeneratedKeyHolder();
    //insert user
    String userSql = "Insert into users (name,email) values (:name,:email)";
    namedParameterJdbcTemplate.update(userSql, new UserCustomSqlParameterSource(user), userKey, new String[]{"id"});

    //userKey contain user id to export the value in a variable
    Integer userId = userKey.getKey().intValue();
    return userId;
}
```

#### Read Single Row

```java 
public Users getUserByName(String name){

        String sql ="select * frm users where name =:name";
        //Object[] params = new Object[]{name};  -- wrong
        Map<String,Object> params=Map.of("name", name);
//        Map<String,Object> params= new HashMap();
//        params.put("name", name);
        RowMapper<Users> rowMapper = (rs,rowNum) ->{
            Users user = new Users();
            user.setId(rs.getInt("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        };

        return namedParameterJdbcTemplate.queryForObject(sql,params,rowMapper);
    }
```

#### Read Multiple Rows

```java
public List<Users> getUsers(){
        String sql ="select * from Users";
        RowMapper<Users> rowMapper = (rs,rowNum) ->{
          Users user = new Users();
          user.setId(rs.getInt("id"));
          user.setName(rs.getString("name"));
          user.setEmail(rs.getString("email"));
          return user;
        };

        return namedParameterJdbcTemplate.query(sql,rowMapper);
    }
```

#### Update row
```java
public int updateUserByName(UserRequest user){
        String sql="Update Users set email=:email where name=:name";
        return namedParameterJdbcTemplate.update(sql,new BeanPropertySqlParameterSource(user));
    }
```

#### Delete row
```java
   public int deleteUser(String name){
        String sql= "Delete from users where name =:name";
        Map<String,Object> params = Map.of("name",name);
        return namedParameterJdbcTemplate.update(sql,params);
    }
```

### Nested tables --Insert

    User (1)
       ↓
    Insert → user_id generated
    
    Addresses (many)
       ↓
    Insert each → address_id generated
    
    Mapping table
       ↓
    (user_id, address_id)

SqlParameterSource for nested objects with NamedParameterJdbcTemplate.

DTO Nested

