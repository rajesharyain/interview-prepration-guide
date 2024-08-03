In SQL, isolation levels are part of the ACID properties (Atomicity, Consistency, Isolation, Durability) that ensure reliable transactions in a database. They control how transaction integrity is visible to other transactions and how changes made by one transaction are isolated from those made by others. There are four primary isolation levels defined by the SQL standard, each offering a different balance between consistency and performance.

### 1. Read Uncommitted

- **Definition**: The lowest isolation level where transactions are allowed to read data that has been modified but not yet committed by other transactions.
- **Phenomena Allowed**:
  - **Dirty Reads**: Transactions can see uncommitted changes made by other transactions.
  - **Non-Repeatable Reads**: A transaction may read the same row twice and see different data each time if another transaction modifies and commits changes in the meantime.
  - **Phantom Reads**: A transaction might see new rows in a subsequent query if another transaction inserts them and commits in the meantime.
- **Use Case**: Suitable for scenarios where performance is critical and data consistency is not a primary concern, such as read-only operations on static data.

### 2. Read Committed

- **Definition**: A common isolation level that prevents dirty reads by ensuring that any data read by a transaction is committed at the moment it is read.
- **Phenomena Allowed**:
  - **Non-Repeatable Reads**: The same row may return different data if another transaction modifies and commits changes in the meantime.
  - **Phantom Reads**: New rows might appear in subsequent queries if another transaction inserts and commits them.
- **Use Case**: Balances performance with a reasonable level of data consistency, suitable for many typical transactional applications.

### 3. Repeatable Read

- **Definition**: Ensures that if a transaction reads a row, subsequent reads within the same transaction will return the same data, preventing non-repeatable reads.
- **Phenomena Allowed**:
  - **Phantom Reads**: New rows can appear in subsequent queries if another transaction inserts and commits them.
- **Use Case**: Suitable for applications that require a higher level of consistency and can tolerate slightly reduced performance compared to Read Committed.

### 4. Serializable

- **Definition**: The highest isolation level that ensures complete isolation from other transactions, appearing as if transactions are executed serially, one after the other.
- **Phenomena Allowed**: None. Serializable isolation prevents dirty reads, non-repeatable reads, and phantom reads.
- **Use Case**: Suitable for applications that require the highest level of consistency and can tolerate reduced performance, such as financial systems where accuracy is critical.

### Summary of Isolation Levels and Allowed Phenomena

| Isolation Level      | Dirty Reads | Non-Repeatable Reads | Phantom Reads |
|----------------------|-------------|----------------------|---------------|
| Read Uncommitted     | Allowed     | Allowed              | Allowed       |
| Read Committed       | Prevented   | Allowed              | Allowed       |
| Repeatable Read      | Prevented   | Prevented            | Allowed       |
| Serializable         | Prevented   | Prevented            | Prevented     |

### Choosing an Isolation Level

The choice of isolation level depends on the specific requirements of the application, particularly the trade-off between consistency and performance:

- **Read Uncommitted**: Use when performance is critical, and dirty reads are acceptable.
- **Read Committed**: Use for a balance of consistency and performance, suitable for most applications.
- **Repeatable Read**: Use when repeatable reads are necessary, and some phantom reads are acceptable.
- **Serializable**: Use when the highest level of consistency is required, and performance can be sacrificed.

# Phenomena Prevented and Allowed
Prevented Phenomena:
Dirty Reads: A dirty read occurs when a transaction reads uncommitted changes made by another transaction. Read Committed prevents dirty reads because it only allows reading committed data.
Allowed Phenomena:
Non-Repeatable Reads: A non-repeatable read occurs when a transaction reads the same row twice and gets different values each time because another transaction modified the row and committed the change between the two reads. Read Committed allows non-repeatable reads.
Phantom Reads: A phantom read occurs when a transaction reads a set of rows that satisfy a condition and, upon re-execution of the query, finds additional rows that satisfy the condition because another transaction inserted and committed new rows. Read Committed allows phantom reads.
