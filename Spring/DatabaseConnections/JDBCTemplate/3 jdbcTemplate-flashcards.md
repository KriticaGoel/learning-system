# 📚 JdbcTemplate Flashcards

## ⚡ 30-Second Revision

- queryForObject → single row
- query → multiple rows
- update → CUD
- RowMapper → mapping
- removes → boilerplate

---

## 🔹 Basics

<details>
<summary>Q: What is JdbcTemplate?</summary>

A: Spring wrapper over JDBC that removes boilerplate code

</details>

<details>
<summary>Q: What does JdbcTemplate remove?</summary>

A:
- Connection handling
- try/catch/finally
- ResultSet boilerplate

</details>

<details>
<summary>Q: What do you still write?</summary>

A: SQL + mapping logic

</details>

---

## 🔹 Setup

<details>
<summary>Q: Dependency required for JdbcTemplate?</summary>

A: spring-boot-starter-jdbc

</details>

<details>
<summary>Q: What does Spring Boot auto-create?</summary>

A:
- DataSource
- JdbcTemplate

</details>

---

## 🔹 Query Methods

<details>
<summary>Q: Method for single row fetch?</summary>

A: queryForObject()

</details>

<details>
<summary>Q: Method for multiple rows?</summary>

A: query()

</details>

<details>
<summary>Q: Method for INSERT/UPDATE/DELETE?</summary>

A: update()

</details>

---

## 🔹 Mapping

<details>
<summary>Q: What is RowMapper?</summary>

A: Converts ResultSet row into Java object

</details>

<details>
<summary>Q: When is RowMapper executed?</summary>

A: For each row in result

</details>

---

## 🔹 Parameters

<details>
<summary>Q: How are parameters passed in JdbcTemplate?</summary>

A: Object[]

</details>

---

## 🔹 Usage Logic

<details>
<summary>Q: When to use queryForObject vs query?</summary>

A:
- queryForObject → single row
- query → multiple rows

</details>

<details>
<summary>Q: Why is update() used for all CUD operations?</summary>

A: JdbcTemplate uses one method for INSERT, UPDATE, DELETE

</details>

---

## 🔹 Advantages

<details>
<summary>Q: Why use JdbcTemplate over JDBC?</summary>

A:
- Less boilerplate
- Better readability
- Faster development

</details>

---

## 🔹 Comparison Thinking

<details>
<summary>Q: JdbcTemplate vs JDBC?</summary>

A:
- JDBC → manual
- JdbcTemplate → automated boilerplate

</details>