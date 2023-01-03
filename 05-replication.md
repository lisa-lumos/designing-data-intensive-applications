# 5. Replication
What happens if multiple machines are involved in storage and retrieval of data?

There are various reasons why you might want to distribute a database across multiple machines: Scalability, Fault tolerance / high availability, Latency. 

## Scaling to higher load
**Shared-memory architecture**: Many CPUs, many RAM chips, and many disks can be joined together under one operating system, and a fast interconnect allows any CPU to access any part of the memory or disk. All the components can be treated as a single machine. Problem: the cost grows faster than linearly, and a machine twice the size cannot necessarily handle twice the load. Limited fault tolerance, with single geographic location. 

**Shared-disk architecture**: Uses several machines with independent CPUs and RAM, but stores data on an array of disks that is shared between the machines, which are connected via a fast network. It is used for some data warehousing workloads, but contention and the overhead of locking limit the scalability of the shared-disk approach. 

**Shared-nothing architecture**: Each machine or virtual machine running the database software is called a node. Each node uses its CPUs, RAM, and disks independently. Any coordination between nodes is done at the software level, using a conventional network. Can distribute data across multiple geographic regions, go get better fault tolerance and increased availability. Can be very powerful, but incurs additional complexity for applications and sometimes limits the expressiveness of the data models you can use. 

Two common ways data is distributed across multiple nodes, and they often go together:
- Replication: Keep a copy of the same data on several different nodes, potentially in different locations. Provides redundancy: if some nodes are unavailable, the data can still be served from the remaining nodes. Replication can also help improve performance. We discuss replication in Chapter 5.
- Partitioning: Split a big database into smaller subsets (partitions), different partitions can be assigned to different nodes (sharding). 


























