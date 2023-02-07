# 9. Consistency and Consensus
Algorithms and protocols for building fault-tolerant distributed systems.

Consensus: getting all of the nodes to agree on something.

For a database with single-leader replication, if two nodes both believe that they are the leader, that situation is called split brain, and it often leads to data loss.

Most replicated databases provide at least eventual consistency, which means that if you stop writing to the database and wait for some unspecified length of time, then eventually all read requests will return the same value. In this case, if you write a value and then immediately read it again, there is no guarantee that you will see the value you just wrote, because the read may be routed to a different replica. 

Systems with stronger guarantees may have worse performance or be less fault-tolerant than systems with weaker guarantees. Nevertheless, stronger guarantees can be appealing because they are easier to use correctly.

## Linearizability
Make a system appear as if there were only one copy of the data, and all operations on it are atomic. Therefore, even though there may be multiple replicas in reality, the application does not need to worry about them.

Maintaining the illusion of a single copy of the data means guaranteeing that the value read is the most recent, up-to-date value, and doesn’t come from a stale cache or replica.

























