Perfect. This is the volume that I think has the **highest probability** of being asked by **GATP Solutions** because they are hiring for an **Automation Developer**.

---

# 📘 VOLUME 3 – AUTOMATION DEVELOPER (GATP INTERVIEW)

This volume is based on real Automation Developer interviews.

---

# CHAPTER 1 – File Automation

## Q1. How do you read a CSV file in Python?

### Answer

The most common way is using the `csv` module or `pandas`.

Simple example:

```python
import csv

with open("employees.csv") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row["name"])
```

### Interview Explanation

> I would use `csv.DictReader` because it allows me to access each column using its name instead of remembering indexes.

---

## Q2. When would you use Pandas instead?

### Answer

When

* large datasets
* filtering
* grouping
* cleaning
* joining files
* exporting reports

Example

```python
import pandas as pd

df = pd.read_csv("employees.csv")
print(df.head())
```

---

# Q3. Difference between CSV and Excel?

| CSV                 | Excel          |
| ------------------- | -------------- |
| Plain text          | Spreadsheet    |
| Faster              | Slower         |
| No formatting       | Formatting     |
| Small size          | Bigger         |
| Automation friendly | Human friendly |

---

# Q4. Suppose you receive 1000 CSV files daily.

How would you automate it?

### Answer

My pipeline would be:

```
Watch Folder

↓

Read File

↓

Validate

↓

Process Data

↓

Call APIs

↓

Store Results

↓

Move file to Processed Folder

↓

Generate Logs
```

---

# Q5. How do you rename multiple files?

```python
import os

for file in os.listdir():

    os.rename(file,new_name)
```

---

# Q6. How do you move files?

```python
import shutil

shutil.move(source,destination)
```

---

# Q7. How do you delete files?

```python
import os

os.remove(file)
```

---

# Q8. How do you process 10000 files?

### Answer

I would

* Process one file at a time instead of loading all files into memory.
* Use batch processing.
* Use parallel processing if tasks are independent.
* Log failures separately.
* Continue processing even if one file fails.

---

# CHAPTER 2 – API Automation

---

## Q9. How do you call an API?

```python
import requests

response=requests.get(url)
```

---

## Q10. POST request?

```python
requests.post(url,json=data)
```

---

## Q11. What are Headers?

### Answer

Headers contain metadata.

Example

Authorization

Content-Type

Accept

User-Agent

---

## Q12. What is Bearer Token?

```
Authorization

Bearer eyJ.....
```

Used for JWT authentication.

---

## Q13. How do you authenticate API?

Answer

Usually

* JWT
* OAuth
* API Key
* Basic Authentication

In my project

Firebase JWT.

---

# Q14. API returns 500.

What will you do?

Answer

* Retry
* Exponential Backoff
* Log
* Fallback Provider
* Notify User

Exactly what you did in BlueDigit.

---

# CHAPTER 3 – Retry

---

## Q15. Why Retry?

Temporary errors.

Network.

Timeout.

Server Busy.

---

## Q16. How many retries?

Three.

---

## Q17. Why not 100?

Because

* User waits longer.
* Server load increases.
* Usually temporary failures recover quickly.

---

## Q18. Exponential Backoff?

Wait times

```
1 second

↓

2 seconds

↓

4 seconds
```

instead of

```
Retry

Retry

Retry
```

---

# CHAPTER 4 – Logging

---

## Q19. Why Logging?

Because production systems cannot be debugged using print statements.

---

## Q20. What should be logged?

* Timestamp
* User
* API
* Error
* Request ID
* Response Time

---

## Q21. Logging Levels

DEBUG

INFO

WARNING

ERROR

CRITICAL

---

# CHAPTER 5 – Scheduling

---

## Q22. How do you schedule automation?

Linux

Cron

Windows

Task Scheduler

Python

APScheduler

Celery Beat

---

## Q23. What is Cron?

Cron executes scripts at fixed times.

Example

Every day

2 AM

---

# CHAPTER 6 – Error Handling

---

## Q24. Suppose one file fails.

Should entire automation stop?

No.

Continue.

Move failed file.

Generate log.

Retry later.

---

## Q25. Suppose API fails.

Answer

Retry.

If still fails

Log

Move to Dead Letter Queue (if available)

Notify

---

# CHAPTER 7 – Database

---

## Q26. Why store automation logs?

Audit.

Debugging.

Reporting.

Recovery.

---

## Q27. Suppose duplicate records arrive.

Answer

Check

Unique ID

Email

Hash

Skip duplicate.

---

# CHAPTER 8 – Real Automation Scenarios

---

## Q28.

Every day

500 PDFs arrive.

How will you automate?

Answer

```
Folder

↓

Read PDF

↓

Extract Text

↓

Validate

↓

Call AI/API

↓

Store Database

↓

Archive PDF
```

---

## Q29.

100 Excel files arrive.

Answer

```
Read Excel

↓

Validate

↓

Transform

↓

Insert Database

↓

Move Completed
```

---

## Q30.

Suppose one Excel file is corrupted.

Answer

Skip.

Log.

Continue.

Notify.

---

# CHAPTER 9 – API + Database

---

## Q31.

Read CSV

Call API

Store Database

Draw architecture.

```
CSV

↓

Python Script

↓

Validation

↓

API

↓

JSON Response

↓

PostgreSQL

↓

Logs
```

---

## Q32.

Suppose API takes 30 seconds.

Answer

Don't block the whole application.

Use asynchronous requests or background workers so other tasks can continue.

---

# CHAPTER 10 – Python Questions

---

## Q33.

Difference

append()

extend()

Already covered.

---

## Q34.

Difference

List

Tuple

Already covered.

---

## Q35.

Difference

is

==

```python
a=[1]

b=[1]

a==b
```

True

Because values are equal.

---

```python
a is b
```

False

Because objects are different.

---

## Q36.

Difference

deepcopy()

copy()

Interview favourite.

### Answer

**Shallow Copy (`copy.copy`)**

* Creates a new outer object.
* Nested objects are still shared.

**Deep Copy (`copy.deepcopy`)**

* Creates a completely independent copy, including nested objects.

---

## Q37.

Difference

Thread

Process

Async

| Thread        | Process                  | Async                              |
| ------------- | ------------------------ | ---------------------------------- |
| Shared memory | Separate memory          | Single thread with event loop      |
| Lightweight   | Heavyweight              | Efficient for I/O                  |
| Good for I/O  | Good for CPU-bound tasks | Best for many concurrent I/O tasks |

---

## Q38.

When would you use multiprocessing?

Answer

For CPU-intensive tasks like:

* Image processing
* Large mathematical computations
* ML training
* Video processing

---

## Q39.

When would you use async?

Answer

For I/O-bound operations like:

* API calls
* Database queries
* File uploads/downloads
* Waiting for AI provider responses

This directly matches your FastAPI experience.

---

## Q40.

Biggest automation project?

### Answer

> At BlueDigit Technologies, I automated the AI image generation pipeline. The backend accepted requests, routed them to the appropriate AI provider, retried on transient failures, stored generated images in S3, saved metadata in PostgreSQL, and returned the processed results to users. This involved integrating multiple external services and handling long-running asynchronous operations.

---

# ⭐ Scenario-Based Questions (Very Likely)

### Q41. How would you design a folder watcher?

**Answer:**

* Continuously monitor an input directory (or use a file system watcher like `watchdog`).
* Detect newly added files.
* Validate file type and size.
* Process the file.
* Move successful files to a `processed` folder.
* Move failed files to an `error` folder.
* Log every step.

---

### Q42. How would you ensure the same file isn't processed twice?

**Answer:**

* Maintain a processed file log or database table.
* Check a unique file name, checksum (hash), or file ID before processing.
* Skip duplicates and log them.

---

### Q43. If your automation crashes midway, how would you recover?

**Answer:**

* Save processing status in a database or checkpoint file.
* Restart from the last successful file instead of processing everything again.
* Retry only failed or pending jobs.

---

## ⭐ Final Advice for Tomorrow

If you're asked an automation scenario, structure your answer like this:

1. **Input** (Where does the data come from?)
2. **Validation** (How do you ensure it's correct?)
3. **Processing** (What business logic runs?)
4. **Error Handling** (Retries, logging, fallback)
5. **Storage** (Database, S3, file system)
6. **Monitoring** (Logs, alerts, metrics)

This structured approach demonstrates engineering thinking and is often more impressive than immediately jumping into code.

At this point, you're well prepared for a typical Automation Developer screening. If time allows after this, the next highest-value topic would be **SQL interview questions with answers**, followed by a **mock interview** that simulates the actual conversation.
