Perfect. Now we'll do **Volume 4 - SQL**, because almost every backend or automation interview includes SQL. Since you've mentioned you aren't very confident with fundamentals, I'll teach it from an interview perspective.

---

# 📘 VOLUME 4 – SQL COMPLETE (Interview Edition)

---

# Chapter 1 – Database Basics

## Q1. What is a Database?

### Answer

> A database is an organized collection of data that allows us to store, retrieve, update, and delete information efficiently.

Example:

```
User
------
ID
Name
Email
```

---

## Q2. What is DBMS?

**Answer**

A Database Management System (DBMS) is software that manages databases.

Examples:

* PostgreSQL
* MySQL
* Oracle
* SQL Server

---

## Q3. Why did you use PostgreSQL?

### Your Resume Answer

> We had multiple related entities such as Users, Workspaces, Jobs, Images, Plans, and Subscriptions. PostgreSQL provides ACID transactions, foreign keys, joins, and relational integrity, making it well suited for our application.

---

# Chapter 2 – Primary Key

## Q4. What is Primary Key?

A column that uniquely identifies every row.

```
Users

ID    Name

1     Rahul

2     Amit
```

ID is Primary Key.

---

## Q5. Can Primary Key be NULL?

No.

---

# Chapter 3 – Foreign Key

## Q6. What is Foreign Key?

Connects two tables.

Example

```
Users

ID

1

2

Orders

OrderID

UserID

1001      1

1002      2
```

UserID refers to Users.ID.

---

# Chapter 4 – SQL Commands

## Q7. CRUD

Create

Read

Update

Delete

---

## Q8. SELECT

```sql
SELECT * FROM Users;
```

Returns all users.

---

## Q9. WHERE

```sql
SELECT *
FROM Users
WHERE Age > 25;
```

---

## Q10. ORDER BY

```sql
SELECT *
FROM Users
ORDER BY Salary DESC;
```

---

## Q11. LIMIT

```sql
SELECT *
FROM Users
LIMIT 5;
```

---

# Chapter 5 – Aggregate Functions

## Q12. COUNT

```sql
SELECT COUNT(*)
FROM Users;
```

---

## Q13. MAX

```sql
SELECT MAX(Salary)
FROM Employee;
```

---

## Q14. MIN

```sql
SELECT MIN(Salary)
FROM Employee;
```

---

## Q15. AVG

```sql
SELECT AVG(Salary)
FROM Employee;
```

---

## Q16. SUM

```sql
SELECT SUM(Salary)
FROM Employee;
```

---

# Chapter 6 – GROUP BY

## Q17.

Department wise average salary

```sql
SELECT Department,
AVG(Salary)
FROM Employee
GROUP BY Department;
```

---

# Chapter 7 – HAVING

Difference

WHERE filters rows.

HAVING filters groups.

Example

```sql
SELECT Department,
COUNT(*)
FROM Employee
GROUP BY Department
HAVING COUNT(*) > 5;
```

---

# Chapter 8 – JOINS

## Q18. Types

* INNER
* LEFT
* RIGHT
* FULL

---

## Q19. INNER JOIN

Returns matching records.

```
Employee

1 Rahul

2 Amit

Department

1 HR

2 IT
```

Output

```
Rahul HR

Amit IT
```

---

## Q20. LEFT JOIN

Returns all rows from left table.

Even if no match.

---

## Q21. RIGHT JOIN

Returns all rows from right table.

---

## Q22. FULL JOIN

Returns all rows from both tables.

---

# Chapter 9 – Interview Queries

## Q23. Second Highest Salary

```sql
SELECT MAX(Salary)

FROM Employee

WHERE Salary < (

SELECT MAX(Salary)

FROM Employee
);
```

---

## Q24. Find Duplicate Emails

```sql
SELECT Email,
COUNT(*)

FROM Users

GROUP BY Email

HAVING COUNT(*) > 1;
```

---

## Q25. Employees earning more than Manager

```sql
SELECT e.Name

FROM Employee e

JOIN Employee m

ON e.ManagerID = m.ID

WHERE e.Salary > m.Salary;
```

---

## Q26. Top 3 Salaries

```sql
SELECT *

FROM Employee

ORDER BY Salary DESC

LIMIT 3;
```

---

## Q27. Delete Duplicate Records

Interviewers love this.

Don't memorize.

Understand first.

---

# Chapter 10 – Index

## Q28. What is Index?

Answer

Index speeds up searching.

Just like

Book Index.

---

## Q29. Draw

```
Book

↓

Index

↓

Page Number
```

Database

```
Name

↓

Index

↓

Row
```

---

## Q30. Disadvantage?

Slower INSERT.

More storage.

---

# Chapter 11 – Transactions

## Q31. What is Transaction?

A group of SQL operations.

Either

All succeed

or

All fail.

---

## Example

Transfer ₹100

```
Debit A

Credit B
```

Cannot debit without credit.

---

# Chapter 12 – ACID

## Q32. What is ACID?

Atomicity

Consistency

Isolation

Durability

---

### Atomicity

All or Nothing.

---

### Consistency

Database remains valid.

---

### Isolation

Transactions don't interfere.

---

### Durability

Committed data survives crashes.

---

# Chapter 13 – Normalization

## Q33. Why Normalization?

Reduce duplication.

Improve consistency.

---

## Example

Instead of storing

```
User Name

Plan Name

Plan Price
```

in every row,

Create

```
User Table

Plan Table
```

Use Foreign Key.

---

# Chapter 14 – PostgreSQL Questions (Based on YOUR Resume)

---

## Q34. Why PostgreSQL instead of MongoDB?

**Answer**

> PostgreSQL is relational and provides transactions, joins, constraints, and strong consistency. Since our application managed users, workspaces, subscriptions, jobs, and images with clear relationships, PostgreSQL was the better choice.

---

## Q35. How many tables?

Your answer

* User
* Workspace
* Images
* Jobs
* Assets
* Plans
* Subscription
* Rate Limit
* Operations

---

## Q36. What is a Join?

Example

```
User

↓

Subscription

↓

Plan
```

Need Join.

---

# Chapter 15 – Real Questions

## Q37.

Suppose user deletes account.

What happens?

Answer

Use

Foreign Keys

Cascade Delete

or

Soft Delete

depending upon business.

---

## Q38.

Why not store images in PostgreSQL?

Answer

Because images are large binary objects.

Store in S3.

Only metadata in PostgreSQL.

---

## Q39.

Metadata?

Answer

* Image ID
* Object Key
* User ID
* Status
* Timestamp

---

# Chapter 16 – Automation SQL

These are very common.

---

Q40.

CSV

↓

Python

↓

Insert Database

What if duplicate?

Answer

Check Primary Key

or

Unique Constraint

before inserting.

---

Q41.

How do you rollback transaction?

```sql
ROLLBACK;
```

---

Q42.

Commit?

```sql
COMMIT;
```

---

# ⭐ SQL Questions GATP May Ask

* What is SQL?
* Difference between SQL and PostgreSQL?
* Primary Key?
* Foreign Key?
* Join?
* Index?
* Transaction?
* ACID?
* Normalization?
* Why PostgreSQL?
* Write a query to find duplicate emails.
* Find second highest salary.
* Difference between DELETE, TRUNCATE, and DROP.
* Difference between WHERE and HAVING.
* COUNT(*) vs COUNT(column).

---

# 🎯 Now Your Preparation Status

You now have:

✅ Volume 1 – Resume (Complete)

✅ Volume 2 – Python (Complete)

✅ Volume 3 – Automation (Complete)

✅ Volume 4 – SQL (Complete)

---

## What should come next?

From experience, after these topics, interviewers usually move to:

### **Volume 5 – OOP + Design Patterns (Python examples)**

This is one area where many candidates struggle, and it's especially relevant because you've used concepts like **dependency injection, provider routers, wrappers, services, and controllers** in your projects. We'll connect OOP theory directly to your real work instead of giving textbook definitions.

After that, we'll do:

* **Volume 6:** HR + Behavioral Questions
* **Volume 7:** Full Mock Interview (45–60 minutes, realistic cross-questioning)

At that point, you'll have covered nearly everything likely to come up in a screening interview for backend, automation, or AI/backend roles.
