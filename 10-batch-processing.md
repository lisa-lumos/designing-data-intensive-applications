# 10. Batch Processing
Applications commonly use a combination of several different datastores, indexes, caches, analytics systems, etc. and implement mechanisms for moving data from one store to another. In reality, integrating disparate systems is one of the most important things that needs to be done in a nontrivial application.

A `system of record`, also known as source of truth, holds the authoritative version of your data. `Data in a derived system` is the result of taking some existing data from another system and transforming or processing it in some way.

Technically speaking, derived data is redundant, in the sense that it duplicates existing information. However, it is often essential for getting good performance on read queries. It is commonly denormalized.

Three different types of systems:
- Services (online systems). A service waits for a request or instruction from a client to arrive. When one is received, the service tries to handle it ASAP and sends a response back. Response time is usually the primary measure of performance of a service, and availability is often very important. 
- Batch processing systems (offline systems). Takes a large amount of input data, runs a job to process it, and produces some output data. Often scheduled to run periodically. The primary performance measure of a batch job is usually throughput. 
- Stream processing systems (near-real-time systems). A stream processor consumes inputs and produces outputs. 

The Unix philosophy: Automation, rapid prototyping, incremental iteration, being friendly to experimentation, and breaking down large projects into manageable chunks - sounds remarkably like the Agile and DevOps movements of today.

A Unix shell like bash lets us easily compose these small programs into surprisingly powerful data processing jobs.

If you expect the output of one program to become the input to another program, that means those programs must use the same data format—in other words, a compatible interface. In Unix, that interface is a file. 

























