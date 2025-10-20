# Phase 4: Advanced SQL Concepts

## 1. Stored Procedures
Stored procedures are saved sets of SQL statements stored in the database that can be reused and executed multiple times.
Example: `CREATE PROCEDURE GetTodayPatients AS SELECT * FROM Patients WHERE AdmissionDate = GETDATE();`
Interview Question: What is a stored procedure? A saved collection of SQL statements that can be executed whenever needed.

---

## 2. Functions
Functions are database routines that always return a single value.
Example: `CREATE FUNCTION GetAge(@BirthDate DATE) RETURNS INT AS RETURN DATEDIFF(YEAR, @BirthDate, GETDATE());`
Interview Question: What is the difference between a function and a procedure? A function always returns a value, a procedure may or may not.

---

## 3. Triggers
Triggers are automatic actions executed when an event (INSERT, UPDATE, DELETE) happens in a table.
Example: `CREATE TRIGGER BackupPatientRecord ON Patients AFTER UPDATE AS INSERT INTO Patient_Backup SELECT * FROM DELETED;`
Interview Question: What is a trigger? A trigger runs automatically when a specific database event occurs.

---

## 4. Views (Detailed)
Views are virtual tables created from SQL queries to simplify complex data or restrict access.
Example: `CREATE VIEW AdmittedPatients AS SELECT Name, RoomNumber FROM Patients WHERE Status = 'Admitted';`
Interview Question: What is the use of a view? It simplifies queries and enhances security by hiding underlying data.

---

## 5. Indexing (Detailed)
Indexes make searching and sorting data faster by storing reference structures.
Types: Clustered (changes physical order), Non-Clustered (separate structure).
Example: `CREATE INDEX idx_patient_name ON Patients(Name);`
Interview Question: Difference between clustered and non-clustered indexes? Clustered defines physical order, non-clustered points to data.

---

## 6. Advanced Joins
Self Join joins a table with itself. Cross Join combines all rows from two tables.
Example Self Join: `SELECT A.DoctorName, B.DoctorName AS ReferredBy FROM Doctors A JOIN Doctors B ON A.ReferrerID = B.DoctorID;`
Example Cross Join: `SELECT * FROM Doctors CROSS JOIN Patients;`
Interview Question: What is a self join? A join where a table is joined to itself to compare its rows.

---

## 7. Query Optimization
Optimization improves performance by reducing time and resources.
Tips: Use indexes, avoid SELECT *, filter early, use proper data types.
Example Good Query: `SELECT Name, Age FROM Patients WHERE Age > 60;`
Interview Question: How to optimize SQL queries? Use indexes, fetch limited columns, and avoid repeated subqueries.

---

## 8. Transactions with Error Handling
Transactions group operations and ensure all succeed or all fail.
Example:
```
BEGIN TRANSACTION;
BEGIN TRY
   INSERT INTO Patients VALUES (...);
   INSERT INTO Visits VALUES (...);
   COMMIT;
END TRY
BEGIN CATCH
   ROLLBACK;
END CATCH;
```
Interview Question: What happens if a transaction fails? The system rolls back all operations to keep data consistent.

---

## 9. One-Line Revision Table

| Feature | One-Line Explanation |
|----------|----------------------|
| Stored Procedure | Saved SQL code that can be reused. |
| Function | Returns one calculated value. |
| Trigger | Runs automatically on a data change. |
| View | Virtual table made from a query. |
| Index | Makes search faster using data pointers. |
| Self Join | A table joined with itself. |
| Cross Join | Combines all rows from two tables. |
| Query Optimization | Improves performance of SQL queries. |
| Transaction | Group of SQL statements acting as one. |

---

## 10. Common Interview Questions

1. What is a stored procedure and why use it?  
2. Difference between procedure and function.  
3. What is a trigger and how does it work?  
4. What is a view and why is it useful?  
5. What is an index? Difference between clustered and non-clustered?  
6. What is query optimization?  
7. How would you speed up a slow SQL query?  
8. What is a self join?  
9. What happens if a transaction fails?  
10. What are the advantages of stored procedures?

---

End of Phase 4 Notes
