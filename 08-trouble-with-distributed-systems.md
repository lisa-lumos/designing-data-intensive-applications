# 8. The trouble with distributed systems
For a software on a single computer, when the hardware is working correctly, the same operation always produces the same result (deterministic). If there is a hardware problem, the consequence is usually a total system failure. An individual computer with good software is usually either fully functional or entirely broken, but not something in between.

In a distributed system, there may well be some parts of the system that are broken in some unpredictable way, even though other parts of the system are working fine - Partial failure. Partial failures are nondeterministic, which makes distributed systems hard to work with. 

Different types of large scale computing systems:
- High performance computing (HPC). Super computers with thousands of CPUs. Usually for scientific computing. A super computer is more like a single-node computer than a distributed system. It escalates partial failure to total failure. 
- Cloud computing. Multi-tenant data centers, commodity computers connected via internet, elastic resource allocation, metered billing
- Traditional enterprise datacenters lie somewhere between the both

Cloud computing:
- need to serve users with low latency at any time
- nodes are built from commodity machines - low cost, but higher failure rates
- often based on internet
- bigger system with thousands of nodes always have something broken. need to tolerate failed nodes and still keep working
- internet an be slow and unreliable for geo distributed system

Therefore, we need to build a reliable system from unreliable components. 

## Unreliable Networks
shared-nothing systems: i.e., a bunch of machines connected by a network.

Shared-nothing has become the dominant approach for building internet services, because it is cheap, can use cloud computing services, and can achieve high availability through redundancy across multiple geo distributed datacenters. 

The internet and most internal networks in datacenters are async packet networks. One node can send a message (packet) to another node, but the network gives no guarantees as to when it will arrive, or whether it will arrive. If you send a request and expect a response, many things could go wrong. If you send a request to another node and don’t receive a response, it is impossible to tell why. The usual way of handling this issue is a timeout. 

You need to know how your software reacts to network problems and ensure that the system can recover from them.

Many systems need to automatically detect faulty nodes.

























