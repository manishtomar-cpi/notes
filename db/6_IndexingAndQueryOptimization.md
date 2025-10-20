# Phase 6: Indexing and Query Optimization

## 1. What is an Index?
An index in a database is like an index in a book — it helps the system find data faster without checking every record.

When you search for a patient named “John” in a large hospital database, the system doesn’t want to look through every patient row.  
Instead, it uses an index (a sorted list of key fields) to jump directly to “John.”

Indexes are built on columns that are frequently searched or filtered — like `patient_id`, `name`, or `device_id`.

Simple explanation:
- Without an index → The database scans every row (called a full table scan).
- With an index → The database jumps directly to matching rows.

Interview Question:  
Q: What is an index in a database?  
A: An index is a data structure that helps the database locate and retrieve records faster without scanning the entire table.

---

## 2. How Indexes Work (B-tree, Hash, Bitmap)
Indexes can be implemented using different data structures.  
Let’s understand the common ones simply 👇

### a) B-Tree Index (Balanced Tree)
- The most common type of index (used in MySQL, PostgreSQL).  
- Works like a sorted tree — each node stores keys in sorted order.  
- The database traverses the tree to find data quickly.

Example:  
If you search for `patient_id = 120`, the B-tree index goes through:
- Root → Middle nodes → Leaf (patient_id 120 found)

Good for range queries (e.g., patients aged 20–40)  
Slightly slower for exact lookups compared to hash.

---

### b) Hash Index
- Uses a hash table (like a dictionary in code).  
- Maps keys to specific memory locations.  
- Very fast for exact matches but not for ranges.

Example:  
Searching for `device_id = D101` → the hash instantly points to where D101 is stored.  
But you can’t easily ask “device_id between D100 and D200.”

Very fast for equality (=) lookups  
Not suitable for range or sorting queries

---

### c) Bitmap Index
- Uses bits (0s and 1s) to represent whether a value exists.  
- Works well for columns with few unique values.

Example:  
A column called `gender` with values `M` or `F`.  
You can store it as:
```
M → 1 0 0 1 1
F → 0 1 1 0 0
```
Great for analytics and filtering large data with limited unique values.  
 Not suitable for frequent updates.

---

## 3. Clustered vs Non-Clustered Indexes

| Type | How it works | Example | Pros | Cons |
|------|---------------|----------|------|------|
| Clustered Index | The actual table data is stored in the order of the index | Patient records sorted by `patient_id` | Fast retrieval for that column | Only one clustered index per table |
| Non-Clustered Index | Separate structure pointing to data location | Index on `name` pointing to `patient_id` | Can have multiple indexes | Slightly slower because it needs an extra lookup |

Example in healthcare:  
- Clustered index: Patient data stored by `patient_id`.  
- Non-clustered index: Extra index on `name` to quickly find patients by name.

Interview Question:  
Q: Difference between clustered and non-clustered indexes?  
A: A clustered index defines how data is physically stored; a non-clustered index is a separate lookup structure that points to the data.

---

## 4. Indexing Strategies and Pitfalls

Good practices:
- Index columns used in `WHERE`, `JOIN`, or `ORDER BY` clauses.
- Use indexes on frequently searched fields (like `patient_id`, `device_id`).
- Keep indexes small — avoid indexing every column.
- Use composite indexes (multiple columns) when queries often filter by multiple fields.

Pitfalls (things to avoid):
- Too many indexes slow down `INSERT` and `UPDATE` operations because each index must also be updated.
- Indexing columns with many unique values (like timestamps) can reduce performance.
- Indexing rarely-used columns wastes memory.

Interview Question:  
Q: What are common indexing mistakes?  
A: Creating too many indexes, indexing high-cardinality columns, or indexing columns rarely used in queries.

---

## 5. Query Execution Plans
When you run a SQL query, the database uses a query optimizer to decide the fastest way to execute it.  
You can view this plan using commands like `EXPLAIN` or `EXPLAIN ANALYZE`.

The plan shows:
- Which indexes were used
- How tables were joined
- Whether a full table scan happened

Example:  
If you query:
```sql
SELECT * FROM Patient WHERE age > 60;
```
The optimizer decides:
- Use index on `age` (if exists)
- Or scan entire table (if no index)

In an interview, you can say:  
“Query execution plans help developers understand how the database retrieves data and whether indexes are being used efficiently.”

---

## 6. Caching and Query Optimization

Caching:  
Databases or applications can temporarily store results in memory.  
So, if the same query runs again, it retrieves results instantly.

Example:  
If the nurse dashboard repeatedly fetches “patients admitted today,” caching avoids repeated database work.

Query Optimization Techniques:
- Select only required columns (avoid `SELECT *`)
- Use appropriate indexes
- Avoid unnecessary subqueries or joins
- Use pagination (`LIMIT`) for large result sets
- Analyze query execution plan to detect bottlenecks

---

## 7. Summary Table

| Concept | Purpose | Example |
|----------|----------|----------|
| B-Tree Index | Balanced search tree | Range queries like `age BETWEEN 20 AND 40` |
| Hash Index | Fast exact lookup | `device_id = D101` |
| Bitmap Index | Efficient filtering on few values | `gender = 'F'` |
| Clustered Index | Defines physical data order | `patient_id` |
| Non-Clustered Index | Secondary lookup | `name`, `city` |
| Caching | Store recent query results | Dashboard refreshes |
| Query Plan | Execution path | Use `EXPLAIN` in SQL |

---

## 8. Common Interview Questions (with short answers)

1. What is an index?  
   An index is a data structure that improves data retrieval speed by avoiding full table scans.

2. What are common types of indexes?  
   B-Tree, Hash, and Bitmap indexes.

3. Difference between clustered and non-clustered indexes?  
   Clustered defines physical data order; non-clustered is a separate lookup structure.

4. What is a query execution plan?  
   It’s the database’s internal plan showing how it executes a query and which indexes are used.

5. Why not index every column?  
   Because too many indexes slow down write operations and consume extra memory.

6. What is caching in databases?  
   Storing query results temporarily to speed up repeated queries.

7. How can you optimize slow queries?  
   Use indexes wisely, cache results, reduce `SELECT *`, and check query plans.

8. When should you use a hash index?  
   For exact-match queries (e.g., `id = 101`), not for range queries.

9. When should you use a bitmap index?  
   For columns with few distinct values like gender or yes/no flags.

10. What happens if you index too many columns?  
   It increases storage use and slows down insert/update operations.

---

End of Phase 6 Notes
