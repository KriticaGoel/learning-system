# 🧠 NamedParameterJdbcTemplate Memory Map

## 🟣 Big Picture
NamedParameterJdbcTemplate → JDBC with named params (:name)

---

## 🔵 Core Flow

SQL + Params(Map / Bean / Custom)
↓
NamedParameterJdbcTemplate
↓
update / query / queryForObject

---

## 🟡 Param Types

1. Map<String,Object> → manual
2. BeanPropertySqlParameterSource → auto mapping
3. MapSqlParameterSource (Custom) → reusable / complex

---

## 🟢 Triggers

:name → named param  
Map → manual mapping  
Bean → auto mapping  
Custom → reusable logic  
queryForObject → single  
query → list  
update → CUD

---

## ⚡ Rules

- Field name == SQL param name
- Missing param → runtime error
- Nested DTO ❌ → use Custom
- Dynamic query → use Map

---

## 🔴 Create Options

- Map → simple
- Bean → clean DTO
- Custom → reusable + KeyHolder

---

## 🟠 Read

- Single → queryForObject
- Multiple → query
- M:M → JOIN + ResultSetExtractor

---

## 🔵 Update

- update(sql, params)
- M:M → update + delete mapping + insert mapping

---

## ⚫ Delete

- Single → delete mapping + entity
- All → delete mapping → loop delete
- User → delete mapping + address + user

---

## 🟣 M:M Pattern

Insert:
User → Address → Mapping

Update:
Update → Delete mapping → Reinsert

Read:
JOIN + ResultSetExtractor

---

## 🟡 Dynamic Query

WHERE 1=1
+ optional filters

Patterns:
- LIKE → %value%
- IN → list
- SORT → validate
- Pagination → LIMIT OFFSET

---

## 🔥 10-sec Recall

Named → :param  
Map/Bean/Custom → params  
queryForObject → 1  
query → many  
update → CUD  
M:M → JOIN / mapping  
Dynamic → WHERE 1=1  