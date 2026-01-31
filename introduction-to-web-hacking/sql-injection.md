# SQL Injection
---

## Task 1 – Brief

**SQL Injection (SQLi)** is a web application vulnerability that allows attackers to execute **malicious SQL queries** against a database server.

When user input is not properly validated, attackers may:
- Steal sensitive or customer data
- Delete or alter database records
- Bypass authentication mechanisms

SQLi is one of the **oldest and most dangerous** web vulnerabilities due to its potential impact.

---

## Task 2 – What is a Database?

A **database** is an organized collection of data stored electronically and managed by a **Database Management System (DBMS)**.

### Database Management Systems (DBMS)

DBMS types fall into two categories:
- **Relational**
- **Non-Relational**

This write-up focuses on **Relational databases**.

---

### Databases and Tables

- A single DBMS can host **multiple databases**
- Each database contains **tables**
- Tables store related data separately (e.g., users, products, orders)

---

## Task 3 – What is SQL?

**SQL (Structured Query Language)** is used to communicate with relational databases.

SQL statements are **not case-sensitive**.

---

### SELECT Statement

Used to retrieve data from a table.

- `*` selects all columns
- Specific columns can be selected
- `LIMIT` restricts the number of rows returned
- `WHERE` filters results

---

### WHERE Clause Logic

- `=` Equal
- `!=` Not equal
- `OR` Either condition is true
- `AND` Both conditions must be true

---

### LIKE Operator

Used for pattern matching with `%` wildcard:
- Starts with
- Ends with
- Contains

---

### UNION Statement

- Combines results from multiple SELECT statements
- Rules:
  - Same number of columns
  - Similar data types
  - Same column order

Used to merge results from different tables into one output.

---

### INSERT Statement

Used to add new rows into a table.

---

### UPDATE Statement

Used to modify existing records.
- Uses `SET`
- Can be restricted using `WHERE`

---

### DELETE Statement

Used to remove records.
- Without `WHERE`, **all rows are deleted**
- `LIMIT` can restrict deletions

---

## Task 4 – What is SQL Injection?

SQL Injection occurs when **user input is directly embedded** into SQL queries.

### Example Scenario

- Blog posts accessed via URL parameter
- ID is used directly in SQL query
- Attacker injects SQL syntax to alter logic
- Comment operator (`--`) ignores security checks

This is an example of **In-Band SQL Injection**.

---

## Task 5 – In-Band SQLi

In-Band SQLi uses the **same channel** to inject payloads and receive results.

### Error-Based SQLi
- Database errors displayed on screen
- Useful for schema enumeration

### Union-Based SQLi
- Uses UNION SELECT
- Most common method for dumping data

---

## Task 6 – Blind SQLi (Authentication Bypass)

Blind SQLi provides **little to no visible output**.

Authentication bypass works by:
- Forcing SQL queries to return TRUE
- Using logical conditions like `OR 1=1`

---

## Task 7 – Blind SQLi (Boolean-Based)

- Responses are binary (true/false)
- Used to enumerate:
  - Database names
  - Tables
  - Columns
  - Data

Enumeration is done using:
- UNION SELECT
- LIKE operator
- information_schema

---

## Task 8 – Blind SQLi (Time-Based)

- No visual feedback
- Uses **time delays** to confirm execution
- `SLEEP(x)` executes only on successful queries
- Response delay confirms correctness

---

## Task 9 – Out-of-Band SQLi

- Uses **two communication channels**
- One to inject payload
- One to receive data (DNS / HTTP)
- Requires special database features

---

## Task 10 – Remediation

### Prepared Statements
- SQL structure defined first
- User input passed as parameters
- Prevents query manipulation

### Input Validation
- Use allow-lists
- Restrict expected formats

### Escaping User Input
- Escape characters like `' " $ \`
- Prevents breaking SQL syntax

---
