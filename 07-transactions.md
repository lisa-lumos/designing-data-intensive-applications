# 7. Transactions
A `transaction` a mechanism for grouping multiple operations on multiple objects into one unit of execution. All the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds (commit) or it fails (abort, rollback). If it fails, the application can safely retry. With transactions, application doesn't need to worry about partial failure. 

Historically, the safety guarantees provided by transactions are known as ACID, which is vague: 
- Atomicity: means abortability - a transaction can be rolled back if there's an error in the middle. 
- Consistency: the application makes sure that certain statements (invariants) are always true.
- Isolation: concurrent transactions are isolated from each other (serializability)
- Durability: Once a transaction has committed, the data will not get lost, even with hardware fault or database crash. Means the data is written to disk, or got replicated. Perfect durability does not exist. 

Multi-object operations: These definitions assume that you want to modify several objects (rows, documents, records) at once. Such multi-object transactions are often needed if several pieces of data need to be kept in sync. Multi-object transactions need a way to determine which read and write operations belong to the same transaction. In relational databases, that is typically done based on the client’s TCP connection to the database server: on any particular connection, everything between a BEGIN TRANSACTION and a COMMIT statement is considered to be part of the same transaction.iii

Single-object operations: storage engines almost universally aim to provide atomicity and isolation on the level of a single object on one node. Atomicity can be implemented using a log for crash recovery, and isolation can be implemented using a lock on each object.































