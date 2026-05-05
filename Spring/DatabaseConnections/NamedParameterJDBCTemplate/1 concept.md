## Agenda
1. [Create using Map](#create-using-map)
2. [Create using BeanPropertySqlParameterSource](#create-using-beanpropertysqlparametersource)
3. [Create using Custom Parameter Source](#create-using-custom-parametersource) 
4. [Read Single Row](#read-single-row)
5. [Read Multiple Row](#read-multiple-rows)
6. [Update Row](#update-row)
7. [Delete Row](#delete-row)
8. [Nested Create M:M](#-insert-mm)
9. [Nested Read  M:M](#-read-mm)
10. [Nested Update M:M](#-update-mm)
11. [Nested Delete M:M](#-delete-mm)
12. [Dynamic Query (Very Powerful)](#dynamic-query)

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

### Nested tables 

#### 🧠 Insert (M:M)

         DTO (Input Layer)
      ┌─────────────────────┐
      │ UserDTO             │
      │  └─ List<AddressDTO>│
      | AddressDTO          |  
      └─────────┬───────────┘
                ↓
        Convert → Model
                ↓
      ┌─────────────────────┐
      │ Users               │
      │  └─ List<Address>   │
      │ Address             │
      └─────────┬───────────┘
                ↓
        ParameterSource Layer
                ↓
    ┌─────────────────────────────┐
    │ CustomSQl parameterSource   |
    | (MapSqlParameterSource)     │
    └───────────┬─────────────────┘
                ↓
        Repository Layer
                ↓
    ┌─────────────────────────────┐
    │ NamedParameterJdbcTemplate  │
    └─────────┬─────────┬─────────┘
              │         │
       Insert User   Insert Address
              │         │
         KeyHolder   KeyHolder
              │         │
              └─────┬───┘
                    ↓
        Insert Mapping Table
        (user_id, address_id)

MapSqlParameterSource for nested objects with NamedParameterJdbcTemplate.


DTO
```java
public class AddressDTO {

    private String city;
    private String country;
}
public class UserDTO {

    private String name;
    private String email;
    private List<AddressDTO> address;
}
```

Models
```java
@Component
public class Users {

    private Integer id;
    private String name;
    private String email;
    private List<Address> address;
}

public class Address {

    private Integer addressId;
    private String city;
    private String country;
}
```
MapSqlParameterSource

```java
public class UserCustomSqlParameterSource extends MapSqlParameterSource {

    public UserCustomSqlParameterSource(Users user){
        addValue("name",user.getName());
        addValue("email",user.getEmail());
    }
}
public class AddressCustomSqlParameterSource extends MapSqlParameterSource {

    public AddressCustomSqlParameterSource(Address address){
        addValue("city",address.getCity());
        addValue("country",address.getCountry());
    }
}
public class UserAddressCustomSqlParameterSource extends MapSqlParameterSource {

    public UserAddressCustomSqlParameterSource(Integer userId, Integer addressId){

        addValue("userId",userId);
        addValue("addressId",addressId);
    }
}
```
Repository
```java
@Transactional
    public int createUserAddressUsingMapSqlParameterSource(Users user){

        //get user id
        KeyHolder userKey = new GeneratedKeyHolder();
        //insert user
        String userSql = "Insert into users (name,email) values (:name,:email)";
        namedParameterJdbcTemplate.update(userSql,new UserCustomSqlParameterSource(user),userKey, new String[]{"id"});

        //userKey contain user id to export the value in a variable
        Integer userId= userKey.getKey().intValue();

        //Insert Address
        for(Address address : user.getAddress()){

            KeyHolder addressKey = new GeneratedKeyHolder();
            String addressSql ="insert into address (city,country) values(:city,:country)";
            namedParameterJdbcTemplate.update(addressSql,new AddressCustomSqlParameterSource(address),addressKey,new String[]{"address_id"});

            //address key contain address id
            Integer addressId = addressKey.getKey().intValue();

            //insert into mapping table
            String userAddressMappingSql ="insert into useraddress (user_id,address_id) values(:userId,:addressId)";
            namedParameterJdbcTemplate.update(userAddressMappingSql,new UserAddrressCustomSqlParameterSource(userId,addressId));

        }
        return userId;
    }
```

#### 🧠 Read M:M
Use JOIN + ResultSetExtractor

✅ Single query (no N+1)

✅ Full control

✅ Best performance

Sql+ Params -> Map
```java
 public List<Users> getUsersWithAddress(Integer userId) {
        String sql="select u.id,u.name,u.email,a.address_id,a.city,a.country from users u " +
                "left join useraddress ua on ua.user_id=u.id " +
                "left join address a on a.address_id=ua.address_id where u.id=:userId";
        Map<String,Object> params= Map.of("userId",userId);

        return namedParameterJdbcTemplate.query(sql,params,new UsersResultSetExtractor());
    }
```
ResultSetExtractor - custom class
```java
public class UsersResultSetExtractor implements ResultSetExtractor<List<Users>> {

    @Override
    public List<Users> extractData(ResultSet rs) throws SQLException {

        Map<Integer,Users> map= new HashMap<>();

        if(rs!=null){
            while(rs.next()){
                int userId=rs.getInt("id");
                Users user=map.get(userId);
                if(user==null){
                    user= new Users();
                    user.setId(userId);
                    user.setName(rs.getString("name"));
                    user.setEmail(rs.getString("email"));
                    user.setAddress(new ArrayList<Address>());
                    map.put(userId,user);
                }
                int addressId= rs.getInt("address_id");//may be 0 that means data in db is null
                if(!rs.wasNull()){
                    Address address= new Address();
                    address.setAddressId(addressId);
                    address.setCity(rs.getString("city"));
                    address.setCountry(rs.getString("country"));
                    user.getAddress().add(address);

                }

            }
            return new ArrayList<Users>(map.values());
        }

        return null;
    }
```

#### 🔄 Update M:M

Delete + Reinsert mapping


    Update user attributes
         ↓
    Update existing address attributes
         ↓
    Delete old mappings
         ↓
    Insert new addresses (if needed)
         ↓
    Insert mappings

Delete old mapping-

Before update:
User 1 → [Address 10, Address 11]

After update (UI sends):
User 1 → [Address 11, Address 12]

Best way is to delete all mapping(10,11) and insert new mapping(11,12)

USER table      → attributes update
ADDRESS table   → attributes update
MAPPING table   → delete + reinsert


```java
@Transactional
public int updateUserComplete(Users users) {
    //  Update user attributes
    String userSql="update users set name=:name,email =:email where id=:userId";
    int rowAffected= namedParameterJdbcTemplate.update(userSql,new UserCustomSqlParameterSource(users).addValue("userId",users.getId()));

    if(rowAffected==0){
        System.out.println("user not found");
    }else {
        if (users.getAddress() != null && !users.getAddress().isEmpty()) {
            //Update existing address attributes
            for (Address address : users.getAddress()) {
                if (address.getAddressId() != null) {
                    String addressSql = "update address set city=:city , country=:country where address_id=:addressId";
                    namedParameterJdbcTemplate.update(addressSql, new AddressCustomSqlParameterSource(address).addValue("addressId", address.getAddressId()));
                } else {
                    //Insert new addresses (if needed)
                    KeyHolder addressKey = new GeneratedKeyHolder();
                    String addressSql = "Insert into address (city,country) values(:city,:country)";
                    namedParameterJdbcTemplate.update(addressSql, new AddressCustomSqlParameterSource(address), addressKey, new String[]{"address_id"});

                    int addressId = addressKey.getKey().intValue();
                    address.setAddressId(addressId);
                }
            }
            //Delete old mappings
            String mappingDelete = "delete from useraddress where user_id=:userId";
            namedParameterJdbcTemplate.update(mappingDelete, Map.of("userId", users.getId()));


            //Insert mappings
            for (Address address : users.getAddress()) {
                String mappingInsert = "insert into useraddress (user_id,address_id) values(:userId,:addressId)";
                namedParameterJdbcTemplate.update(mappingInsert, new UserAddressCustomSqlParameterSource(users.getId(), address.getAddressId()));
            }
        }
    }
    return 0;
}
```


User not found ≠ user should be created

👉 silently create new user → data corruption
#### 🗑️ Delete M:M

🟢 Case A: Delete ONE address from a user
```java
@Transactional
    public int deleteAddress(int addressId){
        String sql="Delete from useraddress where address_id=:addressId";
        namedParameterJdbcTemplate.update(sql,Map.of("addressId",addressId));

        String addressSql ="Delete from address where address_id=:addressId";
        return namedParameterJdbcTemplate.update(addressSql,Map.of("addressId",addressId));
    }
```
🟢 Case B: Delete ALL addresses of a user
```java
@Transactional
    public int deleteAllAddress(int userId){
        String mappingSql="select address_id from useraddress where user_id=:userId";
        List<Integer> addressIds = namedParameterJdbcTemplate.queryForList(mappingSql,Map.of("userId",userId),Integer.class);

        String sql="Delete from useraddress where user_id=:userId";
        namedParameterJdbcTemplate.update(sql,Map.of("userId",userId));

        for(Integer addressId: addressIds){
            String addressSql ="Delete from address where address_id=:addressId";
            namedParameterJdbcTemplate.update(addressSql,Map.of("addressId",addressId));
        }
return addressIds.size();
    }
```
🔴 Case C: Delete USER completely
```java
@Transactional
public int deleteUser(int userId){
String mappingSql="select address_id from useraddress where user_id=:userId";
List<Integer> addressIds = namedParameterJdbcTemplate.queryForList(mappingSql,Map.of("userId",userId),Integer.class);

        String sql="Delete from useraddress where user_id=:userId";
        namedParameterJdbcTemplate.update(sql,Map.of("userId",userId));

        for(Integer addressId: addressIds){
            String addressSql ="Delete from address where address_id=:addressId";
            namedParameterJdbcTemplate.update(addressSql,Map.of("addressId",addressId));
        }

        String userSql="Delete from users where user_id=:userId";
        return namedParameterJdbcTemplate.update(sql,Map.of("userId",userId));

    }
```

#### Dynamic Query

Search users with optional filters:
- name
- email
- city
- country

👉 Filters may be null / optional
```java
public List<Users> searchUsers(String name, String email, String city, String country) {

    StringBuilder sql = new StringBuilder("""
        SELECT u.user_id, u.name, u.email,
               a.address_id, a.city, a.country
        FROM users u
        LEFT JOIN useraddress ua ON u.user_id = ua.user_id
        LEFT JOIN address a ON ua.address_id = a.address_id
        WHERE 1=1
    """);

    MapSqlParameterSource params = new MapSqlParameterSource();

    if (name != null && !name.isBlank()) {
        sql.append(" AND u.name = :name");
        params.addValue("name", name);
    }

    if (email != null && !email.isBlank()) {
        sql.append(" AND u.email = :email");
        params.addValue("email", email);
    }

    if (city != null && !city.isBlank()) {
        sql.append(" AND a.city = :city");
        params.addValue("city", city);
    }

    if (country != null && !country.isBlank()) {
        sql.append(" AND a.country = :country");
        params.addValue("country", country);
    }

    return jdbc.query(sql.toString(), params, new UserAddressExtractor());
}
```
🧠 🔥 Advanced Patterns (Production Level)

✅ 1. LIKE search
```
sql.append(" AND u.name LIKE :name");
params.addValue("name", "%" + name + "%");
```
✅ 2. IN clause
```
sql.append(" AND u.user_id IN (:ids)");
params.addValue("ids", List.of(1,2,3));
```
✅ 3. Dynamic Sorting
```
if (sortBy != null) {
sql.append(" ORDER BY ").append(sortBy); // ⚠️ validate input
}
```

👉 Never bind column name as parameter ❗

✅ 4. Pagination
```
sql.append(" LIMIT :limit OFFSET :offset");
params.addValue("limit", limit);
params.addValue("offset", offset);
```








