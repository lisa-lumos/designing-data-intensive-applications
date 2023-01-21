# 7. Transactions
A `transaction` a mechanism for grouping multiple operations on multiple objects into one unit of execution. All the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds (commit) or it fails (abort, rollback). If it fails, the application can safely retry. With transactions, application doesn't need to worry about partial failure. 

Historically, the safety guarantees provided by transactions are known as ACID, which is vague: 
- Atomicity: means abortability - a transaction can be rolled back if there's an error in the middle. 
- Consistency: the application makes sure that certain statements (invariants) are always true.
- Isolation: concurrent transactions are isolated from each other (serializability)
- Durability: Once a transaction has committed, the data will not get lost, even with hardware fault or database crash. Means the data is written to disk, or got replicated. Perfect durability does not exist. 

Multi-object operations: These definitions assume that you want to modify several objects (rows, documents, records) at once. Such multi-object transactions are often needed if several pieces of data need to be kept in sync. Multi-object transactions need a way to determine which read and write operations belong to the same transaction. In relational databases, that is typically done based on the client’s TCP connection to the database server: on any particular connection, everything between a BEGIN TRANSACTION and a COMMIT statement is considered to be part of the same transaction.iii

Single-object operations: storage engines almost universally aim to provide atomicity and isolation on the level of a single object on one node. Atomicity can be implemented using a log for crash recovery, and isolation can be implemented using a lock on each object.

Cases when multi-object transactions are needed: 
- In a relational data model, when updating one row in a table, and this row has a foreign key reference to a row in another table
- In a document data model, when denormalized data needs to be updated, transaction make sure this type of data are in sync
- dbs with secondary indexes, these indexes also need to be updated when value is updated. 

## Weak isolation levels
Concurrency bugs caused by weak transaction isolation have caused serious issues in production.

### Read Committed
Most basic level of transaction isolation:
- No dirty reads: You only see committed data, will not see the database in a partially updated state. 
- No dirty writes: You can only modify committed data, not the data that are currently being modified in another transaction. 

It is the default setting in Oracle 11g, PostgreSQL, SQL Server 2012, MemSQL, etc. 

Databases usually prevent dirty writes by using row-level locks. Only one transaction can hold the lock for any given object. 

Databases usually prevent dirty reads by remembering both the old committed value and the new value set by the on-going transaction. So other transactions that are reading the objects are just given the old value. 

### Snapshot isolation
Problem with read committed transaction isolation level: Alice has 2 bank account, each have $500. Alice checks account 1, and saw 500$. A transfer of $100 from account 2 to account 1 happens, and then completes. Alice now checks account 2, and saw 400$. It seems between her 2 checks, 100$ disappeared. 

Such "non-repeatable read" (read skew) over a time period can cause problems with db backups that takes time to complete (will cause old data and new data over a span of time to be spliced together in a backup). It also cause problem to a time consuming analytical query, or integrity checks to check for system corruption. 

Snapshot isolation is the most common solution to this problem - Each transaction reads (sees) from a snapshot of the database taken at one single point of time, instead of over a time span. 

Snapshot isolation is supported by PostgreSQL, MySQL with InnoDB storage engine, Oracle, SQL Server, etc. 

The database must potentially keep several different committed versions of an object, because various in-progress transactions may need to see the state of the database at different points in time. Because it maintains several versions of an object side by side, this technique is known as multiversion concurrency control (MVCC). When a transaction reads from the database, transaction IDs are used to decide which objects it can see and which are invisible.

















