# Roadmap

## Purpose

This roadmap turns the repository into a study program. It is organized from baseline backend competence to senior-level system ownership.

## Stage 1: Core Engineering Foundations

Focus on clarity, not speed.

- Read [general/fundamentals/data-structures.md](general/fundamentals/data-structures.md)
- Read [general/fundamentals/web-servers.md](general/fundamentals/web-servers.md)
- Read [general/fundamentals/testing-fundamentals.md](general/fundamentals/testing-fundamentals.md)
- Read [backend/dotnet/fundamentals/csharp-fundamentals.md](backend/dotnet/fundamentals/csharp-fundamentals.md)
- Read [backend/dotnet/fundamentals/oop.md](backend/dotnet/fundamentals/oop.md)

### Checkpoint

You should be able to explain:

- value types vs reference types
- abstraction vs encapsulation
- HTTP request flow through a backend service
- the difference between unit, integration, and end-to-end tests

## Stage 2: Production .NET Backend Development

Focus on application design and framework behavior.

- Read [backend/dotnet/framework/aspnet-core.md](backend/dotnet/framework/aspnet-core.md)
- Read [backend/dotnet/framework/entity-framework.md](backend/dotnet/framework/entity-framework.md)
- Read [backend/dotnet/fundamentals/dependency-injection.md](backend/dotnet/fundamentals/dependency-injection.md)
- Read [backend/dotnet/fundamentals/async-await.md](backend/dotnet/fundamentals/async-await.md)
- Read [backend/dotnet/fundamentals/multithreading-and-thread-safety.md](backend/dotnet/fundamentals/multithreading-and-thread-safety.md)

### Checkpoint

You should be able to explain:

- request lifetime in ASP.NET Core
- `async`/`await` without hand-waving
- service lifetimes in DI
- common EF Core performance failures

## Stage 3: Data And Persistence Depth

Focus on query behavior, consistency, and schema decisions.

- Read [backend/databases/fundamentals/relational-database-design.md](backend/databases/fundamentals/relational-database-design.md)
- Read [backend/databases/fundamentals/indexing.md](backend/databases/fundamentals/indexing.md)
- Read [backend/databases/fundamentals/query-optimization.md](backend/databases/fundamentals/query-optimization.md)
- Read [backend/databases/fundamentals/transactions-and-isolation-levels.md](backend/databases/fundamentals/transactions-and-isolation-levels.md)

### Checkpoint

You should be able to explain:

- why an index helps one query and hurts another
- what read committed and snapshot isolation actually change
- how poor schema design leaks into application complexity

## Stage 4: Architecture And System Boundaries

Focus on service boundaries, coupling, and maintainability.

- Read [backend/dotnet/architecture/ddd.md](backend/dotnet/architecture/ddd.md)
- Read [backend/dotnet/architecture/clean-architecture.md](backend/dotnet/architecture/clean-architecture.md)
- Read [backend/dotnet/architecture/design-patterns.md](backend/dotnet/architecture/design-patterns.md)
- Read [backend/architecture/fundamentals/system-design-basics.md](backend/architecture/fundamentals/system-design-basics.md)
- Read [backend/architecture/fundamentals/rest-api-design.md](backend/architecture/fundamentals/rest-api-design.md)

### Checkpoint

You should be able to explain:

- when layering helps and when it becomes ceremony
- how to define service boundaries
- how idempotency, caching, and rate limiting fit into API design

## Stage 5: Distributed Systems And Reliability

Focus on failure modes, consistency, and asynchronous communication.

- Read [backend/distributed-systems/fundamentals/distributed-systems-fundamentals.md](backend/distributed-systems/fundamentals/distributed-systems-fundamentals.md)
- Read [backend/distributed-systems/fundamentals/microservices.md](backend/distributed-systems/fundamentals/microservices.md)
- Read [backend/distributed-systems/fundamentals/event-driven-architecture.md](backend/distributed-systems/fundamentals/event-driven-architecture.md)
- Read [backend/distributed-systems/fundamentals/kafka.md](backend/distributed-systems/fundamentals/kafka.md)
- Read [backend/distributed-systems/fundamentals/resiliency-patterns.md](backend/distributed-systems/fundamentals/resiliency-patterns.md)

### Checkpoint

You should be able to explain:

- at-least-once delivery implications
- why distributed transactions are avoided
- the difference between retries, backoff, circuit breakers, and hedging

## Stage 6: Operations, Scale, And Delivery

Focus on production readiness.

- Read [backend/devops/fundamentals/ci-cd.md](backend/devops/fundamentals/ci-cd.md)
- Read [backend/devops/fundamentals/docker.md](backend/devops/fundamentals/docker.md)
- Read [backend/devops/fundamentals/kubernetes.md](backend/devops/fundamentals/kubernetes.md)
- Read [backend/devops/fundamentals/monitoring-and-observability.md](backend/devops/fundamentals/monitoring-and-observability.md)
- Read [general/fundamentals/debugging-and-troubleshooting.md](general/fundamentals/debugging-and-troubleshooting.md)

### Checkpoint

You should be able to explain:

- how code moves safely from commit to production
- what to monitor for a backend service
- how containerization changes packaging but not architecture

## Backend Interview Preparation Path

Use this order if the primary goal is interview performance:

1. [general/interview/testing.md](general/interview/testing.md)
2. [backend/dotnet/interview/dotnet.md](backend/dotnet/interview/dotnet.md)
3. [backend/dotnet/interview/async-concurrency-threading.md](backend/dotnet/interview/async-concurrency-threading.md)
4. [backend/dotnet/interview/entity-framework.md](backend/dotnet/interview/entity-framework.md)
5. [backend/databases/interview/sql.md](backend/databases/interview/sql.md)
6. [backend/distributed-systems/interview/microservices.md](backend/distributed-systems/interview/microservices.md)
7. [backend/distributed-systems/interview/kafka.md](backend/distributed-systems/interview/kafka.md)
8. [backend/architecture/interview/system-design.md](backend/architecture/interview/system-design.md)
9. [backend/devops/interview/docker-kubernetes.md](backend/devops/interview/docker-kubernetes.md)
10. [general/interview/behavioral.md](general/interview/behavioral.md)

## Senior-Level Signals To Practice

Interviewers usually expect more than correct definitions. Practice answering with:

- context: when the concept matters
- trade-offs: what improves and what gets worse
- failure modes: what breaks in production
- decision criteria: why one approach fits a specific system
- operational awareness: how to observe and debug it
