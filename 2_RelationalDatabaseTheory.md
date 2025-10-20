# Phase 2: Relational Database Theory

## 1. What is a Relation or Table
A relation is a table in a relational database.  
It stores data in rows (records) and columns (fields).  
Each table represents one type of data, such as Patients, Doctors, or Visits.

Example:  
A “Patients” table might have columns for patient_id, name, age, and contact.

Interview Question:  
Q: What is a relation?  
A: A relation is a table that stores related data in rows and columns.

---

## 2. Keys in a Database

Keys uniquely identify rows and create relationships between tables.

### a. Primary Key
A column (or group of columns) that uniquely identifies each row.  
It cannot have duplicate or null values.  
Example: patient_id in the Patients table.

### b. Foreign Key
A column that links one table to another.  
Example: In the Visits table, patient_id is a foreign key referencing the Patients table.

### c. Candidate Key
Columns that could serve as a primary key.  
Only one is selected as the primary key.

### d. Composite Key
A key made up of two or more columns together.  
Example: (doctor_id, visit_date) in an appointment table.

### e. Surrogate Key
A system-generated key used when no natural key exists.  
Example: auto-increment patient number.

Interview Question:  
Q: What is the difference between a primary and foreign key?  
A: A primary key uniquely identifies rows in its own table, while a foreign key links two tables together.

---

## 3. Relationships Between Tables

Relationships show how data in different tables is connected.

| Relationship Type | Meaning | Example |
|-------------------|----------|----------|
| One-to-One | One row in Table A matches one row in Table B | Each patient has one medical record |
| One-to-Many | One row in Table A links to many rows in Table B | One patient can have many visits |
| Many-to-Many | Many rows in A relate to many in B | Many doctors treat many patients |

To handle many-to-many relationships, a separate table is created (junction table).

Interview Question:  
Q: How do you handle a many-to-many relationship?  
A: By creating a linking table that contains foreign keys from both related tables.

---

## 4. Normalization

Normalization means organizing data to remove duplication and maintain consistency.  
It divides large tables into smaller ones with clear relationships.

### Why Normalize
- Avoid data repetition.  
- Maintain accuracy and consistency.  
- Save storage space.  
- Make updates easier.

| Normal Form | Rule | Meaning |
|--------------|------|----------|
| 1NF | No repeating groups; each field holds one value | Each cell has one piece of data |
| 2NF | Be in 1NF; non-key columns depend on the whole key | Split partial dependencies |
| 3NF | Be in 2NF; no column depends on another non-key column | Remove indirect dependencies |
| BCNF | Stronger version of 3NF | Every determinant must be a candidate key |

Interview Question:  
Q: What is normalization?  
A: It is the process of organizing data into smaller related tables to remove duplication and improve integrity.

---

## 5. Denormalization

Denormalization means adding controlled redundancy to improve performance, especially for read-heavy systems.

Example:  
In a reporting database, storing doctor_name in the Visits table helps avoid multiple joins.

Interview Question:  
Q: Why denormalize a database?  
A: To make read operations faster when performance matters more than saving space.

---

## 6. Integrity Constraints

Constraints are rules to ensure data correctness and accuracy.

| Constraint | Description | Example |
|-------------|--------------|----------|
| PRIMARY KEY | Uniquely identifies each row | patient_id |
| FOREIGN KEY | Ensures linked data exists | visit.patient_id must exist in Patients |
| UNIQUE | Prevents duplicate values | patient_email |
| NOT NULL | Field must have a value | patient_name |
| CHECK | Limits allowed values | age > 0 |
| DEFAULT | Sets default values | created_at = current_time |

Interview Question:  
Q: What is referential integrity?  
A: It ensures that foreign keys always refer to valid rows in other tables.

---

## 7. Entity-Relationship (ER) Model

An ER Model visually represents entities (tables), their attributes (columns), and the relationships between them.

Healthcare Example:  
Entities – Patients, Doctors, Visits  
Relationships – Patients "have" Visits; Doctors "handle" Visits.

Interview Question:  
Q: What does an ER diagram represent?  
A: It shows entities, attributes, and relationships in a database.

---

## 8. Relational Operations (Conceptual)

Relational algebra defines how we can work with data in tables.

- SELECT – Choose specific rows based on conditions.  
- PROJECT – Choose specific columns.  
- JOIN – Combine related tables into one view.

Interview Question:  
Q: What is a join?  
A: A join combines data from two or more tables using a related column.

---

## 9. Advantages of Relational Databases
- Data accuracy and consistency.  
- Easy querying with SQL.  
- Clear relationships using keys and constraints.  
- Secure and well-structured.  
- Ideal for transactional systems.

Interview Question:  
Q: Why are relational databases popular?  
A: Because they maintain strong consistency and structured data integrity.

---

## 10. Limitations of Relational Databases
- Difficult to scale horizontally.  
- Performance decreases with very large datasets.  
- Not ideal for unstructured data like logs or images.

Interview Question:  
Q: When would you avoid a relational database?  
A: When working with large unstructured data or systems requiring flexible schema and scaling (use NoSQL instead).

---

## 11. Doubt Clarified: Normalization Example

We will use a simple patient visits example to understand normalization.

### Unnormalized Table (UNF)
| Patient_ID | Name | Doctor | Doctor_Specialty | Visit_Date | Medicine_List |
|-------------|------|---------|------------------|-------------|----------------|
| P001 | John | Dr. Smith | Orthopaedic | 10-Jan | Paracetamol, Ibuprofen |
| P002 | Mary | Dr. Jones | Cardiologist | 11-Jan | Aspirin |
| P001 | John | Dr. Smith | Orthopaedic | 12-Jan | Paracetamol |

Problems:
- Repeated doctor information.  
- Multiple medicines in one column.  
- Updates may cause inconsistency.

---

### First Normal Form (1NF)
Rule: Each field holds one value.

| Patient_ID | Name | Doctor | Doctor_Specialty | Visit_Date | Medicine |
|-------------|------|---------|------------------|-------------|-----------|
| P001 | John | Dr. Smith | Orthopaedic | 10-Jan | Paracetamol |
| P001 | John | Dr. Smith | Orthopaedic | 10-Jan | Ibuprofen |
| P002 | Mary | Dr. Jones | Cardiologist | 11-Jan | Aspirin |
| P001 | John | Dr. Smith | Orthopaedic | 12-Jan | Paracetamol |

---

### Second Normal Form (2NF)
Rule: Be in 1NF and all non-key attributes depend on the whole key.

We separate data into smaller tables.

Patients:
| Patient_ID | Name |
|-------------|------|
| P001 | John |
| P002 | Mary |

Doctors:
| Doctor_ID | Doctor_Name | Specialty |
|------------|--------------|------------|
| D001 | Dr. Smith | Orthopaedic |
| D002 | Dr. Jones | Cardiologist |

Visits:
| Visit_ID | Patient_ID | Doctor_ID | Visit_Date |
|-----------|-------------|------------|-------------|
| V001 | P001 | D001 | 10-Jan |
| V002 | P002 | D002 | 11-Jan |
| V003 | P001 | D001 | 12-Jan |

Medicines:
| Visit_ID | Medicine |
|-----------|-----------|
| V001 | Paracetamol |
| V001 | Ibuprofen |
| V002 | Aspirin |
| V003 | Paracetamol |

Now each attribute depends on its full key.

---

### Third Normal Form (3NF)
Rule: Be in 2NF and remove indirect (transitive) dependencies.

If a non-key column depends on another non-key column, separate it.  
In our example, all columns already depend on their keys, so this design is in 3NF.

---

### Final Normalized Structure
Patients: (Patient_ID, Name)  
Doctors: (Doctor_ID, Doctor_Name, Specialty)  
Visits: (Visit_ID, Patient_ID, Doctor_ID, Visit_Date)  
Medicines: (Visit_ID, Medicine)

---

### Summary of Normal Forms

| Normal Form | Rule | What Changed |
|--------------|------|---------------|
| 1NF | Each field has one value | Split multiple medicines into rows |
| 2NF | No partial dependency | Moved patient and doctor info to new tables |
| 3NF | No transitive dependency | Ensured all data depends directly on key |

---

### Interview Example Answer
Q: Explain normalization with an example.  
A: Suppose patient and doctor details are stored in one table with repeated data.  
In 1NF, we split multiple values into single cells.  
In 2NF, we separate related data into new tables (Patients, Doctors, Visits).  
In 3NF, we remove indirect dependencies.  
This avoids duplication and keeps data clean and consistent.

---

## 12. Common Interview Questions and Answers (Phase 2)

1. What is a relation in a database?  
A relation is a table that stores related data in rows and columns.

2. What is a primary key?  
A column or group of columns that uniquely identify each record.

3. What is a foreign key?  
A column that links one table to another.

4. Difference between primary and foreign keys.  
Primary key identifies data within its table; foreign key links to that primary key in another table.

5. What is normalization?  
The process of organizing data into smaller related tables to remove duplication.

6. What is denormalization?  
Adding redundancy to improve read speed and performance.

7. What is referential integrity?  
It ensures that foreign keys always reference valid data in other tables.

8. What are different types of relationships?  
One-to-One, One-to-Many, and Many-to-Many.

9. What is an ER model?  
A diagram showing entities, attributes, and their relationships.

10. Why use constraints?  
To maintain data accuracy and prevent invalid data entry.

---

End of Phase 2 Notes
