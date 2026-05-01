# 🧠 JdbcTemplate Memory Map

## 🟣 Big Picture
JdbcTemplate → Spring wrapper over JDBC (removes boilerplate)

---

## 🔵 Core Idea
You write:
- SQL
- Mapping logic

Spring handles:
- Connection
- Exceptions
- Resource closing

---

## 🔵 Core Flow
SQL → JdbcTemplate → RowMapper → Object

---

## 🟡 CRUD Methods

- CREATE → update()
- READ (single) → queryForObject()
- READ (list) → query()
- UPDATE → update()
- DELETE → update()

---

## 🟢 Triggers

- queryForObject → single row
- query → multiple rows
- update → CUD
- RowMapper → row → object
- params → Object[]

---

## ⚡ Rules

- SELECT (1 row) → queryForObject
- SELECT (many) → query
- CUD → update
- Always use RowMapper for mapping

---

## 🔴 What It Removes

- ❌ manual connection handling
- ❌ try/catch/finally
- ❌ ResultSet boilerplate

---

## 🟠 Setup Flow

1. Add dependency
2. Configure datasource
3. Spring auto-creates:
    - DataSource
    - JdbcTemplate

---

## ⚪ When to Use

✔ Need SQL control  
✔ Faster than JPA  
✔ Simple + readable

---

## ⚫ When NOT to Use

❌ Complex ORM relationships  
❌ Large domain models

---

## 🔥 10-sec Recall

JdbcTemplate → removes boilerplate  
queryForObject → single  
query → list  
update → CUD  
RowMapper → mapping  