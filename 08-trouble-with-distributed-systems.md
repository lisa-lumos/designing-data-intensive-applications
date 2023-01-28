# 8. The trouble with distributed systems
For a software on a single computer, when the hardware is working correctly, the same operation always produces the same result (deterministic). If there is a hardware problem, the consequence is usually a total system failure. An individual computer with good software is usually either fully functional or entirely broken, but not something in between.

In a distributed system, there may well be some parts of the system that are broken in some unpredictable way, even though other parts of the system are working fine - Partial failure. Partial failures are nondeterministic, which makes distributed systems hard to work with. 

Different types of large scale computing systems:
- High performance computing (HPC). Super computers with thousands of CPUs. Usually for scientific computing. A super computer is more like a single-node computer than a distributed system. It escalates partial failure to total failure. 
- Cloud computing. Multi-tenant data centers, commodity computers connected via internet, elastic resource allocation, metered billing
- Traditional enterprise datacenters lie somewhere between the both





































