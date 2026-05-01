# 🧠 JDBC Memory Map

## 🟣 Big Picture
```JDBC → Low-level DB access (manual control)```

---

## 🔵 Core Flow / Structure
```Connect → Prepare → Execute → Process → Close```
---

## 🟡 Steps / Components
            [Connection]
                 ↓
            [PreparedStatement]
                 ↓
            [Execute]
                 ↓
            [ResultSet]
                 ↓
            [Close]
1.  Get Connection -> DataSource.getConnection()
2. Write query in String
3. PreparedStatement -> Connection.preparedStatement(sql)
4. Set Params in PreparedStatement
5. ResultSet -> 

    PreparedStatement.executeQuery() ->Select

    PreparedStatement.updateQuery() -> CUD
6. Process Result -> resultset.next();
7. Close connection

---

## 🟢 Triggers
Connection → DB link

PreparedStatement → safe SQL

executeQuery → SELECT

executeUpdate → CUD

ResultSet → table data

rs.next → iterate rows

---

## ⚡ Rules
Rule 1: SELECT → ResultSet

Rule 2: CUD → executeUpdate

Rule 3: Everything is manual

Rule 4: Always close resources

---

## 🔴 Problems / Tradeoffs
❌ Boilerplate (try/catch everywhere)

❌ Manual resource handling

❌ New connection each time (slow)

❌ Manual mapping (error-prone)

❌ No auto transaction handling

---

## 🟠 When to Use
✔ Low-level control needed

✔ Debugging DB issues

✔ Understanding internals

---

## ⚪ When NOT to Use
❌ Large apps (too much boilerplate)

❌ When Spring is available
---

## 🔥 10-sec Recall
JDBC → manual DB

Flow → Connect → Prepare → Execute → Process → Close

SELECT → ResultSet

CUD → executeUpdate

Problem → boilerplate + manual handling