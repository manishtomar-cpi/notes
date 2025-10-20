# Phase 7: Transactions, Concurrency & Consistency

## 1. Transaction Lifecycle

A transaction is a group of one or more operations that must either all succeed or all fail together — nothing in between.

In healthcare, if you update both a patient’s record and billing information:
- Both updates must complete successfully (commit)
- Or both must be undone (rollback)

That’s why databases use transactions — to keep data consistent.

Transaction lifecycle:

| Step | Description |
|------|--------------|
| Begin Transaction | Start the transaction |
| Execute | Perform SQL statements (INSERT, UPDATE, DELETE) |
| Commit | Save all changes permanently |
| Rollback | Undo all changes if any error occurs |

Example:
```sql
BEGIN TRANSACTION;
UPDATE Patient SET balance = balance - 500 WHERE patient_id = 101;
INSERT INTO Billing (patient_id, amount) VALUES (101, 500);
COMMIT;
```
If the second query fails, the system rolls back the first change to keep data safe.

Interview Question:  
Q: What is a database transaction?  
A: A transaction is a unit of work that executes multiple operations as one atomic action — either all succeed or none do.

---

## 2. Isolation Levels (Read Uncommitted → Serializable)

When multiple users (or systems) use the same database, they may access or modify the same data at the same time.  
Isolation levels define how visible one transaction’s changes are to others.

| Level | Description | Common Problem Prevented |
|--------|--------------|--------------------------|
| Read Uncommitted | Transactions can read uncommitted (dirty) data | None — can see dirty reads |
| Read Committed | Only committed data can be read | Prevents dirty reads |
| Repeatable Read | Keeps data stable during transaction | Prevents dirty + non-repeatable reads |
| Serializable | Fully isolated — transactions run as if one after another | Prevents all concurrency issues |

Example (Healthcare):
- Read Uncommitted: A nurse might see a patient’s test result that another doctor is still editing.  
- Serializable: Only one can change it at a time — safest but slowest.

Interview Question:  
Q: What are database isolation levels?  
A: They define how transaction visibility and concurrency are managed, balancing data accuracy and performance.

---

## 3. Two-Phase Commit Protocol (2PC)

Used in distributed databases where data is stored across multiple servers (for example, Hospital A and Hospital B each have part of the data).

Goal: Ensure either all sites commit or all rollback.

How it works:

| Phase | Description |
|--------|-------------|
| Phase 1 – Prepare | Coordinator asks all servers if they can commit |
| Phase 2 – Commit/Rollback | If all say “yes,” coordinator sends “commit”; if any says “no,” coordinator sends “rollback” |

Example:
- Hospital A updates patient info  
- Hospital B updates billing  
Both must succeed together — if B fails, A also rolls back.

 Ensures consistency across distributed systems  
 Slower due to waiting for confirmations

Interview Question:  
Q: What is the two-phase commit protocol?  
A: It’s a distributed transaction process where all participants first prepare, then either commit or rollback together.

---

## 4. Deadlock Prevention and Detection

A deadlock happens when two or more transactions are waiting for each other to release resources — and no one ever proceeds.

Example (Healthcare):
- Transaction 1 locks Patient table to update a record.  
- Transaction 2 locks Billing table.  
- Now T1 wants Billing, and T2 wants Patient. Both wait forever → deadlock.

Prevention Techniques:
- Always lock resources in the same order (e.g., always lock Patient before Billing).
- Use timeouts — if waiting too long, rollback automatically.

Detection Techniques:
- Database regularly checks for circular waits and stops one transaction.

Interview Question:  
Q: What is a deadlock and how do you handle it?  
A: A deadlock occurs when two transactions wait for each other’s locks. It’s prevented by using consistent locking order or resolved by detecting and rolling back one transaction.

---

## 5. Consistency Models in Distributed Systems

In distributed systems, data is stored in multiple locations.  
Consistency models define when and how updates made on one node become visible on others.

| Model | Description | Example |
|--------|--------------|----------|
| Strong Consistency | After an update, all users immediately see the same data | After updating heart rate, all dashboards instantly show the new value |
| Eventual Consistency | Updates spread gradually; data becomes consistent over time | A nurse’s dashboard may show old data briefly, then sync later |
| Causal Consistency | Related updates are seen in the right order | If a doctor adds a note and then approves it, no one sees approval before note |

In healthcare systems:  
- Strong consistency is needed for critical patient vitals.  
- Eventual consistency is acceptable for non-critical reports.

Interview Question:  
Q: What are consistency models in distributed systems?  
A: They define how quickly data changes are reflected across distributed servers — strong, eventual, or causal consistency.

---

## 6. Summary Table

| Concept | Purpose | Example |
|----------|----------|----------|
| Transaction | Group of operations executed as one | Update patient record and billing |
| Isolation Levels | Control data visibility between transactions | Prevent dirty reads |
| Two-Phase Commit | Ensure all nodes commit or rollback together | Multi-hospital database update |
| Deadlock | Transactions waiting on each other forever | One locks Patient, another locks Billing |
| Consistency Models | Define update visibility across systems | Strong, eventual, causal |

---

## 7. Common Interview Questions (with short answers)

1. What is a transaction?  
   A transaction is a set of operations executed as one atomic unit — either all succeed or none.

2. What are the four isolation levels?  
   Read Uncommitted, Read Committed, Repeatable Read, Serializable.

3. Which isolation level prevents dirty reads?  
   Read Committed.

4. What is the two-phase commit protocol?  
   It ensures distributed transactions commit or rollback consistently across all servers.

5. What is a deadlock?  
   A situation where two transactions wait indefinitely for each other’s resources.

6. How can you prevent deadlocks?  
   Lock resources in the same order and use timeouts.

7. What is the difference between strong and eventual consistency?  
   Strong consistency updates all nodes instantly; eventual consistency updates them over time.

8. What is causal consistency?  
   It ensures operations that are causally related are seen by everyone in the same order.

9. Why are isolation levels important?  
   They balance accuracy (consistency) and performance in multi-user systems.

10. Why is the two-phase commit slower?  
   Because all nodes must communicate and wait for each other’s response before committing.

---

End of Phase 7 Notes
