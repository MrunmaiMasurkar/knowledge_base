# 📘 Backend Engineer Master Course

# **Volume 19 – Advanced SQL & PostgreSQL (Interview Master Guide)**

> **Goal:** SQL is one of the highest-weight topics in backend interviews. This volume will take you from writing basic queries to understanding how PostgreSQL actually works internally.

---

# Chapter 1 – SQL Execution Order ⭐⭐⭐⭐⭐

Many candidates know SQL syntax but not the execution order.

Example

```sql
SELECT name, salary
FROM Employee
WHERE salary > 50000
GROUP BY name, salary
HAVING COUNT(*) > 1
ORDER BY salary DESC
LIMIT 10;
```

The database **does not execute this from top to bottom.**

Actual execution order:

```
FROM
↓

WHERE
↓

GROUP BY
↓

HAVING
↓

SELECT
↓

ORDER BY
↓

LIMIT
```

---

### Interview Question

**What is the execution order of an SQL query?**

Answer:

> SQL first reads data using FROM, filters rows with WHERE, groups data using GROUP BY, filters groups with HAVING, selects columns, sorts using ORDER BY, and finally applies LIMIT.

---

# Chapter 2 – Joins ⭐⭐⭐⭐⭐

Suppose

Users

| id | name  |
| -- | ----- |
| 1  | John  |
| 2  | Alice |

Orders

| id  | user_id |
| --- | ------- |
| 101 | 1       |
| 102 | 1       |
| 103 | 2       |

---

## INNER JOIN

```sql
SELECT *
FROM users u
INNER JOIN orders o
ON u.id=o.user_id;
```

Returns

```
John 101

John 102

Alice 103
```

Only matching rows.

---

## LEFT JOIN

```
Users

+

Matching Orders
```

If user has no order

Still appears.

---

## RIGHT JOIN

Opposite of LEFT JOIN.

---

## FULL JOIN

Everything

from both tables.

---

### Interview Question

Difference between INNER and LEFT JOIN?

INNER JOIN returns only matching rows.

LEFT JOIN returns all rows from the left table and matching rows from the right table.

---

# Chapter 3 – GROUP BY

Suppose

Orders

```
Alice

Alice

Bob

Bob

Bob
```

Query

```sql
SELECT customer,
COUNT(*)
FROM orders
GROUP BY customer;
```

Output

```
Alice 2

Bob 3
```

---

# Chapter 4 – HAVING vs WHERE ⭐⭐⭐⭐⭐

WHERE

Filters rows

before grouping.

HAVING

Filters groups

after grouping.

Example

```sql
SELECT dept,
COUNT(*)
FROM employee
GROUP BY dept
HAVING COUNT(*)>5;
```

---

Interview Question

Difference?

WHERE filters individual rows.

HAVING filters grouped results.

---

# Chapter 5 – Aggregate Functions

Common ones

```
COUNT()

SUM()

AVG()

MIN()

MAX()
```

Know these well.

---

# Chapter 6 – Window Functions ⭐⭐⭐⭐⭐

Most candidates don't know this.

Huge advantage.

Suppose

Employees

```
A 100

B 200

C 300
```

Need ranking.

---

### ROW_NUMBER()

```sql
ROW_NUMBER()
OVER(
ORDER BY salary DESC
)
```

Output

```
C 1

B 2

A 3
```

Always unique.

---

### RANK()

If

```
300

300

200
```

Output

```
1

1

3
```

Gap exists.

---

### DENSE_RANK()

Output

```
1

1

2
```

No gap.

---

Interview Question

Difference

ROW_NUMBER

RANK

DENSE_RANK

Know this perfectly.

---

# Chapter 7 – Common Table Expressions (CTE)

Instead of writing

huge nested query.

Use

```sql
WITH salary_cte AS (

SELECT *

FROM employee

)

SELECT *

FROM salary_cte;
```

Much cleaner.

---

Interview Question

Why CTE?

Improves readability and allows reuse of intermediate query results.

---

# Chapter 8 – Transactions ⭐⭐⭐⭐⭐

Example

Transfer Money

```
Withdraw

↓

Deposit
```

Suppose

Withdraw succeeds.

Deposit fails.

Money lost.

Need

Transaction.

```sql
BEGIN;

UPDATE...

UPDATE...

COMMIT;
```

If error

```
ROLLBACK;
```

Everything undone.

---

# Chapter 9 – ACID Properties ⭐⭐⭐⭐⭐

Most asked PostgreSQL question.

---

### Atomicity

Everything

or

Nothing.

---

### Consistency

Database always remains valid.

---

### Isolation

Transactions don't interfere.

---

### Durability

Committed data survives crashes.

---

Interview Answer

ACID ensures reliable transaction processing by guaranteeing atomicity, consistency, isolation, and durability.

---

# Chapter 10 – Isolation Levels ⭐⭐⭐⭐

PostgreSQL supports

```
Read Uncommitted*

Read Committed

Repeatable Read

Serializable
```

(*Internally behaves like Read Committed.)

---

Read Committed

Most common.

---

Serializable

Highest consistency.

Slowest.

---

Interview Question

Why not always Serializable?

Higher isolation reduces concurrency and can impact performance.

---

# Chapter 11 – Locks

Suppose

Two users

update

same row.

Need lock.

Types

Shared Lock

Read

Exclusive Lock

Write

---

# Chapter 12 – Deadlock ⭐⭐⭐⭐

User A

locks

Table A.

Needs

Table B.

User B

locks

Table B.

Needs

Table A.

Both waiting.

Deadlock.

PostgreSQL

kills

one transaction.

---

Interview Question

What is Deadlock?

Two or more transactions wait on each other indefinitely because each holds a resource the other needs.

---

# Chapter 13 – Optimistic vs Pessimistic Locking

Optimistic

Assume

No conflict.

Check

Version Number.

Fast.

---

Pessimistic

Lock row

Immediately.

Safer.

Slower.

---

Interview Question

When use Optimistic?

Applications

with

few conflicts.

---

# Chapter 14 – PostgreSQL Internals ⭐⭐⭐⭐

Interviewers sometimes ask

Why PostgreSQL?

Basic answer

* ACID compliant
* Strong consistency
* Advanced indexing
* JSON support
* MVCC
* Reliable transactions

---

# Chapter 15 – MVCC ⭐⭐⭐⭐⭐

One of PostgreSQL's biggest features.

Imagine

User A

reading.

User B

writing.

Without MVCC

Readers wait.

With MVCC

Readers

continue

using

older snapshot.

No blocking.

---

Interview Answer

MVCC (Multi-Version Concurrency Control) allows multiple transactions to access the database concurrently by maintaining different versions of rows.

---

# Chapter 16 – Explain Analyze

Never guess

slow query.

Use

```sql
EXPLAIN ANALYZE

SELECT...
```

Shows

* Index Scan
* Sequential Scan
* Cost
* Execution Time

---

# Chapter 17 – PostgreSQL in YOUR Project

Interviewer

Why PostgreSQL?

Strong answer

> We used PostgreSQL because our application required strong transactional consistency for users, workspaces, subscriptions, jobs, and billing-related data. PostgreSQL also provided relational integrity through foreign keys, efficient indexing, JSON support where required, and reliable ACID transactions.

---

# Chapter 18 – Interview Scenarios

---

## User Registration

Need

```
User Table

Workspace

Subscription
```

Should all succeed.

Use

Transaction.

---

## Image Generation

Need

```
Insert Job

Insert Image

Update Credits
```

Transaction.

---

## Why not MongoDB?

Because

Relationships

were important.

Users

↓

Workspace

↓

Plans

↓

Jobs

↓

Assets

Relational database

fits better.

---

# Chapter 19 – Most Asked SQL Queries

## Second Highest Salary

```sql
SELECT MAX(salary)
FROM employee
WHERE salary <
(
SELECT MAX(salary)
FROM employee
);
```

---

## Duplicate Emails

```sql
SELECT email,
COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*)>1;
```

---

## Employees earning above average

```sql
SELECT *

FROM employee

WHERE salary >

(

SELECT AVG(salary)

FROM employee

);
```

---

## Top 3 Salaries

```sql
SELECT *

FROM

(

SELECT salary,

DENSE_RANK()

OVER(

ORDER BY salary DESC

) r

FROM employee

)t

WHERE r<=3;
```

---

## Count Employees Per Department

```sql
SELECT department,

COUNT(*)

FROM employee

GROUP BY department;
```

---

# Chapter 20 – Real Interview Questions

---

### Difference

DELETE vs TRUNCATE vs DROP

| DELETE       | TRUNCATE                            | DROP           |
| ------------ | ----------------------------------- | -------------- |
| Removes rows | Removes all rows                    | Removes table  |
| Can rollback | Usually transactional in PostgreSQL | Removes schema |

---

### CHAR vs VARCHAR

CHAR

Fixed length.

VARCHAR

Variable length.

---

### Primary Key vs Unique Key

Primary Key

* Unique
* Not Null

Unique Key

* Unique
* Can allow NULL (behavior depends on DBMS)

---

### Clustered vs Non-clustered Index

Know the concept:

* Clustered index affects physical order of data (not user-configurable in PostgreSQL like SQL Server).
* Secondary indexes provide additional lookup structures.

---

### Why PostgreSQL?

* ACID
* MVCC
* Reliability
* JSON support
* Strong indexing
* Excellent concurrency

---

# ⭐ Senior-Level Interview Answer

**Interviewer:**

*"How would you optimize PostgreSQL?"*

Answer:

> I would first analyze slow queries using `EXPLAIN ANALYZE`, create indexes on frequently filtered columns, avoid unnecessary `SELECT *`, paginate large result sets, use connection pooling, optimize joins to avoid N+1 queries, use read replicas for heavy read workloads, cache frequently accessed data in Redis, and monitor long-running queries and database metrics.

---

# ⭐ YOUR Resume Mapping

| Resume Project | SQL Concept           |
| -------------- | --------------------- |
| Jobs Table     | Transactions          |
| Workspace      | Foreign Keys          |
| User Table     | Primary Keys          |
| Subscription   | Joins                 |
| Assets Table   | Indexes               |
| PostgreSQL     | ACID                  |
| FastAPI APIs   | Parameterized Queries |
| TypeORM        | ORM + Transactions    |

---

# 📌 Volume 19 Summary

After this volume, you should confidently understand:

* SQL execution order
* INNER, LEFT, RIGHT, FULL JOIN
* GROUP BY vs HAVING
* Aggregate functions
* Window functions (`ROW_NUMBER`, `RANK`, `DENSE_RANK`)
* CTEs (`WITH`)
* Transactions
* ACID properties
* Isolation levels
* Locks and deadlocks
* Optimistic vs pessimistic locking
* MVCC
* `EXPLAIN ANALYZE`
* Common SQL interview problems
* Why PostgreSQL is a strong choice for backend applications

---

# 🎯 Next Volume (Volume 20 – Backend Interview Master Revision)

This will be the capstone volume. Instead of introducing new topics, it will consolidate everything into:

* A rapid revision of all backend concepts
* 100+ commonly asked backend interview questions
* HR + behavioral questions
* Project-based cross-questions on your resume
* Mock interview scenarios (30-minute, 1-hour)
* How to answer confidently when you don't know something
* A roadmap for interviews at companies like Bonami, product startups, and larger tech companies

This final volume will tie together everything from Volumes 1–19 into an interview-ready playbook.
