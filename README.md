# gudmojo's list of nice things

This whole repo is just a bunch of text about nice things, most of which have
something to do with software development. These are just my personal
preferences, you may agree or disagree.

## Go

- Reading other people's Go code is almost always easy.
- The error-as-result mechanism works very nicely once you get used to it.
- Efficient usage of memory and cpu
- GoLand is a great IDE
- Recommended reading: Learning Go: An Idiomatic Approach to Real-World Go
  Programming by Jon Bodner

## Java

- I used to write a lot of Java in the past, but now I tend to lean towards Go.
- statically typed. I find it easier to read and modify code if I know the
  types of things.
- mature ecosystem of libraries and tools
- fast (on the server)
- IntelliJ is a great IDE

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

## Refactoring and legacy code

As new features are added to the codebase, it should be habitually refactored
so that it does not rot and become difficult to understand or modify.

Before adding a feature, refactor the code in a way that will make the feature
straight-forward to implement.

Code that is not covered by automated tests can't be confidently refactored.

One definition of legacy code is a codebase that is not covered by automated
tests.

## Infrastructure as code

Terraform is the default. If your infrastructure can be productively managed
using Terraform, do that.

I'm interested in looking at Crossplane as a partial replacement for Terraform,
but I haven't fully explored it

## Lean on your cloud provider

Cloud providers provide managed solutions that can reduce the work that goes
into maintaining and operating self-managed solutions. Don't do everything
yourself just to avoid "vendor lock-in".

There is also sometimes a third party that offers managed services. They can be
a good option, but make sure you are ok with depending on their service uptime
and access to support.

## Build your artifact once

The same artifact should flow through the release process from CI build to
production. Don't build for each deploy or build after QA gives go-ahead.

Otherwise you don't really know that the thing that passed testing is the same
as the thing you have in production. Also, an extra build is a waste of time
and resources.

## Hexagonal architecture (ports & adapters)

Keep a clear separation between your application's business logic and the code
that integrates to external actors such as the UI and the database.

Example: The business logic needs to look up currency exchange rates via an
external service. In that case the integration (constructing a json request and
sending it to a specific server) is written in a module unknown to the core
domain, but implementing an interface that the core domain specifies.

When this approach is used it's possible to achieve high flexibility in
switching to a different implementation of the same abstract service. It also
keeps the business logic clean and readable.

## Dependency Injection via constructor

I prefer to do my dependency injection manually. Each service type has a
constructor that receives all the dependencies that the service object needs.
The main function or entry point for the application is the place where I wire
up all the services manually by invoking constructors:

    dbConn = postgres.NewConnection(url)
    userRepo = postgres.NewUserRepo(dbConn)
    bookRepo = postgres.NewBookRepo(dbConn)
    server = server.New(userRepo, bookRepo)

The main function will be long and could be seen as a bit unvieldy, but in
my opinion it's nice to do it all there so you can see explicitly how everything
is wired and keep the rest of the application clean.

## Team-owned services

A service should belong to a *single* team. They have the freedom to evolve it
as needed and the responsibility to keep the code in a good state. People outside
the team can collaborate with the team to get changes to the service.

## Developers are responsible for testing their changes

This creates a culture of taking quality into your own hands and creating fast
automated tests.

## Continuous deployment, controlled releases via feature flags

Empower developers to land code in production in a matter of minutes. Larger
features need to be communicated and timed, so hide them behind feature flags.

## Merge queue

Before merging PRs we want them to be 1) up to date with the main branch, and
2) passing CI with those updates. If the repo has many PRs about to be merged,
this can cause a game of whack-a-mole with rebasing and running CI, only to
find that there is another update on the main branch, so you need to rebase
again and wait for CI, wasting developers time and machine resources. This can
be solved by automation. The PRs are added to a merge queue and a bot takes care
of rebasing and merging one PR at a time.

GitHub.com and GitLab have built in support for merge queues.

## Skaffold

 Skaffold (an open source project from Google) is a CLI for local development
 of a Kubernetes application, and can also be used in CI/CD so that both
 development and production deployments use the same tool.
