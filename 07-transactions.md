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

Many dbs that implement snapshot isolation call it by different names. In Oracle it is called `serializable`, and in PostgreSQL and MySQL it is called `repeatable read`. 

The `lost update problem` can occur if an application reads some value from the database, modifies it, and writes back the modified value (a read-modify-write cycle). If two transactions do this concurrently, for the same record, one of the modifications can be lost, because the second write does not include the first modification. Cases are:
- Incrementing a counter or updating an account balance (a read-modify-write cycle)
- Making a local change to a complex value, e.g., adding an element to a list within a JSON document (requires parsing the document, making the change, and writing back the modified document)
- Two users editing a wiki page at the same time, where each user saves their changes by sending the entire page contents to the server, overwriting whatever is currently in the database. 

Solutions to the `lost update problem`: 
- Atomic write operation: remove the need to implement read-modify-write cycles in application code. Usually implemented by taking an exclusive lock on the object when it is read so that no other transaction can read it until the update has been applied. eg: UPDATE counters SET value = value + 1 WHERE key = 'foo';. 
- Explicit locking: the application explicitly lock the objects that are going to be updated. Then the application can perform a read-modify-write cycle, and if any other transaction tries to concurrently read the same object, it is forced to wait until the first read-modify-write cycle has completed. eg: select * from ... where ... FOR UPDATE. 
- Automatically detect lost updates: allow transactions to execute in parallel, and if the transaction manager detects a lost update, abort the transaction and force it to retry. This is less error prone compared with the first two solutions. PostgreSQL's repeatable read, Oracle's serializable, and SQL Server's snapshot isolation levels automatically do this. But MySQL, InnoDB's repeatable read doesn't do this. 
- Compare-and-set: in dbs that do not provide transactions, it avoids lost updates by allowing an update to happen only if the value has not changed since you last read it. It may not be safe, depends on the db. eg: UPDATE wiki_pages SET content = 'new content' WHERE id = 1234 AND content = 'old content';

For replicated dbs, to prevent lost updates, a common approach is to allow concurrent writes to create several conflicting versions of a value (siblings), and to use application code or special data structures to resolve and merge these versions after the fact. 

`Write skew`: concurrent multi-object updates that violates a pre-checked condition: 
- A hospital must have at least one doctor on call. Doctor A and Doctor B both check how many doctors are there at the same time, and both request leave at the same time, resulting 0 doctor on call. They both modified their own records via a transaction, so it is neither a dirty write nor a lost update. 
- Meeting room booking system: two users concurrently inserting a conflicting meeting. 
- Multiplayer game: two players move different chess to the same position. 
- Claiming a username: two new user concurrently choose a same user name. Can use unique constraint to avoid this one. 

`Phantom`: the result of a search query in a transaction is no longer valid after a write in another transaction. Snapshot isolation can cause write skew in such read-write transactions. 

Solution: 
- Automatically preventing write skew requires true serializable isolation. 
- If you can’t use a serializable isolation level, you can explicitly lock the rows that the transaction depends on, using the FOR UPDATE clause (only doable if there are objects/rows to attach lock to). 
- Materializing conflicts: If there is no object to attach the locks, you can create (materialize) rows, such as creating rows for each time slots. This lets a concurrency control mechanism leak into the application level, so it is not recommended to do this. Recommend serializable isolation in this case. 

## Serializable isolation
The strongest isolation level that prevents all possible race conditions. It guarantees that even though transactions may execute in parallel, the end result is the same as if they had executed one at a time, serially. 

There are 3 techniques to implement it:
- Literally executing transactions in a serial order, on a single thread. Avoids the coordination overhead, but throughput is limited to a single CPU core (can be resolved by partitioning, if you can make them independent of each other). Systems with single-threaded serial transaction processing don’t allow interactive multi-statement transactions, because humans and networks are slow; instead, the app use a stored procedure. Used by VoltDB/H-Store, Redis and Datomic. 
- Two-phase locking. 
- Use optimistic concurrency control such as serializable snapshot isolation. 


































