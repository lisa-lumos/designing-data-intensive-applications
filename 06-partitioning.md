# 6. Partitioning
For very large datasets or very high query throughput, to get better scalability, we need to break the data up into partitions (sharding). Different partitions can be placed on different nodes in a shared-nothing cluster, so a large dataset can be distributed across many disks, and the query load can be distributed across many processors.

Normally, each piece of data (record, row, or document) belongs to exactly one partition. A node may store more than one partition.

Partitioning is usually combined with replication so that copies of each partition are stored on multiple nodes. If leader-follower replication is used, each partition’s leader is assigned to one node, and its followers are assigned to other nodes. Each node may be the leader for some partitions and a follower for other partitions. The choice of partitioning scheme is mostly independent of the choice of replication scheme. 
























