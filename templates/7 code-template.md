# 💻 <TOPIC> Code Snippets

## Example 1
```java


---

# 🧠 7. SAMPLE (JDBC READY FILE)

📄 `spring/jdbc/jdbc-memory-map.md`

```md
# 🧠 JDBC Memory Map

## 🟣 Big Picture
JDBC → manual DB interaction

---

## 🔵 Core Flow
Connect → Prepare → Execute → Process → Close

---

## 🟡 Steps
1. Connection → DataSource.getConnection()
2. Prepare → PreparedStatement
3. Execute → executeQuery / executeUpdate
4. Process → ResultSet
5. Close → resources

---

## 🟢 Triggers
- executeQuery → SELECT
- executeUpdate → CUD
- ResultSet → data

---

## ⚡ Rules
- SELECT → ResultSet
- CUD → update

---

## 🔴 Problems
- Boilerplate
- Manual handling

---

## 🔥 10-sec Recall
JDBC → manual  
Flow → connect → execute → close  
Problem → boilerplate  