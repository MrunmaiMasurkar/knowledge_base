Excellent. This is one of the most important volumes because **90% of backend interviews ask these concepts**. Since your resume contains **FastAPI, NestJS, REST APIs, JWT, PostgreSQL, GraphQL, Firebase**, these questions are almost guaranteed.

---

# 📘 VOLUME 8 – Backend Fundamentals (Complete Interview Edition)

---

# Chapter 1 – What happens when you type google.com?

This is an interview favorite.

### Answer

```
Browser

↓

DNS Lookup

↓

IP Address

↓

TCP Connection

↓

HTTPS Handshake

↓

HTTP Request

↓

Server

↓

Database/API

↓

HTTP Response

↓

Browser Renders Page
```

Explain simply:

> When I type a URL, the browser first finds the server's IP using DNS, establishes a TCP connection, performs the HTTPS handshake if required, sends the HTTP request, receives the response from the server, and finally renders the webpage.

---

# Chapter 2 – What is HTTP?

### Answer

> HTTP (HyperText Transfer Protocol) is the communication protocol used between a client and a server.

Example

```
Frontend

↓

HTTP

↓

Backend
```

---

# Chapter 3 – HTTP vs HTTPS

| HTTP          | HTTPS                |
| ------------- | -------------------- |
| Not encrypted | Encrypted            |
| Port 80       | Port 443             |
| Less secure   | Secure using SSL/TLS |

Interview answer:

> HTTPS encrypts communication between the client and server, protecting sensitive information such as passwords and authentication tokens.

---

# Chapter 4 – What is REST API?

### Answer

> REST (Representational State Transfer) is an architectural style where resources are accessed using HTTP methods.

Example:

```
GET /users
POST /users
PUT /users/1
DELETE /users/1
```

---

# Chapter 5 – HTTP Methods

## GET

Retrieve data

```
GET /users
```

---

## POST

Create

```
POST /users
```

---

## PUT

Update entire object

```
PUT /users/1
```

---

## PATCH

Update only selected fields

```
PATCH /users/1
```

Example

Old

```
Name
Email
Age
```

PATCH

```
Age only
```

---

## DELETE

Delete resource

```
DELETE /users/1
```

---

# Chapter 6 – Status Codes

Most important ones

## 200

Success

---

## 201

Created

Example

```
POST /users
```

---

## 204

Success

No Content

---

## 400

Bad Request

Invalid input.

---

## 401

Unauthorized

No authentication.

---

## 403

Forbidden

Authenticated

But no permission.

---

## 404

Not Found

---

## 409

Conflict

Duplicate email.

---

## 429

Too Many Requests

Rate limiting.

---

## 500

Internal Server Error

---

# Chapter 7 – JWT

One of the most common questions.

## What is JWT?

> JWT (JSON Web Token) is a secure token used for authentication between the client and server.

---

Architecture

```
Login

↓

Server

↓

JWT Token

↓

Client stores Token

↓

Authorization Header

↓

Server verifies Token
```

---

# Chapter 8 – JWT Structure

JWT has three parts.

```
Header

.

Payload

.

Signature
```

---

Header

Algorithm

---

Payload

User ID

Role

Expiry

---

Signature

Prevents tampering.

---

# Chapter 9 – Why JWT?

Advantages

* Stateless
* Fast
* Easy for microservices
* No session storage

---

# Chapter 10 – Sessions vs JWT

| Session               | JWT                |
| --------------------- | ------------------ |
| Stored on Server      | Stored on Client   |
| Needs Server Memory   | Stateless          |
| Better logout control | Better scalability |

---

# Chapter 11 – Cookies vs Local Storage

Interview question.

Cookies

* Automatically sent
* Smaller
* Often used with sessions

Local Storage

* JavaScript access
* Stores JWT
* Larger storage

---

# Chapter 12 – Authentication vs Authorization

Very common.

Authentication

```
Who are you?
```

Authorization

```
What are you allowed to do?
```

Example

```
Login

↓

Authentication

↓

Admin?

↓

Authorization
```

---

# Chapter 13 – CORS

Interview favorite.

## What is CORS?

Suppose

```
Frontend

localhost:3000

Backend

localhost:8000
```

Browser blocks request.

Backend enables

```
Access-Control-Allow-Origin
```

Then request succeeds.

---

# Chapter 14 – Middleware

Question

What is Middleware?

Answer

> Middleware is code that executes before the request reaches the controller.

Example

```
Request

↓

Logging

↓

Authentication

↓

Controller
```

---

# Chapter 15 – FastAPI Middleware

Example

* Logging
* Authentication
* Request timing

---

# Chapter 16 – Dependency Injection

Already studied.

FastAPI

```
Depends()
```

NestJS

Constructor Injection.

---

# Chapter 17 – Validation

FastAPI

```
Pydantic
```

Checks

* Required fields
* Email
* Numbers
* Length

Before controller.

---

# Chapter 18 – GraphQL

Your resume mentions GraphQL.

Interviewer may ask.

## What is GraphQL?

Answer

> GraphQL allows the client to request exactly the data it needs using a single endpoint.

REST

```
GET /users

GET /orders

GET /products
```

GraphQL

```
POST /graphql
```

One endpoint.

---

# Chapter 19 – GraphQL vs REST

REST

Many endpoints.

GraphQL

Single endpoint.

REST

Sometimes over-fetching.

GraphQL

Exact fields.

---

# Chapter 20 – API Versioning

Example

```
/api/v1/users

/api/v2/users
```

Allows backward compatibility.

---

# Chapter 21 – Pagination

Instead of

```
100000 Users
```

Return

```
20 Users

Next Page
```

Example

```
?page=2

&limit=20
```

---

# Chapter 22 – Idempotency

Interview favorite.

GET

Idempotent

POST

Not always.

DELETE

Usually idempotent.

PUT

Idempotent.

---

# Chapter 23 – Caching

Why?

Reduce database load.

Improve speed.

Use Redis.

---

# Chapter 24 – Rate Limiting

Suppose

```
1000 Requests

Per Minute
```

Reject after limit.

HTTP

429

---

# Chapter 25 – WebSockets

REST

Request

↓

Response

WebSocket

Connection stays open.

Useful for

* Chat
* Live notifications
* Job status updates

---

# Chapter 26 – Long Running Jobs

Your AI project.

Wrong

```
User waits

2 Minutes
```

Better

```
Job Queue

↓

Background Worker

↓

Status API
```

---

# Chapter 27 – Logging

Every API should log

* User
* API
* Time
* Status
* Error

---

# Chapter 28 – Exception Handling

FastAPI

Global Exception Handler.

Instead of

```
500

Stack Trace
```

Return

```
{
 "message":"Something went wrong"
}
```

---

# Chapter 29 – Environment Variables

Never hardcode

```
API Keys

Passwords

Secrets
```

Use

```
.env
```

---

# Chapter 30 – API Security

Interview question.

How secure APIs?

Answer

* JWT Authentication
* HTTPS
* Input Validation
* Rate Limiting
* Role-Based Access Control (RBAC)
* SQL Injection Prevention (parameterized queries/ORM)
* Logging and Monitoring

---

# Chapter 31 – Database Connection Pooling

Instead of creating a new database connection for every request, maintain a pool of reusable connections.

Benefits:

* Faster responses
* Lower overhead
* Better scalability

---

# Chapter 32 – ORM

Your resume uses TypeORM.

Question

Why ORM?

Answer

> ORM maps database tables to programming language objects, allowing developers to work with classes and objects instead of writing SQL for every operation.

Examples:

* TypeORM
* SQLAlchemy
* Hibernate

---

# Chapter 33 – N+1 Query Problem

Suppose you fetch 100 users, then execute one query per user to fetch their orders.

Total:

```
1 + 100 = 101 Queries
```

Instead, use joins or eager loading.

---

# ⭐ Interview Questions Based on Your Resume

### FastAPI

* Why FastAPI?
* How does FastAPI validate requests?
* What is async/await?
* What is middleware?
* What is dependency injection?

---

### NestJS

* What is a controller?
* What is a service?
* What is a module?
* How does dependency injection work?
* What is a guard?
* What is middleware?

---

### Authentication

* JWT
* Firebase
* Authentication vs Authorization
* Role-Based Access Control (RBAC)

---

### GraphQL

* Why GraphQL?
* GraphQL vs REST
* What is a resolver?
* What are queries and mutations?

---

### Database

* PostgreSQL
* ORM
* Transactions
* Indexes
* Connection Pooling

---

# 🎯 10 Questions You Should Answer Instantly

1. Why FastAPI instead of Flask?
2. Why PostgreSQL instead of MongoDB?
3. Explain JWT.
4. Authentication vs Authorization.
5. What happens when an API request comes to FastAPI?
6. GET vs POST vs PUT vs PATCH.
7. What is middleware?
8. What is GraphQL?
9. What is CORS?
10. Why use an ORM?

---

# 📚 Your Interview Handbook Progress

You now have:

* ✅ Volume 1 – Resume & Projects
* ✅ Volume 2 – Python Fundamentals
* ✅ Volume 3 – Automation
* ✅ Volume 4 – SQL
* ✅ Volume 5 – OOP
* ✅ Volume 6 – HR & Behavioral
* ✅ Volume 7 – System Design
* ✅ **Volume 8 – Backend Fundamentals**

These eight volumes cover the majority of technical and screening questions for backend, automation, and AI/backend roles. The next logical volume would be **Volume 9 – Data Structures & Coding Patterns for Interviews**, focused on the level typically expected from candidates with 1–3 years of experience.
