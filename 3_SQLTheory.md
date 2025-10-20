# Phase 3: SQL Theory

## 1. What is SQL
SQL stands for Structured Query Language.  
It is used to communicate with relational databases.  
SQL allows creating, reading, updating, and deleting data — called CRUD operations.

| Operation | SQL Command | Example Meaning |
|------------|--------------|----------------|
| Create | INSERT | Add new data |
| Read | SELECT | Retrieve data |
| Update | UPDATE | Modify data |
| Delete | DELETE | Remove data |

Example:  
`SELECT * FROM Patients WHERE Age > 60;` — Retrieves all patients older than 60.

Interview Question:  
Q: What is SQL?  
A: SQL is a language used to interact with relational databases — to create, read, update, and delete data.

---

## 2. Types of SQL Commands

| Type | Full Form | Purpose | Example |
|------|-------------|----------|----------|
| DDL | Data Definition Language | Defines structure | CREATE, ALTER, DROP |
| DML | Data Manipulation Language | Manages data | INSERT, UPDATE, DELETE |
| DCL | Data Control Language | Controls access | GRANT, REVOKE |
| TCL | Transaction Control Language | Manages transactions | COMMIT, ROLLBACK, SAVEPOINT |
| DQL | Data Query Language | Fetches data | SELECT |

Interview Question:  
Q: What is the difference between DDL and DML?  
A: DDL changes the structure of the database, while DML changes the data within it.

---

## 3. SQL Clauses

| Clause | Purpose | Example |
|---------|----------|----------|
| WHERE | Filters rows | WHERE Age > 60 |
| ORDER BY | Sorts results | ORDER BY Name ASC |
| GROUP BY | Groups data | GROUP BY Department |
| HAVING | Filters grouped data | HAVING COUNT(*) > 5 |
| LIMIT | Restricts number of rows | LIMIT 10 |

Example:  
`SELECT Doctor, COUNT(*) FROM Visits GROUP BY Doctor HAVING COUNT(*) > 10;`

Interview Question:  
Q: What is the difference between WHERE and HAVING?  
A: WHERE filters rows before grouping, HAVING filters after grouping.

---

## 4. SQL Joins

| Type | Description | Example |
|------|--------------|----------|
| INNER JOIN | Returns matching rows | Patients with matching Visits |
| LEFT JOIN | All left + matched right | All Patients even if no Visit |
| RIGHT JOIN | All right + matched left | All Visits even if no Patient |
| FULL JOIN | All rows from both sides | Combines all data |

Example:  
`SELECT Patients.Name, Visits.Visit_Date FROM Patients INNER JOIN Visits ON Patients.Patient_ID = Visits.Patient_ID;`

Interview Question:  
Q: What is the difference between INNER JOIN and LEFT JOIN?  
A: INNER JOIN shows only matches, LEFT JOIN keeps all from the left table.

---

## 5. Subqueries

A subquery is a query inside another query.

Example:  
`SELECT Name FROM Patients WHERE Patient_ID IN (SELECT Patient_ID FROM Visits WHERE Visit_Date > '2024-01-01');`

Interview Question:  
Q: What is a subquery?  
A: A query inside another query used to filter or calculate based on another result set.

---

## 6. Views

A view is a virtual table based on a stored query.

Example:  
`CREATE VIEW Active_Patients AS SELECT * FROM Patients WHERE Status = 'Active';`

Interview Question:  
Q: What is a view?  
A: A saved query that acts like a virtual table.

---

## 7. Indexes

Indexes make searching faster by creating a data structure that helps locate data quickly.

Example:  
`CREATE INDEX idx_patient_name ON Patients(Name);`

Interview Question:  
Q: What is the purpose of an index?  
A: To speed up search and retrieval operations.

---

## 8. Transactions and ACID Properties

A **transaction** is a group of one or more SQL operations that are treated as one complete action.  
If any step fails, the entire transaction is rolled back.

### Transaction Flow
1. Begin Transaction  
2. Perform Operations  
3. Check if all succeeded  
4. Commit if all good, otherwise Rollback

Example:
```
BEGIN TRANSACTION;

INSERT INTO Patients (ID, Name) VALUES (1, 'John');
INSERT INTO Visits (Visit_ID, Patient_ID, Date) VALUES (101, 1, '2025-10-20');

IF both succeed:
   COMMIT;
ELSE:
   ROLLBACK;
```

### How it works in real life
If a nurse adds a patient and their vitals together, both should succeed.  
If vitals fail to save, patient data also rolls back — no half-saved data remains.

---

## 9. ACID Properties

| Property | Meaning | Example |
|-----------|----------|----------|
| Atomicity | All or none | Patient and Visit saved together or not at all |
| Consistency | Data stays valid | No Visit for a non-existent Patient |
| Isolation | Transactions don’t affect each other | Two nurses adding data at once don’t clash |
| Durability | Once saved, stays saved | Data remains after restart |

### Detailed Explanation

**Atomicity** – “All or Nothing”  
If one step fails, the entire transaction fails.  
Example: Add Patient + Visit. If Visit fails, both are undone.

**Consistency** – “Data Rules Stay Correct”  
All constraints remain valid.  
Example: Cannot add Visit for missing Patient (foreign key rule).

**Isolation** – “No Interference Between Transactions”  
Multiple transactions can run without affecting each other.  
Example: Two receptionists booking at once don’t overwrite each other.

**Durability** – “Saved Permanently”  
Once committed, data is written to disk and will remain even after crash.  
Example: After COMMIT, a patient record stays saved even if power fails.

### How Database Handles Rollback or Crash
Databases maintain a transaction log (temporary record of all actions).  
If a crash happens before COMMIT, the database reads this log and rolls back incomplete actions automatically.  
If COMMIT has occurred, the transaction remains safe even after restart.

### Simple Interview Answer
> A transaction ensures reliability by using ACID properties:  
> - Atomicity: All or none  
> - Consistency: Keeps rules valid  
> - Isolation: No interference  
> - Durability: Saved permanently even after a crash

---

## 10. Constraints in SQL

| Constraint | Example |
|-------------|----------|
| PRIMARY KEY | CREATE TABLE Patients (ID INT PRIMARY KEY, Name VARCHAR(50)); |
| FOREIGN KEY | FOREIGN KEY (Doctor_ID) REFERENCES Doctors(ID) |
| UNIQUE | UNIQUE (Email) |
| CHECK | CHECK (Age > 0) |
| DEFAULT | DEFAULT 'Active' |

Interview Question:  
Q: Why do we use constraints?  
A: To maintain accuracy and prevent invalid entries.

---

## 11. SQL Aggregate Functions

| Function | Purpose | Example |
|-----------|----------|----------|
| COUNT() | Counts rows | COUNT(*) |
| SUM() | Adds numbers | SUM(Fee) |
| AVG() | Averages values | AVG(Age) |
| MAX() | Finds largest | MAX(HeartRate) |
| MIN() | Finds smallest | MIN(Age) |

Example:  
`SELECT Doctor_ID, COUNT(*) FROM Visits GROUP BY Doctor_ID;`

Interview Question:  
Q: What is the use of GROUP BY?  
A: To group rows based on a column and apply functions like COUNT or SUM.

---

## 12. SQL vs NoSQL Quick Recap

| Feature | SQL | NoSQL |
|----------|-----|--------|
| Structure | Tables | Documents, key-value |
| Query | SQL | Database-specific |
| Scaling | Vertical | Horizontal |
| Schema | Fixed | Flexible |
| Examples | MySQL, PostgreSQL | MongoDB, Redis |

Interview Question:  
Q: Why is SQL still widely used?  
A: It offers strong consistency and reliability for structured data.

---

## 13. Common Interview Questions (Phase 3)

1. What is SQL?  
2. What are DDL and DML commands?  
3. What is a transaction?  
4. What are ACID properties?  
5. What is the difference between WHERE and HAVING?  
6. What is an index?  
7. What is a view?  
8. What is the purpose of JOIN?  
9. What is the difference between SQL and NoSQL?  
10. What are constraints in SQL?

---

End of Phase 3 Notes
