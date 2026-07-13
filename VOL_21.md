# 📘 Backend Engineer Master Course

# **Volume 17 – Backend Performance & Database Optimization (Interview Master Guide)**

> **Goal:** Learn how experienced backend engineers make APIs faster, reduce database load, and build scalable systems. These questions are very common in interviews because they test practical backend knowledge.

---

# Chapter 1 – Why Performance Matters

Imagine your API

```http
GET /users
```

Response Time

```
4 seconds
```

Users complain.

Interviewer asks:

**How would you improve performance?**

This is what this volume teaches.

---

# Chapter 2 – Where Does Latency Come From?

An API request goes through multiple layers.

```text
Client
   │
   ▼
Load Balancer
   │
   ▼
FastAPI
   │
   ▼
Business Logic
   │
   ▼
PostgreSQL
   │
   ▼
S3 / Redis / External APIs
```

Latency can occur at any layer.

---

# Chapter 3 – Database Indexing ⭐⭐⭐⭐⭐

Suppose you have

```
Users Table

10 Million Rows
```

Query

```sql
SELECT * FROM users
WHERE email='abc@gmail.com';
```

Without Index

Database checks

```
Row 1

Row 2

Row 3

...

Row 10,000,000
```

Very slow.

---

With Index

```
Index

↓

Email

↓

Direct Row
```

Database immediately finds the record.

---

### Interview Answer

> An index is a data structure that allows the database to locate records quickly without scanning the entire table. It improves read performance but slightly increases storage usage and write time.

---

# Chapter 4 – When NOT to Create Indexes

Many beginners think

> More indexes = Better performance.

Wrong.

Every INSERT

Every UPDATE

Every DELETE

must also update indexes.

Too many indexes slow writes.

---

### Good candidates for indexes

* Email
* Username
* User ID
* Order ID
* Foreign Keys

---

# Chapter 5 – N+1 Query Problem ⭐⭐⭐⭐⭐

Very common interview question.

Suppose

100 Orders.

You execute

```sql
SELECT * FROM Orders;
```

Then

For each Order

```sql
SELECT * FROM Users
WHERE id=?
```

Total

```
1 + 100

=

101 Queries
```

Bad.

---

Better

Use JOIN

```sql
SELECT *
FROM Orders
JOIN Users
ON Orders.user_id = Users.id;
```

One query.

Huge improvement.

---

### Interview Answer

The N+1 query problem occurs when an application first loads a list of records and then performs an additional query for each record. It causes unnecessary database calls and can be solved using joins or eager loading.

---

# Chapter 6 – Pagination ⭐⭐⭐⭐⭐

Never return

```
1 Million Rows
```

Instead

```http
GET /products?page=1
```

Return

20 items.

---

Offset Pagination

```sql
LIMIT 20 OFFSET 40;
```

Easy.

---

Cursor Pagination

Uses

```
Last ID

↓

Next Records
```

Much faster

for large datasets.

---

### Interview Question

Offset vs Cursor?

| Offset                | Cursor                |
| --------------------- | --------------------- |
| Simple                | Faster                |
| Slower for large data | Excellent scalability |
| Can skip records      | Stable                |

---

# Chapter 7 – Caching with Redis

Suppose

1000 users request

```
Product List
```

Database executes

1000 queries.

Instead

```
Client

↓

Redis

↓

Hit

↓

Return
```

No database call.

---

If Cache Miss

```
Database

↓

Redis

↓

Client
```

---

### Interview Answer

Redis stores frequently accessed data in memory, reducing database load and improving response time.

---

# Chapter 8 – Connection Pooling ⭐⭐⭐⭐

Without Pool

Every request

```
Connect Database

↓

Run Query

↓

Disconnect
```

Very expensive.

---

With Pool

```
Application

↓

Pool

↓

Existing Connection
```

Reuse connections.

Much faster.

---

### Interview Answer

Connection pooling maintains a pool of reusable database connections, reducing the overhead of creating new connections for every request.

---

# Chapter 9 – Lazy Loading vs Eager Loading

Suppose

User

has

100 Orders.

---

Lazy Loading

Load User first.

Later

Load Orders.

---

Eager Loading

Load

User

*

Orders

Together.

---

Interview Question

When to use Eager Loading?

When related data is definitely needed, because it reduces multiple database queries.

---

# Chapter 10 – Query Optimization

Bad

```sql
SELECT *
FROM Orders;
```

Need only

```
Order ID

Status
```

Better

```sql
SELECT id,status
FROM Orders;
```

Less data.

Faster.

---

Never use

```sql
SELECT *
```

unless necessary.

---

# Chapter 11 – API Optimization

Suppose

One API

calls

Replicate

↓

Topaz

↓

Database

↓

Analytics

↓

Notifications

Sequentially.

Slow.

---

Better

Independent operations

run

in parallel

using async.

Exactly

what FastAPI supports.

---

### YOUR PROJECT

FastAPI

↓

Async Requests

↓

Replicate

↓

Topaz

This is a good real-world example.

---

# Chapter 12 – Compression

Suppose API returns

```
10 MB JSON
```

Compress

↓

1 MB

Much faster.

Common

Gzip

Brotli

---

# Chapter 13 – CDN

Suppose image stored

Mumbai.

User

USA.

Slow.

CloudFront

serves

USA user

from nearby server.

---

# Chapter 14 – Database Normalization

Bad

```
User

Order

Product

Everything

One Table
```

Better

Separate

```
Users

Orders

Products
```

Avoid duplication.

---

Interview Question

Why Normalize?

Reduce redundancy and improve consistency.

---

# Chapter 15 – Denormalization

Sometimes

JOINs become slow.

Store

duplicate

information.

Example

```
Order

↓

Customer Name
```

instead of joining.

Trade-off

More storage.

Faster reads.

---

# Chapter 16 – Read Replica

Database

```
Master

↓

Read Replica

↓

Read Replica
```

Writes

↓

Master

Reads

↓

Replicas

Improves scalability.

---

# Chapter 17 – Explain Query Plan

PostgreSQL

supports

```sql
EXPLAIN ANALYZE
SELECT ...
```

Shows

* Index Usage
* Sequential Scan
* Cost
* Execution Time

Interviewers love this.

---

# Chapter 18 – Performance Improvements in YOUR Project

Interviewer:

**How would you optimize your AI platform?**

Strong Answer

> I would cache frequently accessed metadata using Redis, process image generation asynchronously through RabbitMQ, store images in S3 instead of PostgreSQL, use indexes on user_id and job_id columns, paginate gallery APIs, use connection pooling for PostgreSQL, and horizontally scale FastAPI workers behind a Load Balancer.

Excellent answer.

---

# Chapter 19 – Real Interview Questions

---

### What is an Index?

Speeds up database searches.

---

### Why not index every column?

Indexes consume storage and slow INSERT, UPDATE, and DELETE operations.

---

### Explain N+1 Problem.

Loading parent records and then executing an additional query for each parent record.

---

### Offset vs Cursor Pagination?

Cursor is better for very large datasets.

---

### What is Connection Pooling?

Reuse database connections.

---

### Redis vs PostgreSQL?

| Redis     | PostgreSQL       |
| --------- | ---------------- |
| In-memory | Disk-based       |
| Very Fast | Persistent       |
| Cache     | Primary Database |

---

### What is Cache Hit?

Requested data found in cache.

---

### Cache Miss?

Cache doesn't contain data, so the application retrieves it from the database and usually stores it back in the cache.

---

### Lazy vs Eager Loading?

Lazy loads related data only when accessed. Eager loads related data together in the initial query.

---

### How would you optimize a slow API?

1. Add indexes.
2. Optimize SQL queries.
3. Use Redis.
4. Reduce external API calls.
5. Add pagination.
6. Use async processing.
7. Enable connection pooling.
8. Compress responses.
9. Move long-running work to background jobs.

---

# ⭐ Scenario-Based Questions

### Your `/gallery` API is slow. What would you do?

A strong approach:

* Check SQL execution time.
* Add indexes on `user_id` or `created_at`.
* Return only required columns.
* Add pagination.
* Cache popular results in Redis.
* Use a CDN for image delivery.
* Profile the API to identify bottlenecks.

---

### PostgreSQL CPU is at 100%. What could be wrong?

Possible reasons:

* Missing indexes
* Full table scans
* N+1 queries
* Too many concurrent connections
* Expensive JOINs
* Long-running transactions

---

### Your Redis server goes down. What happens?

The application should fall back to PostgreSQL. Response times may increase because cached data is no longer available, but the system should continue functioning if Redis is used only as a cache.

---

# 📌 Volume 17 Summary

After this volume, you should confidently understand:

* Database indexing
* Choosing the right indexes
* N+1 query problem
* Pagination (Offset vs Cursor)
* Redis caching
* Connection pooling
* Lazy vs Eager loading
* Query optimization
* API optimization with async
* Compression
* CDN
* Normalization vs Denormalization
* Read replicas
* `EXPLAIN ANALYZE`
* Practical performance optimization for your FastAPI AI platform

---

# 🎯 Next Volume (Volume 18)

We'll cover **Security for Backend Engineers**, including:

* JWT authentication
* OAuth 2.0
* Refresh tokens
* CSRF vs XSS vs SQL Injection
* Password hashing (bcrypt/Argon2)
* HTTPS and TLS
* API security
* Rate limiting
* Secrets management
* Secure coding practices

These are essential topics for backend interviews and directly relate to the authentication and authorization systems you've described in your resume.
