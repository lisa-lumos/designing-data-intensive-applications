# 1. Reliable, Scalable, and Maintainable Applications
Many applications today are `data-intensive` - their limiting factor: `amount/complexity/speed-of-change of data`. When building an application, we need to figure out `which tools and which approaches are the most appropriate` for the task. And it can be hard to `combine tools` when you need to do something that a single tool cannot do alone.

Why put data storage tools all under `data systems`?
- The `boundaries` between the traditional categories are becoming blurred. (Redis for message queues, Kafka for db-like durability guarantees, etc)
- Apps now have diverging requirements, so a single tool is no longer sufficient. Need to `break down into tasks` that can be performed with `a single tool`, then `stitch` different tools together using app code. (create `special-purpose data systems` from `smaller, general-purpose components`)

Many factors influence the design of a data system, depend on the situation:
- the skills and experience of the people
- legacy system dependencies
- the timescale for delivery
- your org’s tolerance of different kinds of risk
- regulatory constraints
- ...

There are 3 concerns that are important in most software systems: 
1. **Reliability** - The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error).
2. **Scalability** - As the system grows (in data/traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
3. **Maintainability** - Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.



































