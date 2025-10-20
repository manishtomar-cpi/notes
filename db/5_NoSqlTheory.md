# Phase 5: NoSQL Theory

## 1. What is NoSQL
NoSQL stands for Not Only SQL.
It is used for storing and managing unstructured or semi-structured data with flexible schema.
NoSQL is best for large-scale, rapidly changing, or real-time data such as wearable or IoT device data.

Interview Question: What is NoSQL?
Answer: NoSQL databases store and manage unstructured or semi-structured data and allow flexible schemas for fast scalability.

---

## 2. CAP Theorem

| Property | Meaning | Example |
|-----------|----------|----------|
| Consistency (C) | All users see the same data at the same time | A patient’s updated heart rate is visible to all nurses instantly |
| Availability (A) | The system always responds, even if some parts fail | A nurse can still view old data even if one server is down |
| Partition Tolerance (P) | The system continues to work even if communication between servers breaks | Devices in one hospital wing continue to function independently |

A distributed system cannot guarantee all three (C, A, P) simultaneously. It can only ensure any two.

| Type | Focus | Example Database |
|------|--------|------------------|
| CP | Consistency + Partition Tolerance | MongoDB |
| AP | Availability + Partition Tolerance | Cassandra |
| CA | Consistency + Availability | Single-server SQL systems |

Interview Question: What does CAP Theorem stand for?
Answer: CAP stands for Consistency, Availability, and Partition Tolerance. A distributed database can only guarantee two of the three properties at once.

---

## 3. BASE vs ACID Properties

| Property | SQL (ACID) | NoSQL (BASE) |
|-----------|-------------|--------------|
| A | Atomic – All or nothing | Basically Available – system always responds |
| C | Consistent – rules always followed | Soft state – data may change over time |
| I | Isolated – transactions don’t interfere | Eventually consistent – becomes consistent later |
| D | Durable – once saved, stays saved | - |

ACID = strict consistency, safe transactions.
BASE = high availability, flexibility, and eventual consistency.

Example:
SQL/ACID – Adding a patient and their visit both must succeed or fail together.
NoSQL/BASE – Patient vitals data may be slightly delayed but the system remains available.

Interview Question: What is the difference between ACID and BASE?
Answer: ACID ensures strict consistency and reliability, while BASE focuses on high availability and eventual consistency.

---

## 4. Types of NoSQL Databases

### a) Document-Based (MongoDB)
Stores data in JSON-like documents.
Each document can have different fields.
Example:
```
{
  "patient_id": "P001",
  "name": "John",
  "heart_rate": 88,
  "device_id": "D101"
}
```
When to use: Flexible data, IoT, or patient monitoring data.

Interview Question: What is a document-based database?
Answer: A database that stores data in JSON-like documents where each document can have a unique structure.

---

### b) Key-Value Databases (Redis)
Stores data as key-value pairs.
Example: `"device_D101_heart_rate" : "88"`
When to use: Fast lookups, caching, or session storage.

Interview Question: What is a key-value database?
Answer: It stores data as key and value pairs for quick retrieval and caching purposes.

---

### c) Columnar Databases (Cassandra)
Stores data by columns instead of rows.
Example Table:
```
Patient_ID | Time  | HeartRate | Temperature
P001       | 10:00 | 88        | 98.6
```
When to use: High write performance, time-series or analytical data.

Interview Question: What is a columnar NoSQL database used for?
Answer: It is used for handling large amounts of time-series or analytical data efficiently.

---

### d) Graph-Based Databases (Neo4j)
Stores data as nodes and relationships (edges).
Example:
Node1: Patient
Node2: Doctor
Edge: TreatedBy
When to use: For relationships like patient referrals or doctor networks.

Interview Question: What is a graph database?
Answer: It stores data as nodes and relationships, useful for connected or relational data.

---

## 5. Schema Flexibility and Scaling

Schema Flexibility: NoSQL databases do not require predefined schemas. You can add new fields anytime without changing old data.

Example: Earlier stored `{ name, age }` now can store `{ name, age, heart_rate }` without altering structure.

Scaling: NoSQL databases scale horizontally by adding more servers, ideal for IoT or wearable systems.

Interview Question: Why is NoSQL considered schema-flexible?
Answer: Because new fields can be added anytime without altering or breaking existing records.

---

## 6. When to Choose NoSQL over SQL

| Scenario | Best Choice |
|-----------|-------------|
| Large, unstructured, or fast-changing data | NoSQL |
| Strict consistency and relationships | SQL |
| Real-time analytics or flexible schemas | NoSQL |
| Fixed structure and joins | SQL |
| IoT or large-scale distributed systems | NoSQL |

Healthcare Example:
SQL for patient records and billing (structured data).
NoSQL for wearable sensor data (unstructured, streaming).

Interview Question: When should you use NoSQL instead of SQL?
Answer: When data is unstructured, rapidly changing, and scalability or speed is more important than strict consistency.

---

## 7. Summary Table

| NoSQL Type | Example | Structure | When to Use |
|-------------|----------|------------|--------------|
| Document | MongoDB | JSON-like documents | Flexible records like patient data |
| Key-Value | Redis | Key → Value pairs | Fast lookups or caching |
| Columnar | Cassandra | Columns | Analytics and time-series data |
| Graph | Neo4j | Nodes and relationships | Complex relationships and networks |

---

## 8. Why CAP Theorem Says a Distributed Database Can Only Guarantee Two of Three (C, A, P)

### What CAP Theorem Means
The CAP Theorem applies to distributed databases — systems where data is stored across multiple servers.  
It states that when a network failure occurs, a system can only guarantee **two** of the following three properties:

1. **Consistency (C)** – All users see the same data at the same time.  
2. **Availability (A)** – Every request gets a response, even if some servers fail.  
3. **Partition Tolerance (P)** – The system continues working even when servers cannot communicate with each other.

A system cannot provide all three at once during a failure.

---

### Example (Healthcare Scenario)
Imagine a patient monitoring system used by three hospitals (A, B, C) sharing patient data.

If the network connection between Hospital A and the others breaks (a partition happens),  
the database must choose between **Consistency** and **Availability**.

#### Option 1: CP – Consistency + Partition Tolerance
Hospital A temporarily stops updates until the connection is restored.  
All hospitals will always see the same, correct data, but A may not respond during the partition.

- Consistency maintained  
- Availability reduced  

**Example:** MongoDB focuses on CP.

#### Option 2: AP – Availability + Partition Tolerance
Hospital A continues accepting data during the disconnection.  
The system stays available, but A’s data may differ from B and C until syncing later.

- Availability maintained  
- Temporary inconsistency  

**Example:** Cassandra focuses on AP.

---

### Why You Can’t Have All Three
When a network partition (P) happens:
- If you keep **Consistency**, some nodes must stop responding (lose Availability).  
- If you keep **Availability**, some nodes may serve outdated data (lose Consistency).

So a system must choose either **CP** or **AP** — never all three simultaneously.

---

### Interview Summary
> CAP Theorem means that in a distributed system, only two of the three properties —  
> Consistency, Availability, and Partition Tolerance — can be fully guaranteed at one time.  
>  
> During network issues, you must choose between consistent data and always-on availability.


## 9. Common Interview Questions and Answers

1. What is NoSQL?  
Answer: NoSQL is a database type used for storing unstructured or semi-structured data with a flexible schema and horizontal scaling.

2. Explain CAP Theorem.  
Answer: CAP Theorem states that a distributed database can only guarantee two of three: Consistency, Availability, and Partition Tolerance.

3. Difference between ACID and BASE.  
Answer: ACID ensures strict consistency and safe transactions, while BASE allows flexibility, high availability, and eventual consistency.

4. Types of NoSQL databases.  
Answer: Document-based (MongoDB), Key-value (Redis), Columnar (Cassandra), Graph-based (Neo4j).

5. What is schema flexibility?  
Answer: It means NoSQL allows adding new fields anytime without changing or breaking existing data.

6. Difference between SQL and NoSQL.  
Answer: SQL uses fixed schemas and structured data; NoSQL uses flexible schemas and handles unstructured or dynamic data.

7. When should you use NoSQL?  
Answer: When handling large, unstructured, high-speed, or real-time data where scalability is essential.

8. Give one use case for each NoSQL type.  
Answer: Document - Patient records, Key-Value - Session cache, Columnar - Analytics, Graph - Doctor-patient network.

9. Which NoSQL type suits IoT or wearable data best?  
Answer: Document-based (MongoDB) because it can store flexible, JSON-like device readings efficiently.

10. Why can’t a distributed system have all three CAP properties?  
Answer: Because when network partitions occur, a system must choose between consistency and availability — it cannot guarantee both.

---

End of Phase 5 Notes
