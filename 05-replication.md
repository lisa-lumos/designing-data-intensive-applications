Distributed data section

What happens if multiple machines are involved in storage and retrieval of data?

There are various reasons why you might want to distribute a database across multiple machines: Scalability, Fault tolerance / high availability, Latency. 

## Scaling to higher load
**Shared-memory architecture**: Many CPUs, many RAM chips, and many disks can be joined together under one operating system, and a fast interconnect allows any CPU to access any part of the memory or disk. All the components can be treated as a single machine. Problem: the cost grows faster than linearly, and a machine twice the size cannot necessarily handle twice the load. Limited fault tolerance, with single geographic location. 

**Shared-disk architecture**: Uses several machines with independent CPUs and RAM, but stores data on an array of disks that is shared between the machines, which are connected via a fast network. It is used for some data warehousing workloads, but contention and the overhead of locking limit the scalability of the shared-disk approach. 

**Shared-nothing architecture**: Each machine or virtual machine running the database software is called a node. Each node uses its CPUs, RAM, and disks independently. Any coordination between nodes is done at the software level, using a conventional network. Can distribute data across multiple geographic regions, go get better fault tolerance and increased availability. Can be very powerful, but incurs additional complexity for applications and sometimes limits the expressiveness of the data models you can use. 

Two common ways data is distributed across multiple nodes, and they often go together:
- Replication: Keep a copy of the same data on several different nodes, potentially in different locations. Provides redundancy: if some nodes are unavailable, the data can still be served from the remaining nodes. Replication can also help improve performance. 
- Partitioning: Split a big database into smaller subsets (partitions), different partitions can be assigned to different nodes (sharding). 

# 5. Replication
Replication: keeping a copy of the same data on multiple machines that are connected via a network. It reduces latency, increases availability, and increase read throughput. 

3 popular algorithms for replicating changes between nodes: single-leader, multi-leader, and leaderless replication. Almost all distributed databases use one of these three approaches.

Each node that stores a copy of the database is called a `replica`. Every write to the database needs to be processed by every replica, so they have same data. 

Leader-based replication: One of the replicas is designated the leader - every write to the db has to go through the leader, and it writes new data to its local storage. This leader also send the data change to all of its followers (other replicas), so followers can update their local copy. Clients can query any replicas, but only leader takes writes. It is used in PostgreSQL, MySQL, Oracle, SQL Server, MongoDB, Kafka, ...

Synchronous replication: Advantage: follower is guaranteed to have up-to-date data consistent with the leader, so data is still available on follower if leader fails. Disadvantage: if sync follower is unavailable, the write halts. 

Asynchronous replication: Advantage: leader can still process writes when follower not available. Disadvantage: if leader fails and unrecoverable, the writes that are not synced yet are lost. 

## Achieve high availability with leader-based replication
Follower failure: on local disk, each follower keeps a log of the data changes it has received from the leader. After it is back, it checks the log and get missed data changes from the leader. 

Leader failure: Using failover - promote a follower to be the new leader, clients reconfigured to send writes to the new leader, other follower start to take data changes from the new leader. It can be manual or automatic. 

In certain fault scenarios, it could happen that two nodes both believe that they are the leader (split brain), and it can cause lost or corrupted data. 

## Implement Replication Logs
- Statement-based replication: The leader logs every write request that it executes and sends this statement log to its followers. Potential issues: nondeterministic statement, order of execution. Used by VoltDB. 
- Write-ahead log (WAL) shipping: Every write is originally appended to a log, this exact same log can be used to build a replica on another node. Potential issues: this log is low level (which bytes changed on the which disk blocks), so coupled to the storage engine. Cannot handle it if storage format of the db changes, so requires same version of db software on all nodes. Need downtime to update db versions, so no rolling upgrade possible. Used by PostgreSQL and Oracle. 
- Logical (row-based) log replication: decouple physical storage engine and log using logical log. Allows for different storage formats on different nodes, and hence rolling upgrade. MySQL binlog when using row-based replication can do change data capture, and can send these changes to DWH for analysis. 
- Trigger-based replication: 








