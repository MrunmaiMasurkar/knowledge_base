# 📘 Backend Engineer Master Course

# **Volume 12 – Microservices, API Gateway & Distributed Systems (Interview Master Guide)**

> **Goal:** This is one of the most important backend interview topics. By the end of this volume, you'll understand how companies like Amazon, PhonePe, Razorpay, and AI startups design scalable backend systems.

---

# Chapter 1 – Why Microservices?

Let's start with a simple e-commerce application.

Initially, everything is inside one application.

```text
User
   │
   ▼
┌──────────────────────────┐
│        Backend           │
│--------------------------│
│ Authentication           │
│ Products                 │
│ Orders                   │
│ Payments                 │
│ Notifications            │
└──────────────────────────┘
```

This is called a **Monolith**.

---

## Problems with Monolith

Imagine:

* 100 developers
* 5 teams
* 10 million users

Problems:

* Entire application is deployed together.
* One bug can affect the whole system.
* Scaling is difficult.
* Codebase becomes huge.
* Long deployment times.

---

# Interview Answer

### What is a Monolithic Architecture?

A monolithic application contains all business modules in a single deployable application. While it is easier to develop initially, it becomes difficult to maintain, scale, and deploy as the application grows.

---

# Chapter 2 – What are Microservices?

Instead of one large application,

split it into multiple independent services.

```text
User
 │
 ▼
API Gateway
 │
 ├───────────────┐
 ▼               ▼
Auth Service   Product Service
 │               │
 ▼               ▼
User DB       Product DB

       ▼
Order Service

       ▼
Order DB

       ▼
Notification Service
```

Each service has

* Its own code
* Its own database
* Independent deployment

---

# Interview Answer

### What is a Microservice?

A microservice is a small, independently deployable service responsible for a single business capability. Each microservice can be developed, deployed, and scaled independently.

---

# Chapter 3 – Why Companies Use Microservices

Imagine

Authentication receives

```text
500 requests/sec
```

Orders receive

```text
50 requests/sec
```

Products receive

```text
20 requests/sec
```

Should we scale entire backend?

No.

Scale only

```text
Authentication Service
```

Huge cost saving.

---

# Interview Question

Why Microservices?

Answer

* Independent deployment
* Independent scaling
* Better fault isolation
* Smaller codebases
* Faster development
* Different teams can work independently

---

# Chapter 4 – Single Responsibility

One service.

One responsibility.

Good

```text
Auth Service

↓

Login

Register

JWT
```

Bad

```text
Auth Service

↓

Login

Payments

Orders

Email
```

---

# Chapter 5 – API Gateway

Suppose client has

10 services.

Without Gateway

```text
Client

↓

Auth

↓

Orders

↓

Products

↓

Payment

↓

Notification
```

Client manages all URLs.

Very difficult.

---

Instead

```text
Client

↓

API Gateway

↓

Routes Request

↓

Correct Service
```

---

Gateway Responsibilities

* Authentication
* Routing
* Rate Limiting
* Logging
* Load Balancing
* SSL Termination

---

Interview Question

What is API Gateway?

Answer

API Gateway acts as a single entry point for all client requests. It routes requests to appropriate microservices and handles cross-cutting concerns such as authentication, logging, rate limiting, and load balancing.

---

# Chapter 6 – Communication Between Services

There are two methods.

## Synchronous

```text
Order Service

↓

Payment Service

↓

Wait
```

Order waits.

Simple.

---

## Asynchronous

```text
Order Created

↓

RabbitMQ

↓

Payment

↓

Notification

↓

Email
```

No waiting.

More scalable.

---

Interview Question

REST vs Message Queue?

REST is synchronous.

Message queues enable asynchronous communication and improve scalability and resilience.

---

# Chapter 7 – Database per Service

Wrong

```text
Auth

↓

Products

↓

Orders

↓

One Database
```

Correct

```text
Auth

↓

User DB

Products

↓

Product DB

Orders

↓

Order DB
```

Reason

Services become independent.

---

# Chapter 8 – Service Discovery

Imagine

Product Service

moves

from

```text
10.0.0.5
```

to

```text
10.0.0.9
```

How does Order Service know?

Service Discovery.

Example

* Kubernetes DNS
* Eureka
* Consul

---

# Chapter 9 – Load Balancer

One server.

```text
1000 requests/sec
```

Server crashes.

Instead

```text
Load Balancer

↓

Server 1

↓

Server 2

↓

Server 3
```

Requests distributed.

---

Interview Question

Difference between Load Balancer and API Gateway?

| API Gateway        | Load Balancer        |
| ------------------ | -------------------- |
| Routes APIs        | Distributes traffic  |
| Authentication     | Doesn't authenticate |
| Rate Limiting      | No                   |
| Logging            | Limited              |
| Client Entry Point | Infrastructure Layer |

---

# Chapter 10 – Distributed Transactions

Suppose

User buys laptop.

Steps

```text
Order Created

↓

Payment Success

↓

Inventory Reduced

↓

Email Sent
```

Payment succeeds.

Inventory fails.

Now?

Very difficult.

---

# Saga Pattern

Instead of one huge transaction,

each service performs

its own transaction.

If failure happens,

undo previous work.

Example

```text
Payment Done

↓

Inventory Failed

↓

Refund Payment
```

---

Interview Question

Why not use one SQL transaction?

Because services have separate databases.

Distributed transactions are difficult.

Saga Pattern is commonly used.

---

# Chapter 11 – Circuit Breaker

Suppose

Payment Service

is down.

Without Circuit Breaker

```text
Retry

Retry

Retry

Retry
```

Everything hangs.

With Circuit Breaker

```text
Payment Down

↓

Immediately Return Error

↓

Retry Later
```

Prevents cascading failures.

---

# Chapter 12 – Microservices in YOUR AI Project

Your architecture

```text
Client

↓

FastAPI

↓

Provider Router

↓

Replicate

↓

Topaz

↓

Postgres
```

If redesigned

```text
API Gateway

↓

Authentication Service

↓

Image Generation Service

↓

Upscaling Service

↓

Color Separation Service

↓

Notification Service

↓

Billing Service
```

Each can scale independently.

---

# Chapter 13 – Where Redis Fits

```text
API Gateway

↓

Redis

↓

Authentication

↓

Image Generation

↓

Orders
```

Redis stores

* Sessions
* Cache
* Rate limits

---

# Chapter 14 – Where RabbitMQ Fits

Suppose image generation

takes

45 seconds.

Don't wait.

```text
Client

↓

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

Database

↓

Completed
```

Client immediately receives

```text
Job Accepted
```

Later

```text
Completed
```

This is much better than making the client wait.

---

# Chapter 15 – Real Interview Questions

## Why Microservices?

Independent deployment, scaling, and maintenance.

---

## Monolith vs Microservices?

| Monolith          | Microservices              |
| ----------------- | -------------------------- |
| One application   | Many small services        |
| One deployment    | Independent deployment     |
| One database      | Usually separate databases |
| Difficult scaling | Easy scaling               |
| Easier initially  | More complex but scalable  |

---

## What is API Gateway?

Single entry point that routes requests and handles authentication, logging, rate limiting, and other shared concerns.

---

## Why separate databases?

To avoid tight coupling and allow services to evolve independently.

---

## REST vs gRPC?

REST:

* JSON
* Human-readable
* Great for public APIs

gRPC:

* Protocol Buffers
* Faster
* Efficient for internal service-to-service communication

---

## When should you choose Microservices?

When the application is large, developed by multiple teams, and requires independent deployment or scaling. Small applications often benefit from starting as a monolith.

---

## Biggest disadvantage of Microservices?

* Operational complexity
* Network failures
* Monitoring
* Distributed debugging
* Data consistency challenges

---

## How would you convert your AI platform into Microservices?

I would separate responsibilities into independent services:

* Authentication Service
* Image Generation Service
* Upscaling Service
* Color Separation Service
* Billing Service
* Notification Service

An API Gateway would route requests. Redis would be used for caching and job status, and a message queue such as RabbitMQ would handle long-running AI processing asynchronously.

---

# ⭐ If an interviewer asks

**"Have you worked on Microservices?"**

Be honest.

A strong answer is:

> "In my previous projects, I primarily worked on modular backend services rather than a fully distributed microservices architecture. However, I understand microservice principles such as independent deployment, API Gateway, service communication, database-per-service, and asynchronous processing. Based on my FastAPI AI image generation project, I can explain how it could be decomposed into microservices for better scalability."

This is truthful and demonstrates architectural understanding.

---

# 📌 Volume 12 Summary

After this volume, you should confidently understand:

* Monolithic architecture
* Microservices
* Benefits and trade-offs
* API Gateway
* Load Balancer
* Service Discovery
* Database per Service
* REST vs Message Queues
* Saga Pattern
* Circuit Breaker
* Distributed Systems basics
* Applying microservices to your own AI and backend projects

---

## 🚀 Next Volume (Volume 13)

We'll cover **RabbitMQ, Kafka, Background Jobs, Event-Driven Architecture, Celery, and Worker Processes**. These topics are frequently discussed alongside microservices and are essential for designing scalable backend systems.
