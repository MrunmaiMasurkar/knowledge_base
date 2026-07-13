# 📘 Backend Engineer Master Course

# **Volume 15 – System Design (Interview Master Guide)**

> **Goal:** This is one of the most valuable topics for backend engineers. Even with 2 years of experience, interviewers often ask simplified system design questions to evaluate how you think, not whether you've memorized architectures.

---

# Before We Start

Many candidates think System Design means:

> "Design YouTube."

No.

For 2–3 years of experience, interviewers ask:

* How would you design an authentication service?
* How would you handle 1 million users?
* How would you reduce latency?
* How would you design your own project?

They are evaluating:

* Breaking down a problem
* Choosing technologies
* Thinking about scalability
* Identifying bottlenecks

---

# Chapter 1 – What is System Design?

Suppose someone asks

> Design an AI Image Generation Platform.

They don't want code.

They want architecture.

Like this

```text
Users

↓

Load Balancer

↓

FastAPI

↓

RabbitMQ

↓

Workers

↓

Replicate

↓

Topaz

↓

S3

↓

Postgres

↓

Redis
```

System Design is deciding

* Components
* Communication
* Storage
* Scaling
* Reliability

---

# Chapter 2 – Steps to Solve ANY System Design Question

Always follow this framework.

## Step 1

Clarify Requirements.

Example

Design WhatsApp.

Ask

* Text only?
* Images?
* Voice?
* Video?

---

## Step 2

Estimate Scale.

Example

Users

10,000?

1 Million?

100 Million?

---

## Step 3

Identify Components.

Example

Authentication

Database

Cache

Queue

Storage

---

## Step 4

Design Architecture.

Draw diagram.

---

## Step 5

Find Bottlenecks.

Database?

Cache?

Workers?

---

## Step 6

Suggest Improvements.

Redis

RabbitMQ

CDN

Load Balancer

---

# This six-step approach works for almost every system design interview.

---

# Chapter 3 – Design Authentication Service

Architecture

```text
Client

↓

API Gateway

↓

Auth Service

↓

JWT

↓

Redis

↓

Postgres
```

Flow

User logs in

↓

Verify Password

↓

Generate JWT

↓

Store Session (optional)

↓

Return Token

---

Interview Question

Why JWT?

Because it is stateless.

The server doesn't need to store session data for every request.

---

# Chapter 4 – Design AI Image Generation System

This is YOUR project.

Architecture

```text
Client

↓

API Gateway

↓

FastAPI

↓

RabbitMQ

↓

Workers

↓

Replicate

↓

Topaz

↓

S3

↓

Postgres

↓

Redis
```

Flow

Generate Image

↓

Queue

↓

Worker

↓

Replicate

↓

Topaz

↓

Save S3

↓

Save Database

↓

Update Job Status

↓

Frontend Polls Status

---

Interview Question

Why Queue?

Generation takes

2 minutes.

Client shouldn't wait.

---

# Chapter 5 – Design URL Shortener

Example

```text
https://google.com/search/interview/backend
```

↓

Generate

```text
abc123
```

Store

```text
abc123

↓

Original URL
```

Database

```text
ShortURL

OriginalURL
```

User opens

```text
tiny.com/abc123
```

↓

Redirect

---

Interview Question

How do you generate unique IDs?

* Auto Increment
* UUID
* Base62 Encoding
* Hash

Base62 is common.

---

# Chapter 6 – Design Notification Service

```text
Order Completed

↓

RabbitMQ

↓

Notification Service

↓

Email

↓

SMS

↓

Push Notification
```

Why Queue?

Don't block Order Service.

---

# Chapter 7 – Design Chat System

```text
User A

↓

WebSocket

↓

Chat Server

↓

Redis

↓

Database

↓

User B
```

Why WebSocket?

Persistent connection.

No polling.

---

Interview Question

Why not HTTP?

HTTP requires repeated requests.

WebSocket allows real-time communication.

---

# Chapter 8 – Design Food Delivery

Services

```text
Authentication

Restaurant

Orders

Payments

Delivery

Notifications
```

Each service

Independent.

---

# Chapter 9 – Design E-Commerce

```text
Authentication

↓

Products

↓

Cart

↓

Orders

↓

Payments

↓

Inventory

↓

Notifications
```

Database

Separate

for each service.

---

# Chapter 10 – Load Balancer

One server

↓

Crash.

Instead

```text
Users

↓

Load Balancer

↓

Server1

↓

Server2

↓

Server3
```

---

Interview Question

What if Server2 dies?

Load Balancer stops sending traffic.

---

# Chapter 11 – Redis

Database slow.

Use cache.

```text
Client

↓

Redis

↓

Hit

↓

Return

OR

↓

Database

↓

Redis

↓

Return
```

---

# Chapter 12 – RabbitMQ

Long-running work.

```text
Client

↓

Queue

↓

Worker

↓

Database
```

Asynchronous.

---

# Chapter 13 – S3

Never store

Images

inside PostgreSQL.

Store

```text
Image

↓

S3

↓

URL

↓

Database
```

Exactly like your project.

---

# Chapter 14 – Scaling Database

Problem

1 million users.

One database.

Slow.

Solutions

Read Replicas

↓

Sharding

↓

Caching

↓

Indexes

---

Interview Question

Difference

Read Replica

vs

Sharding?

Read Replica

Copies database.

Used for reads.

Sharding

Splits database.

Different data

Different servers.

---

# Chapter 15 – Database Index

Without Index

```text
Scan

1 million rows
```

With Index

```text
Direct Lookup
```

Huge speed improvement.

---

# Chapter 16 – CDN

Suppose

Image stored in

Mumbai.

User

USA.

Slow.

CDN

Stores image

closer

to user.

Example

CloudFront.

---

Interview Question

Why CDN?

Reduce latency.

---

# Chapter 17 – Rate Limiting

Suppose

Bot sends

10000 requests/sec.

Redis

Counts.

Limit

100/sec.

Reject.

---

# Chapter 18 – High Availability

Never

One server.

Always

```text
Load Balancer

↓

Server1

↓

Server2

↓

Server3
```

If one dies

Application continues.

---

# Chapter 19 – Designing YOUR AI Platform (Senior-Level Answer)

Interviewer:

Design your AI platform.

Answer:

> The client sends a request through the API Gateway to the FastAPI backend. Authentication is handled using JWT. The backend validates the request and immediately creates a job in PostgreSQL while publishing a message to RabbitMQ. Background workers consume the job, call the AI provider (such as Replicate), perform seamless processing and upscaling with Topaz, upload the final image to Amazon S3, update the job status in PostgreSQL, and cache frequently accessed job metadata in Redis. The frontend periodically checks the job status or receives updates via WebSockets. Multiple worker instances can be added horizontally to increase throughput, and a Load Balancer distributes incoming traffic across multiple FastAPI instances.

That is a strong answer for someone with your experience.

---

# Chapter 20 – Real Interview Questions

### Design TinyURL

Use:

* API
* Database
* Base62 IDs
* Cache
* Load Balancer

---

### Design Image Upload

Use:

* API
* S3
* Database
* CDN

---

### Design Authentication

Use:

* JWT
* Redis
* PostgreSQL

---

### Design Notification System

Use:

* RabbitMQ
* Workers
* Email Service

---

### Design AI Image Generation

Use:

* FastAPI
* RabbitMQ
* Workers
* S3
* Redis
* PostgreSQL

---

### Design WhatsApp

Use:

* WebSocket
* Redis
* Database
* Notification Service

---

# ⭐ Common Follow-Up Questions

## How would you scale to 10 million users?

* Load Balancer
* Multiple application servers
* Redis cache
* Read replicas
* RabbitMQ
* CDN
* Auto Scaling

---

## Single Point of Failure?

Examples:

* One database
* One application server
* One RabbitMQ node

Solution:

* Replication
* Clustering
* Multiple instances
* Backups

---

## What would you monitor?

* CPU usage
* Memory
* API latency
* Error rates
* Queue length
* Database response time
* Worker failures

---

# ⭐ Interview Tips

For 2 years of experience, interviewers are **not expecting perfect system designs**. They want to see that you understand the purpose of common building blocks and can explain why you'd use them.

When answering, focus on **why**:

* **Redis** → Reduce latency and database load.
* **RabbitMQ** → Handle long-running work asynchronously.
* **S3** → Store large files efficiently.
* **Load Balancer** → Distribute traffic and improve availability.
* **Docker** → Consistent deployment.
* **PostgreSQL** → Durable relational data.

If you consistently explain the reasoning behind your choices, you'll make a much stronger impression than simply naming technologies.

---

# 📌 Volume 15 Summary

You now understand:

* A structured approach to system design
* Authentication service design
* AI image generation architecture
* URL shortener
* Notification system
* Chat system basics
* E-commerce architecture
* Load balancing
* Caching with Redis
* Background processing with RabbitMQ
* S3 for object storage
* Read replicas vs sharding
* Database indexing
* CDN
* High availability
* Scaling strategies

---

# 🎯 What's Next?

The next volume is one of the most important for backend interviews:

## **Volume 16 – Design Patterns & SOLID Principles**

This is where you'll learn:

* SOLID principles
* Dependency Injection
* Repository Pattern
* Factory Pattern
* Strategy Pattern
* Singleton Pattern
* Observer Pattern

We'll connect every concept to examples from your **FastAPI**, **NestJS**, and **AI provider router** projects so that the patterns feel practical rather than theoretical. This is a common topic in interviews for backend engineers with 2–5 years of experience.
