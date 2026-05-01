# 📘 JDBC - Complete Concept

## 🧠 What is JDBC?
JDBC is a low-level API used to interact with databases using Java.

---

## ⚙️ How it Works (Flow)
1. Load driver
2. Get connection
3. Create PreparedStatement
4. Execute query
5. Process ResultSet
6. Close resources

---

## 🔍 Step-by-Step Deep Dive

### 1. Connection
- Created using DataSource or DriverManager
- Represents DB session

### 2. PreparedStatement
- Precompiled SQL
- Prevents SQL injection

### 3. Execution
- executeQuery → SELECT
- executeUpdate → INSERT/UPDATE/DELETE

### 4. ResultSet
- Table-like structure
- Iterate using rs.next()

---

## ⚠️ Problems in JDBC
- Boilerplate code
- Manual resource handling
- No automatic transaction management

---

## 🔁 Relation to Spring
- JdbcTemplate → wraps JDBC
- Hibernate → internally uses JDBC

---

## 💡 Key Takeaways
- JDBC = foundation
- Everything else builds on it