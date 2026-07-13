# 📘 Backend Engineer Master Course

# **Volume 16 – Design Patterns & SOLID Principles (Interview Master Guide)**

> **Goal:** This volume teaches you how experienced backend engineers write maintainable, scalable, and extensible code. Since you've worked with **NestJS**, **FastAPI**, **JWT**, and your **Provider Router**, you'll recognize many of these concepts in practice.

---

# Part 1 — SOLID Principles

SOLID is a set of five design principles that help create software that is easier to maintain, extend, and test.

Think of SOLID as **best practices for writing clean object-oriented code**.

---

# S — Single Responsibility Principle (SRP)

## Definition

> A class should have only one reason to change.

Meaning:

One class → One responsibility.

---

## Bad Example

```text
ImageService

↓

Generate Image

Save Database

Send Email

Create Invoice

Upload S3
```

This class does everything.

If email changes, ImageService changes.

If S3 changes, ImageService changes.

Bad design.

---

## Good Example

```text
ImageGenerationService

↓

Generate Image

-----------------

StorageService

↓

Upload S3

-----------------

NotificationService

↓

Send Email

-----------------

BillingService

↓

Create Invoice
```

Each service has one job.

---

## YOUR PROJECT

Your Provider Router

```python
generate_image()
```

Only routes providers.

It doesn't

* Save Database
* Send Email
* Generate JWT

Excellent example of SRP.

---

## Interview Answer

> Single Responsibility Principle states that a class should have only one responsibility or one reason to change. This improves maintainability and reduces coupling.

---

# O — Open Closed Principle

## Definition

Software should be

Open for extension

Closed for modification

---

Suppose

Initially

Only Replicate.

Later

Need Fal AI.

Bad design

```python
if provider=="replicate":
    ...

if provider=="fal":
    ...
```

Every new provider

Modify existing code.

---

Better

```text
Provider Interface

↓

Replicate

↓

Fal

↓

Topaz
```

Now

Adding new provider

↓

Create new class

No existing code changes.

---

YOUR PROJECT

Exactly this.

You added

Replicate

↓

Fal

↓

Topaz

without changing frontend.

Excellent OCP example.

---

Interview Answer

> Open Closed Principle means software should allow new functionality to be added without modifying existing code.

---

# L — Liskov Substitution Principle

Definition

A child class should be replaceable for its parent.

Example

```text
AIProvider

↓

Replicate

↓

Fal

↓

Topaz
```

Every provider

should support

```python
generate_image()
```

FastAPI

doesn't care

which provider.

Exactly your architecture.

---

Interview Answer

> Any subclass should be usable wherever its parent class is expected without changing the correctness of the program.

---

# I — Interface Segregation Principle

Don't force classes

to implement

methods they don't need.

Bad

```text
Provider

↓

Generate

Upscale

Delete

Translate

Video

Audio
```

Replicate

doesn't support

Video.

Why implement?

Bad interface.

---

Better

```text
ImageGeneration Interface

Upscale Interface

Editing Interface
```

Separate interfaces.

---

# D — Dependency Inversion Principle

High-level modules

shouldn't depend

on low-level modules.

Bad

```python
FastAPI

↓

Replicate API
```

Good

```text
FastAPI

↓

Provider Interface

↓

Replicate

↓

Fal

↓

Topaz
```

FastAPI

depends on abstraction.

Not implementation.

Exactly your project.

---

# SOLID Summary

| Principle | Meaning                  |
| --------- | ------------------------ |
| SRP       | One Responsibility       |
| OCP       | Extend without modifying |
| LSP       | Child replaces Parent    |
| ISP       | Small Interfaces         |
| DIP       | Depend on Abstraction    |

---

# Part 2 — Design Patterns

---

# Factory Pattern

Problem

Need different providers.

```text
Replicate

Fal

Topaz
```

Don't do

```python
if provider=="replicate":
...

elif provider=="fal":
...
```

Instead

Factory

returns correct object.

```text
ProviderFactory

↓

Replicate

Fal

Topaz
```

---

YOUR PROJECT

Provider Router

acts similar to Factory.

---

Interview Answer

Factory Pattern creates objects without exposing object creation logic to the client.

---

# Strategy Pattern ⭐⭐⭐⭐⭐

One of the most asked.

Suppose

Three providers

```text
Replicate

Fal

Topaz
```

All generate image.

Different implementation.

```python
provider.generate()
```

Backend doesn't know

which provider.

Switch anytime.

Exactly your project.

---

YOUR PROJECT

Best real-world example.

---

Interview Answer

Strategy Pattern allows selecting an algorithm at runtime by encapsulating different implementations behind a common interface.

---

# Singleton Pattern

Need

One Database Connection.

Not

1000 database connections.

Singleton

```text
Application

↓

One Database Instance
```

Used in

* Database
* Logger
* Configuration

---

Interview Question

Where have you seen Singleton?

Database Connection.

NestJS Providers.

Configuration.

Logger.

---

# Observer Pattern

One event

↓

Many listeners.

Example

Image Generated.

Need

* Email
* Analytics
* Billing
* Notification

Instead

```text
Image Generated

↓

Observer

↓

Email

Analytics

Notification
```

---

Exactly similar

to RabbitMQ event architecture.

---

# Repository Pattern ⭐⭐⭐⭐⭐

Most important backend pattern.

Instead of

FastAPI

↓

SQL

Directly

Use

```text
Controller

↓

Service

↓

Repository

↓

Postgres
```

Repository

contains

database queries.

Business logic

stays

inside Service.

---

YOUR PROJECT

NestJS

TypeORM Repository.

Exactly this.

---

Interview Answer

Repository Pattern separates business logic from database access, making the application easier to test and maintain.

---

# Dependency Injection (DI)

Extremely important.

NestJS

uses it everywhere.

Instead of

```python
service = UserService()
```

Framework provides it.

```text
Controller

↓

Inject Service

↓

Inject Repository

↓

Database
```

Loose coupling.

Easy testing.

---

Interview Question

Why Dependency Injection?

Answer

Dependency Injection reduces coupling between classes by providing dependencies from outside instead of creating them internally. It also improves testability and maintainability.

---

# MVC Pattern

Classic architecture.

```text
Controller

↓

Service

↓

Repository

↓

Database
```

Controller

Receives Request.

Service

Business Logic.

Repository

Database.

---

YOUR NestJS Project

Exactly MVC.

---

# Clean Architecture

Large applications.

```text
Controller

↓

Use Cases

↓

Domain

↓

Repository

↓

Database
```

Business logic

never depends

on database.

---

# Design Pattern Mapping to YOUR Resume

| Resume Project      | Pattern              |
| ------------------- | -------------------- |
| Provider Router     | Strategy + Factory   |
| NestJS Services     | Dependency Injection |
| TypeORM             | Repository           |
| JWT Auth            | Singleton (Config)   |
| FastAPI Services    | MVC                  |
| AI Provider Wrapper | Adapter + Strategy   |

---

# Adapter Pattern ⭐⭐⭐⭐⭐

One of your strongest examples.

Replicate returns

```json
{
 "prediction":
 ...
}
```

Fal returns

```json
{
 "output":
 ...
}
```

Topaz returns

```json
{
 "image":
 ...
}
```

Frontend wants

```json
{
 "image_url":"..."
}
```

You created

Wrapper.

This is

Adapter Pattern.

---

Interview Answer

Adapter Pattern converts one interface into another so that incompatible systems can work together.

---

# Which Pattern Did YOU Actually Use?

Provider Router

↓

Strategy

Provider Wrapper

↓

Adapter

NestJS

↓

Dependency Injection

Repository

↓

Repository Pattern

Configuration

↓

Singleton

---

# Real Interview Questions

---

### Explain SOLID.

Answer

Five principles that improve maintainability, scalability, and flexibility of object-oriented software.

---

### Which Design Pattern did you use?

> In my AI image generation project, the Provider Router follows the Strategy Pattern because different providers implement the same image generation operation. I also used an Adapter-like wrapper to normalize responses from different AI providers into a common format. In my NestJS project, I used Dependency Injection and the Repository Pattern through TypeORM.

---

### What is Dependency Injection?

Framework creates and injects dependencies instead of classes creating them themselves.

---

### Why Repository Pattern?

Keeps database code separate from business logic.

---

### Factory vs Strategy?

| Factory         | Strategy            |
| --------------- | ------------------- |
| Creates objects | Chooses behavior    |
| Object creation | Algorithm selection |

---

### Adapter vs Strategy?

| Adapter             | Strategy            |
| ------------------- | ------------------- |
| Converts interfaces | Switches algorithms |
| Compatibility       | Flexibility         |

Your wrapper is an Adapter.

Your provider selection is Strategy.

---

### Singleton Example?

Database connection.

Configuration.

Logger.

---

# ⭐ Senior-Level Interview Answer

**Interviewer:** *Which design patterns have you used?*

A strong answer:

> "In my AI image generation backend, I used a Strategy-like approach where different AI providers such as Replicate, Fal AI, and Topaz exposed a common interface, allowing the application to switch providers without changing the business logic. I also implemented an Adapter-like wrapper to normalize different provider responses into a common response format for the frontend. In my NestJS projects, I used Dependency Injection extensively, and TypeORM followed the Repository Pattern to separate data access from business logic."

This answer directly connects design patterns to your actual experience and is much stronger than simply defining the patterns.

---

# 📌 Volume 16 Summary

After this volume, you should confidently understand:

* SOLID principles
* Single Responsibility Principle (SRP)
* Open/Closed Principle (OCP)
* Liskov Substitution Principle (LSP)
* Interface Segregation Principle (ISP)
* Dependency Inversion Principle (DIP)
* Strategy Pattern
* Factory Pattern
* Adapter Pattern
* Repository Pattern
* Dependency Injection
* Singleton Pattern
* Observer Pattern
* MVC
* Clean Architecture
* Most importantly, how these patterns apply to **your own FastAPI, NestJS, and AI projects**, which is what interviewers care about most.

---

## 🎯 Next Volume (Volume 17)

We'll move into **Backend Performance & Database Optimization**, covering:

* Database Indexing
* Query Optimization
* N+1 Query Problem
* Pagination (Offset vs Cursor)
* Connection Pooling
* Lazy vs Eager Loading
* API performance optimization
* Reducing latency in real backend systems

These topics are very common in interviews for backend engineers with 2–5 years of experience and tie directly into the scalable architectures we've covered so far.
