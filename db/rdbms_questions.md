# Solid questions focusing on Relational Database Management Systems (RDBMS), with a particular emphasis on PostgreSQL and ACID properties:

### Basic Concepts
1. **What are the ACID properties in a database? Explain each property.**
2. **What is a primary key, and why is it important?**
3. **What is a foreign key, and how does it enforce referential integrity?**
4. **Explain the difference between a primary key and a unique key.**
5. **What is a join? Explain the different types of joins available in SQL.**

#### Ans:
### ACID Properties in a Database

The ACID properties are a set of principles that ensure reliable processing of database transactions. They stand for:

1. **Atomicity**:
   - **Definition**: Ensures that a transaction is treated as a single, indivisible unit. It either completes in its entirety or not at all.
   - **Example**: In a bank transfer, money must be deducted from one account and added to another. If one part of the transaction fails, the entire transaction is rolled back.

2. **Consistency**:
   - **Definition**: Ensures that a transaction brings the database from one valid state to another, maintaining database invariants (rules).
   - **Example**: If a database rule states that account balances cannot be negative, any transaction violating this rule will be rolled back.

3. **Isolation**:
   - **Definition**: Ensures that concurrent transactions do not affect each other. Each transaction should appear to run in isolation from others.
   - **Example**: Two transactions modifying the same data do not interfere with each other and produce the same result as if they were run sequentially.

4. **Durability**:
   - **Definition**: Ensures that once a transaction is committed, its effects are permanently recorded in the database, even in case of a system failure.
   - **Example**: After a successful bank transfer, the changes to account balances are saved and will survive a crash or power failure.

### Primary Key

- **Definition**: A primary key is a column or a set of columns that uniquely identify a row in a table.
- **Importance**: 
  - **Uniqueness**: Ensures that each row can be uniquely identified.
  - **Indexing**: Automatically creates a unique index on the column(s), improving search performance.
  - **Data Integrity**: Prevents duplicate rows and ensures each row is easily accessible.

### Foreign Key

- **Definition**: A foreign key is a column or a set of columns in one table that uniquely identifies rows in another table.
- **Referential Integrity**:
  - **Enforcement**: Ensures that the values in the foreign key column match values in the primary key column of the referenced table.
  - **Cascading Actions**: Supports actions like CASCADE, SET NULL, or RESTRICT on updates and deletions, ensuring that related data remains consistent.

### Difference Between Primary Key and Unique Key

- **Primary Key**:
  - **Uniqueness**: Ensures unique identification of each row.
  - **NULL Values**: Does not allow NULL values.
  - **Single per Table**: Only one primary key can be defined per table.

- **Unique Key**:
  - **Uniqueness**: Ensures that all values in the column(s) are unique.
  - **NULL Values**: Allows one or more NULL values (depending on the database system).
  - **Multiple per Table**: Multiple unique keys can be defined per table.

### Joins in SQL

Joins are used to combine rows from two or more tables based on a related column. The different types of joins are:

1. **Inner Join**:
   - **Definition**: Returns rows when there is a match in both tables.
   - **Example**: `SELECT * FROM A INNER JOIN B ON A.id = B.id;`

2. **Left (Outer) Join**:
   - **Definition**: Returns all rows from the left table and the matched rows from the right table. If no match is found, NULLs are returned for columns from the right table.
   - **Example**: `SELECT * FROM A LEFT JOIN B ON A.id = B.id;`

3. **Right (Outer) Join**:
   - **Definition**: Returns all rows from the right table and the matched rows from the left table. If no match is found, NULLs are returned for columns from the left table.
   - **Example**: `SELECT * FROM A RIGHT JOIN B ON A.id = B.id;`

4. **Full (Outer) Join**:
   - **Definition**: Returns rows when there is a match in one of the tables. If there is no match, the result is NULL from the non-matching side.
   - **Example**: `SELECT * FROM A FULL OUTER JOIN B ON A.id = B.id;`

5. **Cross Join**:
   - **Definition**: Returns the Cartesian product of both tables, i.e., all possible combinations of rows.
   - **Example**: `SELECT * FROM A CROSS JOIN B;`

6. **Self Join**:
   - **Definition**: Joins a table to itself.
   - **Example**: `SELECT A.*, B.* FROM A INNER JOIN A B ON A.id = B.parent_id;`

### Example SQL Statements

**Inner Join**:
```sql
SELECT employees.name, departments.name 
FROM employees 
INNER JOIN departments ON employees.department_id = departments.id;
```

**Left Join**:
```sql
SELECT employees.name, departments.name 
FROM employees 
LEFT JOIN departments ON employees.department_id = departments.id;
```

**Right Join**:
```sql
SELECT employees.name, departments.name 
FROM employees 
RIGHT JOIN departments ON employees.department_id = departments.id;
```

**Full Outer Join**:
```sql
SELECT employees.name, departments.name 
FROM employees 
FULL OUTER JOIN departments ON employees.department_id = departments.id;
```

**Cross Join**:
```sql
SELECT employees.name, departments.name 
FROM employees 
CROSS JOIN departments;
```

**Self Join**:
```sql
SELECT A.name, B.name 
FROM employees A 
INNER JOIN employees B ON A.manager_id = B.id;
```


### PostgreSQL Specific
6. **What is the difference between `VARCHAR` and `TEXT` in PostgreSQL?**
7. **How do you create a user and grant them permissions in PostgreSQL? Provide SQL commands.**
8. **What are the different types of indexes available in PostgreSQL? Explain each type briefly.**
9. **How would you perform a full-text search in PostgreSQL? Provide an example.**
10. **Explain the concept of table inheritance in PostgreSQL.**

### Advanced Concepts
11. **What is a transaction in a database, and how is it used? Provide an example in PostgreSQL.**
12. **Explain the concept of isolation levels in a transaction. What isolation levels are supported by PostgreSQL?**
13. **What are materialized views, and how do they differ from regular views in PostgreSQL?**
14. **Describe the process of database normalization. What are the normal forms, and why are they important?**
15. **Explain the concept of a deadlock. How can deadlocks be prevented in a database system?**

### ACID Properties
16. **What is atomicity in the context of a database transaction? Provide an example.**
17. **Explain consistency as one of the ACID properties. How does PostgreSQL ensure consistency?**
18. **What is isolation in a database transaction? How does PostgreSQL handle isolation?**
19. **Define durability in terms of database transactions. How is durability achieved in PostgreSQL?**
20. **Give an example scenario where a transaction would fail to meet the ACID properties and explain the consequences.**

### Performance and Optimization
21. **How do you analyze and optimize a slow query in PostgreSQL?**
22. **What are some common techniques for indexing in PostgreSQL?**
23. **How can you use EXPLAIN and EXPLAIN ANALYZE to understand query performance in PostgreSQL?**
24. **What are some strategies for database partitioning in PostgreSQL?**
25. **Describe the role of VACUUM in PostgreSQL. Why is it necessary?**

### Practical PostgreSQL Questions
26. **Write a SQL query to find the second highest salary from the employees table in PostgreSQL.**
27. **How would you implement a many-to-many relationship in PostgreSQL? Provide the SQL commands.**
28. **Explain the use of CTE (Common Table Expressions) in PostgreSQL. Provide an example.**
29. **How do you handle large datasets in PostgreSQL? What techniques can you use to improve performance?**
30. **Write a SQL command to create a composite index on the `employees` table for the columns `department` and `salary`.**

### Data Integrity and Security
31. **What is data integrity, and how can it be enforced in PostgreSQL?**
32. **Describe the different types of constraints available in PostgreSQL.**
33. **How can you implement row-level security in PostgreSQL?**
34. **What are roles and privileges in PostgreSQL? How do you manage them?**
35. **Explain the concept of data encryption at rest and in transit. How can it be achieved in PostgreSQL?**
