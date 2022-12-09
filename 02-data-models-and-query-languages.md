# Data Models and Query Languages
Data models are one of the most important part of developing software, because they not only affect how the software is written, but also affect how we think about the problem that we are solving. 

Most applications are built by layering one data model on top of another. Each layer hides the complexity of the layers below it by providing a clean data model.

Since the data model has such a profound effect on what the software above it can and can’t do, it’s important to choose one that is appropriate to the application.

## Relational Model Versus Document Model
Much of what you see on the web today is still powered by relational databases, be it online publishing, discussion, social networking, e-commerce, games, software-as-a-service productivity applications, or much more.

There are several driving forces behind the adoption of NoSQL databases, including:
- A need for greater scalability, including very large datasets or very high write throughput
- Preference for free and open source software over commercial ones
- Specialized query that are not well supported by the relational model
- A desire for a more dynamic and expressive data model

## The Object-Relational Mismatch
Most application development today is done in object-oriented programming languages, but if data is stored in relational tables, an awkward translation layer is required between them. The disconnect between the models is sometimes called an `impedance mismatch`.

For a data structure with one to many relationships like a resume (one person with many occupations, education history, etc), which is mostly a self-contained document, a JSON representation can be quite appropriate. JSON has the appeal of being much simpler than XML. Document-oriented databases like MongoDB, RethinkDB, CouchDB, and Espresso support this data model.




























