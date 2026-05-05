# 📚 NamedParameterJdbcTemplate Flashcards

## ⚡ 30-Second Revision

- :name → named param
- Map → manual
- Bean → auto mapping
- Custom → reusable
- queryForObject → single
- query → list
- update → CUD

---

## 🔹 Basics

<details>
<summary>Q: What is NamedParameterJdbcTemplate?</summary>

A: JdbcTemplate with named parameters (:name)

</details>

<details>
<summary>Q: Main advantage over JdbcTemplate?</summary>

A: Readable SQL using named params

</details>

---

## 🔹 Parameter Types

<details>
<summary>Q: What are parameter types?</summary>

A:
- Map
- BeanPropertySqlParameterSource
- MapSqlParameterSource

</details>

<details>
<summary>Q: When to use Map?</summary>

A: Simple or dynamic queries

</details>

<details>
<summary>Q: When to use BeanPropertySqlParameterSource?</summary>

A: DTO → auto mapping

</details>

<details>
<summary>Q: When to use Custom ParameterSource?</summary>

A: Reusable logic / complex mapping

</details>

<details>
<summary>Q: Important rule for BeanPropertySqlParameterSource?</summary>

A: Field names must match SQL params

</details>

<details>
<summary>Q: Limitation of BeanPropertySqlParameterSource?</summary>

A: Cannot flatten nested objects

</details>

---

## 🔹 CRUD

<details>
<summary>Q: Method for CREATE/UPDATE/DELETE?</summary>

A: update()

</details>

<details>
<summary>Q: Method for single row?</summary>

A: queryForObject()

</details>

<details>
<summary>Q: Method for multiple rows?</summary>

A: query()

</details>

---

## 🔹 Create

<details>
<summary>Q: How many ways to create?</summary>

A:
- Map
- Bean
- Custom

</details>

<details>
<summary>Q: Why use Custom ParameterSource?</summary>

A: Avoid repeated mapping logic

</details>

---

## 🔹 Read

<details>
<summary>Q: How to read single row?</summary>

A: queryForObject()

</details>

<details>
<summary>Q: How to read multiple rows?</summary>

A: query()

</details>

---

## 🔹 M:M (Many-to-Many)

<details>
<summary>Q: Best way to read M:M?</summary>

A: JOIN + ResultSetExtractor

</details>

<details>
<summary>Q: Why use JOIN?</summary>

A: Avoid N+1 problem

</details>

<details>
<summary>Q: M:M Insert flow?</summary>

A:
User → Address → Mapping

</details>

<details>
<summary>Q: M:M Update strategy?</summary>

A:
Update → Delete mapping → Reinsert

</details>

---

## 🔹 Delete

<details>
<summary>Q: Steps to delete user completely?</summary>

A:
Delete mapping → Delete address → Delete user

</details>

---

## 🔹 Dynamic Query

<details>
<summary>Q: Why use WHERE 1=1?</summary>

A: Easier to append conditions

</details>

<details>
<summary>Q: How to handle optional filters?</summary>

A: Add condition only if value present

</details>

<details>
<summary>Q: How to do LIKE search?</summary>

A: %value%

</details>

<details>
<summary>Q: How to handle IN clause?</summary>

A: Pass List

</details>

<details>
<summary>Q: Important rule for sorting?</summary>

A: Never bind column name directly

</details>

---

## 🔹 Advanced

<details>
<summary>Q: What is KeyHolder used for?</summary>

A: Get generated ID

</details>

<details>
<summary>Q: What is ResultSetExtractor?</summary>

A: Custom mapping for complex result sets

</details>

---