# 📚 JDBC Flashcards

---

## 🔹 Basics & Setup

<details>
<summary>Q: What dependency is required for JDBC (Oracle)?</summary>

A: ojdbc11

</details>

<details>
<summary>Q: What are the 3 basic DB connection configs?</summary>

A: URL, username, password

</details>

<details>
<summary>Q: Example JDBC URL format?</summary>

A: jdbc:mysql://localhost:3306/db_name

</details>


- Connection → DataSource
- PreparedStatement → safe SQL
- executeQuery → SELECT
- executeUpdate → CUD
- ResultSet → data
- Problem → boilerplate

---

## 🔹 Connection

<details>
<summary>Q: Two ways to get DB connection in JDBC?</summary>

A: DriverManager and DataSource

</details>

<details>
<summary>Q: Syntax to get connection using DriverManager?</summary>

A: DriverManager.getConnection(url, user, password)

</details>

<details>
<summary>Q: Problem with DriverManager?</summary>

A: Creates new connection every time → slow

</details>

<details>
<summary>Q: Preferred way to get connection in production? 🔥</summary>

A: DataSource

</details>

<details>
<summary>Q: Why is DataSource better?</summary>

A: Uses connection pooling → faster & efficient

</details>

---

## 🔹 Query Creation

<details>
<summary>Q: Which class is used to execute parameterized SQL?</summary>

A: PreparedStatement

</details>

<details>
<summary>Q: Why use PreparedStatement instead of Statement?</summary>

A: Prevents SQL injection + better performance

</details>

<details>
<summary>Q: How do you set parameters in PreparedStatement?</summary>

A: ps.setInt(), ps.setString(), etc.

</details>

---

## 🔹 Execution

<details>
<summary>Q: Method used to execute SELECT query?</summary>

A: executeQuery()

</details>

<details>
<summary>Q: What does executeQuery() return?</summary>

A: ResultSet

</details>

<details>
<summary>Q: Method used for INSERT/UPDATE/DELETE?</summary>

A: executeUpdate()

</details>

---

## 🔹 Result Processing

<details>
<summary>Q: What is ResultSet?</summary>

A: Object representing query result (rows)

</details>

<details>
<summary>Q: How to iterate over ResultSet?</summary>

A: while(rs.next())

</details>

<details>
<summary>Q: How to get column value from ResultSet?</summary>

A: rs.getInt("id"), rs.getString("name")

</details>

---

## 🔹 Resource Management

<details>
<summary>Q: Which resources must be closed in JDBC?</summary>

A: Connection, PreparedStatement, ResultSet

</details>

<details>
<summary>Q: Why is closing resources important?</summary>

A: Prevent memory leaks

</details>

<details>
<summary>Q: Where are resources usually closed?</summary>

A: finally block

</details>

---

## 🔹 Problems in JDBC

<details>
<summary>Q: Biggest issue with raw JDBC?</summary>

A: Boilerplate code

</details>

<details>
<summary>Q: What happens if you don’t close resources?</summary>

A: Memory leaks

</details>

<details>
<summary>Q: Why is manual mapping a problem?</summary>

A: Error-prone and repetitive

</details>

<details>
<summary>Q: Who manages transactions in JDBC?</summary>

A: Developer manually

</details>

<details>
<summary>Q: How to handle transactions manually?</summary>

A:
- setAutoCommit(false)
- commit()
- rollback()

</details>

---

## 🔹 Importance (Very Important)

<details>
<summary>Q: Why should you learn JDBC even if using Spring?</summary>

A: Because everything (JdbcTemplate, Hibernate) uses it internally

</details>

<details>
<summary>Q: JdbcTemplate is built on top of what? 🔥</summary>

A: JDBC

</details>

<details>
<summary>Q: Hibernate internally uses what?</summary>

A: JDBC

</details>