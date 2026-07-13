# 📘 Backend Engineer Master Course

# **Volume 18 – Backend Security (Interview Master Guide)**

> **Goal:** Security is one of the favorite interview topics for backend developers. Since your resume mentions **JWT Authentication**, **RBAC**, **FastAPI**, **NestJS**, and **PostgreSQL**, you should be able to answer these confidently.

---

# Chapter 1 – Why Security Matters

Suppose your API is

```http
POST /login
```

Anyone can call it.

Without security,

someone can

* Access another user's account
* Modify data
* Delete data
* Steal passwords

Backend security prevents this.

---

# Chapter 2 – Authentication vs Authorization ⭐⭐⭐⭐⭐

This is probably the most common interview question.

## Authentication

> **Who are you?**

Example

```
Email

Password
```

System verifies.

User is authenticated.

---

## Authorization

> **What are you allowed to do?**

Example

```
Admin

↓

Delete Users
```

Regular User

↓

Cannot Delete Users.

---

### Interview Answer

Authentication verifies the identity of a user.

Authorization determines what an authenticated user is allowed to access or perform.

---

# Chapter 3 – JWT Authentication ⭐⭐⭐⭐⭐

This is directly from your resume.

Flow

```text
Client

↓

POST /login

↓

FastAPI

↓

Verify Password

↓

Generate JWT

↓

Return Token
```

Future Requests

```text
Client

↓

Authorization

Bearer Token

↓

Backend

↓

Verify JWT

↓

Return Response
```

---

## JWT Structure

JWT has

```text
Header

Payload

Signature
```

---

Example

```text
xxxxx.yyyyy.zzzzz
```

---

## Payload

Usually contains

```json
{
 "user_id": 10,
 "role":"admin"
}
```

---

## Signature

Prevents token tampering.

---

### Interview Question

Why JWT?

Answer

JWT is stateless. The server does not need to store session information for every user. The client includes the token with each request, and the server verifies its signature.

---

# Chapter 4 – Refresh Token ⭐⭐⭐⭐⭐

Suppose

JWT expires

after

15 minutes.

User shouldn't login again.

Solution

Refresh Token.

Flow

```text
Login

↓

Access Token

15 min

↓

Refresh Token

7 Days

↓

New Access Token
```

---

Interview Question

Why Refresh Token?

Improves security by keeping access tokens short-lived while allowing users to remain logged in without re-entering credentials frequently.

---

# Chapter 5 – Password Hashing

Never store

```text
password123
```

Database.

Instead

```text
password123

↓

bcrypt

↓

$2a$10$....
```

---

Even database admins

cannot know

password.

---

Interview Question

Why hash passwords?

To prevent exposure of user passwords if the database is compromised.

---

# Chapter 6 – bcrypt vs Encryption

Hashing

One Way.

Cannot reverse.

Encryption

Two Way.

Can decrypt.

Passwords

Always

Hash.

---

# Chapter 7 – HTTPS

Without HTTPS

```text
Client

↓

Password

↓

Internet
```

Can be intercepted.

---

With HTTPS

```text
Encrypted

↓

Safe
```

Uses

TLS.

---

Interview Question

Why HTTPS?

Encrypts communication between client and server, protecting sensitive information from interception.

---

# Chapter 8 – SQL Injection ⭐⭐⭐⭐⭐

Bad

```python
query = "SELECT * FROM users WHERE email='"+email+"'"
```

Attacker sends

```sql
' OR 1=1 --
```

Now

Database returns

every user.

---

Correct

Parameterized Query.

```python
cursor.execute(
"SELECT * FROM users WHERE email=%s",
(email,)
)
```

Safe.

---

Interview Question

How do you prevent SQL Injection?

* Parameterized queries
* ORM (TypeORM, SQLAlchemy)
* Input validation

---

YOUR PROJECT

NestJS + TypeORM

already prevents

most SQL Injection.

---

# Chapter 9 – XSS (Cross Site Scripting)

Attacker enters

```html
<script>alert("Hacked")</script>
```

Browser executes.

Bad.

---

Prevent

* Escape HTML
* Sanitize input
* Content Security Policy

---

# Chapter 10 – CSRF

User logged into Bank.

Attacker opens

another website.

Hidden request

```http
POST /transfer
```

Browser sends cookies.

Money transferred.

---

Prevent

* CSRF Token
* SameSite Cookies

---

Interview Question

Difference

XSS vs CSRF?

| XSS              | CSRF              |
| ---------------- | ----------------- |
| Malicious Script | Fake Request      |
| Executes JS      | Uses User Session |

---

# Chapter 11 – RBAC ⭐⭐⭐⭐⭐

You already used this.

Example

```text
Admin

↓

Delete User

Create User

View Reports

-----------------

Employee

↓

View Reports
```

Backend checks

Role

before API.

---

Interview Answer

RBAC (Role-Based Access Control) restricts access based on user roles rather than individual users.

---

# Chapter 12 – Rate Limiting

Suppose

Attacker

calls

```text
/login
```

10000 times.

Prevent

```text
100 Requests/minute
```

Redis

stores

count.

---

YOUR PROJECT

You mentioned

Rate Limiting Table.

Excellent example.

---

# Chapter 13 – API Keys

Some AI providers

need

```text
API KEY
```

Never expose

Frontend.

Store

Environment Variables.

---

Interview Question

Where should API Keys be stored?

Answer

Server-side environment variables or secret management services, never in frontend code or version control.

---

# Chapter 14 – Environment Variables

Never

```python
API_KEY="abcd123"
```

inside code.

Instead

```text
.env

↓

API_KEY=******
```

---

# Chapter 15 – OAuth

Suppose

Login

using

Google.

No password.

Flow

```text
User

↓

Google

↓

Token

↓

Backend
```

OAuth.

---

Interview Question

Difference

OAuth vs JWT?

JWT

Authentication Token.

OAuth

Authorization Framework.

---

# Chapter 16 – CORS

Browser

Frontend

```text
localhost:3000
```

Backend

```text
localhost:8000
```

Blocked.

Need

CORS.

---

Interview Question

What is CORS?

CORS controls which origins are allowed to access backend resources from browsers.

---

# Chapter 17 – Logging

Never log

```text
Password

Credit Card

JWT
```

Only log

* Request ID
* Endpoint
* Status
* Error

---

# Chapter 18 – Security in YOUR AI Project

Interviewer

How was security handled?

Strong Answer

> Firebase handled user authentication. After successful authentication, user details were synchronized to PostgreSQL for workspace and subscription management. Backend APIs were protected using JWT-based authentication, role-based authorization where applicable, rate limiting to prevent abuse, and API keys for external AI providers were stored securely on the server.

---

# Chapter 19 – Real Interview Questions

---

### Authentication vs Authorization?

Authentication verifies identity.

Authorization verifies permissions.

---

### Why JWT?

Stateless authentication.

---

### What is JWT?

JSON Web Token containing claims signed by the server.

---

### Why Refresh Token?

Allows short-lived access tokens while keeping users logged in.

---

### Why Hash Passwords?

Passwords should never be stored in plaintext.

---

### bcrypt vs Encryption?

| bcrypt    | Encryption     |
| --------- | -------------- |
| One-way   | Two-way        |
| Passwords | Sensitive Data |

---

### SQL Injection?

Malicious SQL execution.

Prevent using parameterized queries and ORMs.

---

### XSS?

Malicious JavaScript execution.

---

### CSRF?

Forces authenticated users to perform unintended actions.

---

### RBAC?

Role-Based Access Control.

---

### OAuth?

Authorization framework used for third-party login.

---

### CORS?

Browser security mechanism controlling cross-origin requests.

---

# ⭐ Scenario-Based Questions

## Suppose your JWT secret leaks.

What happens?

An attacker could generate valid-looking JWTs.

Solution:

* Rotate the secret immediately.
* Invalidate existing tokens if possible.
* Reissue tokens to users.

---

## User steals another user's JWT.

How can you reduce damage?

* Short-lived access tokens.
* Refresh tokens.
* HTTPS.
* Secure token storage.
* Token revocation for sensitive systems.

---

## AI Provider API Key leaks.

What would you do?

* Revoke the key.
* Generate a new key.
* Update server configuration.
* Monitor logs for misuse.

---

# ⭐ YOUR Resume Mapping

| Resume             | Security Concept         |
| ------------------ | ------------------------ |
| JWT Authentication | JWT                      |
| RBAC               | Authorization            |
| Firebase           | Authentication           |
| PostgreSQL         | Secure Data Storage      |
| Rate Limiting      | API Protection           |
| AI Providers       | API Keys                 |
| FastAPI            | Protected REST APIs      |
| NestJS Guards      | Authorization Middleware |

---

# 📌 Volume 18 Summary

After this volume, you should understand:

* Authentication vs Authorization
* JWT
* Refresh Tokens
* Password Hashing
* bcrypt vs Encryption
* HTTPS/TLS
* SQL Injection
* XSS
* CSRF
* RBAC
* Rate Limiting
* API Keys
* Environment Variables
* OAuth
* CORS
* Secure Logging
* How these concepts apply to your FastAPI/NestJS projects

---

# 🎯 Next Volume (Volume 19 – Advanced SQL & PostgreSQL)

This volume will cover the SQL knowledge expected from backend engineers:

* Joins (Inner, Left, Right, Full)
* GROUP BY and HAVING
* Window Functions (`ROW_NUMBER`, `RANK`, `DENSE_RANK`)
* Common Table Expressions (CTEs)
* Transactions and ACID
* Isolation Levels
* Deadlocks
* Locks (Optimistic vs Pessimistic)
* Normalization (1NF–BCNF)
* PostgreSQL internals and interview scenarios

These are among the most frequently tested topics in backend interviews and will significantly strengthen your database expertise.
