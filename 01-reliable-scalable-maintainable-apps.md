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
1. **Reliability** - The system should continue to work correctly (performing the correct function at the desired level of performance) even when thing go wrong (hardware or software faults, and human error).
2. **Scalability** - As the system grows (in data/traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
3. **Maintainability** - Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

## Reliability
Bugs in business applications cause lost productivity,and legal risks if figures are reported incorrectly; and outages of e-commerce sites can have huge costs in terms of lost revenue and damage to reputation.

A `fault` is usually defined as one component of the system deviating from its spec, whereas a `failure` is when the system as a whole stops providing the required service to the user.

For `hardware faults`, we usually to add `redundancy` to the individual hardware components in order to reduce the failure rate of the system. It makes total failure of `a single machine` fairly rare (but needs planned downtime). But more applications have begun using `larger numbers of machines`, which proportionally increases the rate of hardware faults - Hence there is a move toward systems that can `tolerate the loss of entire machines`, by using software fault-tolerance techniques in preference or in addition to hardware redundancy (allows for rolling upgrade).

For `software errors`, the bugs often lie dormant for a long time until they are triggered by an unusual set of circumstances. There is no quick solution to the problem of systematic faults in software. Lots of small things can help: carefully thinking about assumptions and interactions in the system; thorough testing; process isolation; allowing processes to crash and restart; measuring, monitoring, and analyzing system behavior in production.

For `human errors`, we could Design systems in a way that minimizes opportunities for error; use sandbox environments; test thoroughly; Allow quick and easy recovery; set up detailed and clear monitoring; implement good management practices and training; etc. 

## Scalability
`Scalability` is the term we use to describe a system’s ability to `cope with increased load`. Discussing scalability means considering questions like “If the system grows in a particular way, what are our options for coping with the growth?” and “How can we add computing resources to handle the additional load?” 



























