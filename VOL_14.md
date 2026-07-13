# 📘 Backend Engineer Master Course

# **Volume 11 – Redis (Interview Master Guide)**

> **Goal:** By the end of this volume, you'll understand **why Redis exists, where it's used, how it works internally, and how to answer Redis interview questions confidently.**

---

# Chapter 1 – Why Redis Exists

Imagine your FastAPI application.

```
User

↓

FastAPI

↓

PostgreSQL

↓

Response
```

Suppose one API is

```
GET /products
```

There are **1 million products**.

Every request does

```
SELECT * FROM products;
```

Database gets hit every time.

```
User 1

↓

Postgres

User 2

↓

Postgres

User 3

↓

Postgres
```

Eventually,

Database becomes slow.

CPU increases.

Disk I/O increases.

Response time increases.

---

## Solution

Keep frequently accessed data in RAM.

RAM is **much faster** than disk.

This is Redis.

```
User

↓

FastAPI

↓

Redis

↓

(Postgres only if needed)
```

---

# Interview Answer

### Why Redis?

Redis is an **in-memory data store** used for caching, session storage, rate limiting, pub/sub messaging, and other high-speed operations. It reduces database load and significantly improves application performance.

---

# Chapter 2 – What is Redis?

Redis stands for

> **REmote DIctionary Server**

It is

* NoSQL Database
* Key-Value Store
* In-memory Database

Everything is stored in RAM.

That's why it is extremely fast.

---

# Chapter 3 – Why is Redis Faster?

Suppose

PostgreSQL

```
Read from Disk

↓

Find Data

↓

Return
```

Disk access is comparatively slow.

Redis

```
Read from RAM

↓

Return
```

RAM access is much faster.

Typical latency:

```
Redis

≈ 1 ms

Postgres

≈ 20–100 ms
```

---

Interview Question

### Why is Redis so fast?

**Answer**

Redis stores data in memory (RAM) instead of disk. Memory access is significantly faster than disk I/O. Redis also uses efficient data structures and a single-threaded event loop for handling commands.

---

# Chapter 4 – Key Value Store

Everything in Redis is

```
Key

↓

Value
```

Example

```
User:101

↓

{
name:"Mrunmai",
plan:"Premium"
}
```

---

Another example

```
Image_123

↓

Generated Image URL
```

---

# Chapter 5 – Redis Data Types

Redis supports multiple data structures.

---

## String

```
"user"

↓

"Mrunmai"
```

Most common.

---

## List

```
Notifications

↓

Image Generated

↓

Upscaling Completed

↓

Download Ready
```

---

## Set

Unique values.

```
Users Online

↓

101

↓

202

↓

303
```

Duplicates not allowed.

---

## Hash

Like Dictionary.

```
User:101

↓

Name

↓

Plan

↓

Credits
```

---

## Sorted Set

Useful for

Leaderboard

Ranking

Priority

---

Interview Question

### Which Redis data structure did you use?

For caching

→ String

For User Session

→ Hash

For Leaderboard

→ Sorted Set

---

# Chapter 6 – Caching

Most important Redis topic.

Suppose

```
GET /products
```

First request

```
Redis

↓

Not Found

↓

Postgres

↓

Save to Redis

↓

Return
```

Second request

```
Redis

↓

Found

↓

Return
```

No database query.

Huge performance improvement.

---

Interview Question

### What is Cache Hit?

Data found in Redis.

No database access.

---

### Cache Miss?

Redis doesn't have the data.

Application fetches from database.

Stores in Redis.

Returns response.

---

# Chapter 7 – TTL (Time To Live)

Suppose

Weather API.

Weather changes.

Cache forever?

No.

Store for

```
5 Minutes
```

After

5 minutes

Redis automatically deletes.

Example

```
Weather

↓

Expires

↓

300 Seconds
```

---

Interview Question

Why TTL?

Answer

To automatically remove stale data and prevent serving outdated information.

---

# Chapter 8 – Cache Invalidation

One of the hardest problems.

Example

Product Price

```
₹500
```

Cached.

Now database changes

```
₹600
```

Redis still returns

```
₹500
```

Wrong.

Solution

Delete Redis key.

Or update cache.

---

Interview Question

### What is Cache Invalidation?

Updating or deleting stale cached data whenever the original database changes.

---

# Chapter 9 – Session Storage

Instead of storing login sessions in server memory,

Store in Redis.

```
JWT

↓

Redis

↓

User Session
```

Useful when multiple backend servers exist.

---

# Chapter 10 – Rate Limiting

Suppose

One user sends

```
1000 requests/sec
```

Bad.

Redis counts.

```
User

↓

Redis Counter

↓

100

↓

Limit Reached

↓

429 Too Many Requests
```

---

Interview Question

How does Redis help Rate Limiting?

Answer

Redis stores request counters with expiration time.

If request count exceeds limit,

API rejects further requests.

---

# Chapter 11 – Pub/Sub

Publisher

↓

Redis

↓

Subscribers

Example

Image Generated

↓

Redis

↓

Email Service

↓

Notification Service

↓

Analytics Service

One message.

Multiple listeners.

---

# Chapter 12 – Redis in YOUR FastAPI Project

Your project

```
User

↓

Generate Image

↓

FastAPI

↓

Replicate
```

Generation

45 seconds.

Instead of asking

```
Finished?

Finished?

Finished?
```

Store status.

```
Redis

↓

PROCESSING

↓

UPSCALING

↓

COMPLETED
```

Frontend checks Redis.

Very fast.

---

# Another Example

Store

```
JobID

↓

Status
```

Example

```
Job123

↓

Processing
```

Later

```
Completed
```

---

# Chapter 13 – Redis vs PostgreSQL

| Redis          | PostgreSQL     |
| -------------- | -------------- |
| RAM            | Disk           |
| Extremely Fast | Slower         |
| Temporary Data | Permanent Data |
| Key-Value      | Relational     |
| Cache          | Main Database  |

---

Interview Question

Why not store everything in Redis?

Answer

Redis stores data in memory, making it fast but expensive and not ideal for permanent storage. PostgreSQL provides durability, transactions, indexing, and relational queries, making it suitable as the primary database.

---

# Chapter 14 – Eviction Policies

Suppose

Redis memory full.

What now?

Redis removes data.

Policies

```
LRU

Least Recently Used
```

or

```
TTL Based
```

---

Interview Question

What happens when Redis memory is full?

Redis removes keys according to its configured eviction policy, such as Least Recently Used (LRU), allowing new data to be stored.

---

# Chapter 15 – Redis Persistence

Many people think

Redis only stores in RAM.

Actually

Redis supports persistence.

Methods

### RDB

Snapshot

Every few minutes.

---

### AOF

Append Only File.

Logs every write.

Can recover after crash.

---

Interview Question

Does Redis lose data after restart?

Answer

Not necessarily. Redis supports persistence using RDB snapshots and Append Only Files (AOF), allowing recovery after restarts depending on the configuration.

---

# Chapter 16 – Real System Design Example

Without Redis

```
Client

↓

FastAPI

↓

Postgres

↓

Response
```

With Redis

```
Client

↓

FastAPI

↓

Redis

↓

Cache Hit

↓

Response

OR

↓

Postgres

↓

Redis

↓

Response
```

---

# Chapter 17 – Redis Interview Questions

---

## Why Redis?

For caching, sessions, rate limiting, pub/sub, and reducing database load.

---

## Redis vs PostgreSQL?

Redis is an in-memory key-value store optimized for speed. PostgreSQL is a relational database designed for durable, structured data storage and complex queries.

---

## Why not cache everything?

Some data changes frequently, some is user-specific, and memory is limited. Caching everything would increase memory usage and risk serving stale data.

---

## What is Cache Hit?

Data found in Redis.

---

## Cache Miss?

Redis doesn't contain the requested data, so the application retrieves it from the database and usually stores it in Redis for future requests.

---

## Why TTL?

To automatically expire cached data and keep it fresh.

---

## How does Redis help AI Image Generation?

* Cache frequently accessed metadata.
* Store job status (`PROCESSING`, `UPSCALING`, `COMPLETED`).
* Cache user plans and rate limits.
* Coordinate background workers efficiently.

---

## How does Redis improve performance?

It reduces repeated database queries by serving frequently accessed data directly from memory, lowering latency and database load.

---

# ⭐ If the interviewer asks

**"Have you used Redis professionally?"**

Don't claim experience you don't have.

Say:

> "I haven't used Redis extensively in production, but I understand where it fits into backend architecture. For example, in my FastAPI AI image generation project, Redis could be used to cache frequently accessed data, manage rate limiting, and track long-running job status like image generation and upscaling. I understand its role in improving performance and reducing database load."

This answer is technically sound and honest.

---

# 📌 Volume 11 Summary

You should now understand:

* Why Redis exists
* Redis architecture
* Key-value storage
* Redis data structures
* Caching
* Cache Hit vs Cache Miss
* TTL
* Cache Invalidation
* Session storage
* Rate limiting
* Pub/Sub
* Persistence (RDB & AOF)
* Eviction policies
* Redis vs PostgreSQL
* How Redis fits into your FastAPI and AI projects

These concepts are asked frequently in backend interviews and will prepare you well for the next topic: **Volume 12 – Microservices & API Gateway**, where we'll build on these foundations to discuss scalable distributed systems.
