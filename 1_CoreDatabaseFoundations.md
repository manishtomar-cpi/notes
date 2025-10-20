# Phase 1: Core Database Foundations

## 1. What is a Database
A database is a place where information is stored in an organized way so it can be easily added, changed, searched, and retrieved.  
It helps multiple users or applications use the same data safely and efficiently.

Example:  
Think of a database like a digital filing cabinet. Each drawer (table) stores related files (records).

Interview Question:  
Q: Why do we need databases?  
A: To store and manage data efficiently, avoid duplication, and make data easily accessible for multiple users or applications.

---

## 2. What is a DBMS
DBMS means Database Management System.  
It is the software that allows users or applications to interact with the database by performing operations like creating, reading, updating, or deleting data.

Example:  
If the database is the cabinet, the DBMS is the person who organizes, adds, or removes files.

Interview Question:  
Q: What is the difference between a database and a DBMS?  
A: A database stores the data, and the DBMS manages how that data is stored, retrieved, and updated.

---

## 3. What is an RDBMS
RDBMS means Relational Database Management System.  
It stores data in tables with rows and columns. Tables are related to each other using keys.

Example:  
A “Patients” table can link to a “Visits” table using a common column like patient_id.

Interview Question:  
Q: What makes an RDBMS different from a DBMS?  
A: RDBMS organizes data in tables with relationships, while a normal DBMS may not support relationships.

---

## 4. SQL vs NoSQL Databases

| Feature | SQL Databases | NoSQL Databases |
|----------|----------------|----------------|
| Structure | Tables (rows and columns) | Collections, key-value pairs, or documents |
| Schema | Fixed | Flexible |
| Query Language | SQL | Varies by database |
| Scaling | Vertical (bigger machine) | Horizontal (more machines) |
| Examples | MySQL, PostgreSQL | MongoDB, Redis, Cassandra |

Interview Question:  
Q: When should you use NoSQL instead of SQL?  
A: When data is unstructured or semi-structured, schema changes often, or horizontal scaling is needed.

---

## 5. Data Models
A data model defines how data is stored and related inside a database.

1. Hierarchical Model – Tree-like structure (each record has one parent).  
2. Network Model – Each record can have many parents and children.  
3. Relational Model – Data stored in tables linked by keys.  
4. Object-Oriented Model – Data stored as objects like in programming.

Interview Question:  
Q: Which data model is most common today?  
A: The relational model because it is simple, flexible, and widely used.

---

## 6. OLTP vs OLAP

| Feature | OLTP (Online Transaction Processing) | OLAP (Online Analytical Processing) |
|----------|--------------------------------------|------------------------------------|
| Purpose | Daily transactions and live operations | Data analysis and reporting |
| Data Volume | Small, frequent updates | Large, historical data |
| Example | Hospital patient monitoring system | Health analytics dashboard |

Interview Question:  
Q: Difference between OLTP and OLAP?  
A: OLTP handles real-time data operations, while OLAP analyzes stored data to find trends or insights.

---

## 7. Important Terms
- Record or Row: One complete set of related data.  
- Field or Column: One attribute of data.  
- Schema: The plan or structure of the database (tables, columns, relationships).  
- Query: A request to read or modify data.  
- Metadata: Data about data, like column names and types.

Interview Question:  
Q: What is a schema?  
A: A schema defines how data is organized in the database — its tables, columns, and relationships.

---

## 8. Structured vs Unstructured Data

Structured data is organized and stored in a fixed format, usually in tables with rows and columns.  
Unstructured data has no fixed format and can vary in type and size.

Healthcare Example:  
- Structured Data: Patient vitals like heart rate, blood pressure, and SpO2 readings stored in a table.  
- Unstructured Data: ECG waveforms, medical images, or doctor notes in text or image format.

Interview Question:  
Q: What is the difference between structured and unstructured data?  
A: Structured data fits in tables (like vitals), while unstructured data does not (like ECG waveforms or images).

---

## 9. Vertical and Horizontal Scaling

Vertical Scaling (Scale Up):  
Add more power (CPU, RAM, storage) to a single server.  
Example: Upgrading your patient data server to handle more load.

Horizontal Scaling (Scale Out):  
Add more servers and divide the load among them.  
Example: Adding more MongoDB servers to handle wearable data from different hospitals.

Interview Question:  
Q: Which scaling method is better for large systems?  
A: Horizontal scaling because it allows adding more machines instead of depending on one large server.

---

## 10. Replication and Sharding

Replication:  
Making exact copies of a database on multiple servers.  
All copies have the same data.  
Used for reliability and faster reads.

Healthcare Example:  
A hospital database in London has replicas in Manchester and Glasgow. If one fails, others can serve data.

Sharding:  
Splitting one large database into smaller parts (shards), each holding a portion of the data.  
Used for scalability and better write performance.

Healthcare Example:  
Sharding data by patient region:  
Shard 1 - North India patients  
Shard 2 - South India patients  
Shard 3 - East India patients

Comparison Table:

| Feature | Replication | Sharding |
|----------|--------------|-----------|
| Data Copy | All servers have same data | Each server has different data |
| Goal | Reliability and read performance | Scalability and write performance |
| Example | Database copies for backup | Split data by patient region |

Interview Question:  
Q: Can replication and sharding be used in vertical scaling?  
A: No. Both need multiple servers. Vertical scaling uses a single machine.

---

## 11. Summary

| Concept | Simple Meaning | Healthcare Example |
|----------|----------------|--------------------|
| Structured Data | Organized, fixed format | Heart rate and BP readings |
| Unstructured Data | No fixed format | ECG waveforms, doctor notes |
| Vertical Scaling | Add power to one server | Upgrade main patient DB |
| Horizontal Scaling | Add more servers | Spread wearable data |
| OLTP | Real-time operations | Storing live patient vitals |
| OLAP | Data analysis | Research and reporting trends |
| Replication | Copies of database | Backup copies in other cities |
| Sharding | Split data | Divide by patient region |

---

## 12. Common Interview Questions and Answers (Phase 1)

1. What is a database?  
A database is an organized collection of data stored electronically.

2. What is a DBMS?  
It is software that helps manage, store, and retrieve data in a database.

3. What is an RDBMS?  
It is a DBMS that stores data in tables and supports relationships using keys.

4. Difference between SQL and NoSQL databases.  
SQL uses tables with a fixed schema; NoSQL stores data flexibly in documents or key-value pairs.

5. What is the difference between structured and unstructured data?  
Structured data fits into tables, unstructured data does not have a fixed format.

6. What is vertical and horizontal scaling?  
Vertical scaling adds resources to one server; horizontal scaling adds more servers.

7. What is replication?  
Replication means keeping multiple copies of the same database for reliability.

8. What is sharding?  
Sharding means splitting a database into smaller parts stored on different servers.

9. What is the difference between OLTP and OLAP?  
OLTP is for real-time data operations; OLAP is for analyzing large sets of historical data.

10. What is a schema?  
A schema defines how data is structured and related inside a database.

---

End of Phase 1 Notes
