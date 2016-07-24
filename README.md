# nice-things #################################################################

This whole repo is just a bunch of text about nice things, all of which have
something to do with software development.


## Java

- statically typed. I find it easier to read and modify code if I know the
  types of things.
- mature ecosystem of libraries and tools
- fast (on the server)
- IntelliJ is a great IDE

Caveats:
- Java EE and application servers have lost their mojo. Prefer Java SE and
  embedded web servers.


## Cassandra NoSQL database

- really fast
- instant strong consistency is possible using *quorum* writes and reads, even
  if a node is down.
- linear horizontal scaling up to millions of writes/sec
- masterless. no single point of failure
- no wait for failover when a node goes down
- near real time multi-datacenter replication is relatively easy
- SQL-like syntax, tables, columns

Caveats:
- no joins. Denormalize your data, perhaps using views or CQRS
- Automating backups can be a whole project


## Spring Data

Spring Data is a Java framework for database access. It promotes the
*repository* design pattern and uses annotations and reflection to map between
classes and tables.


## The Optional type in Java

I use it mainly to remind my future self that functions sometimes return
non-values which often requires special handling.


## Refactoring and legacy code

As new features are added to the codebase, it should be habitually refactored
so that it does not rot and become difficult to understand or modify.

Code that is not covered by automated tests can't be confidently refactored.

One definition of legacy code is a codebase that is not covered by automated tests.
