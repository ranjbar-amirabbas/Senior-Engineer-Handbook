# DDD Interview Questions

## Intro

Domain-driven design questions evaluate whether you can model business complexity cleanly rather than merely naming patterns. Strong answers make it clear that DDD is a tool for difficult domains, not a ceremony package to apply everywhere.

## Use This File With

- [../architecture/ddd.md](../architecture/ddd.md)
- [../architecture/clean-architecture.md](../architecture/clean-architecture.md)
- [../../distributed-systems/fundamentals/microservices.md](../../distributed-systems/fundamentals/microservices.md)

## Modeling Concepts

### Question

What is domain-driven design?

### Short Answer

Domain-driven design is an approach to software design that centers the model around the business domain, its language, and its rules. It is most useful when the problem space is complex enough that careful modeling improves correctness and maintainability.

### Deep Explanation

DDD is not a fixed architecture. It is a way of aligning software structure with domain understanding. Concepts such as ubiquitous language, bounded contexts, entities, value objects, and aggregates help teams model business rules more explicitly. In simple CRUD systems, full DDD may be more ceremony than value.

### Senior-Level Notes

The strongest answers usually include a contrast: "I would not introduce full DDD for a straightforward admin CRUD service, but I would for pricing, ordering, underwriting, or any workflow with heavy invariants." That sounds grounded rather than ideological.

### Common Mistakes

- describing DDD as only layers and repositories
- applying DDD terminology to every project regardless of complexity
- ignoring the language and collaboration aspect

### Question

What is the difference between an entity and a value object?

### Short Answer

An entity has identity that persists across state changes, while a value object is defined only by its attributes and is usually immutable. Entities model things you track over time. Value objects model descriptive concepts.

### Deep Explanation

An `Order` remains the same order even if its status changes, so identity matters. A `Money` or `Address` value object is meaningful through its value, not an independent lifecycle. Modeling these correctly makes code clearer and helps enforce invariants in the right place.

### Senior-Level Notes

A strong answer mentions that many codebases accidentally turn everything into entities because ORMs make identity easy. That often creates unnecessary mutability and weakens the domain model.

### Common Mistakes

- making value objects mutable
- assigning artificial identities to everything
- ignoring equality semantics

### Question

What is an aggregate, and why does it exist?

### Short Answer

An aggregate is a consistency boundary around a cluster of related domain objects that are modified together under one root. It exists to protect invariants and keep transactional updates focused.

### Deep Explanation

The aggregate root is the only external entry point for modifying the aggregate. That helps ensure business rules are enforced consistently. Aggregates are not just object graphs. Their boundaries should be small enough for transactional consistency and large enough to protect real invariants.

### Senior-Level Notes

Senior candidates usually connect aggregate size to operational behavior. If an aggregate is too large, every change loads too much state and conflicts too often. If it is too small, the application starts enforcing invariants across multiple writes and loses its consistency boundary.

### Common Mistakes

- treating the entire domain as one aggregate
- making aggregates mirror database joins
- exposing internal entities directly for mutation

### Question

What is a bounded context?

### Short Answer

A bounded context is an explicit boundary within which a domain model and its language are consistent. The same term can mean different things in different bounded contexts, and that is normal.

### Deep Explanation

Large systems often fail because one shared model tries to serve multiple teams and use cases with conflicting meanings. Bounded contexts allow models to stay cohesive. For example, `Customer` in billing, support, and marketing may overlap in name but differ in responsibilities, data, and rules.

### Senior-Level Notes

A strong answer connects bounded contexts to team ownership, integration design, and service boundaries. They are not only modeling tools but also organizational tools.

### Common Mistakes

- assuming bounded contexts must equal microservices one-to-one
- using one shared entity model across unrelated domains
- confusing technical modules with domain boundaries

## Application Of DDD

### Question

What is the role of a repository in DDD?

### Short Answer

A repository provides an abstraction for loading and persisting aggregates. Its purpose is to support the domain model, not to expose arbitrary database querying capabilities everywhere.

### Deep Explanation

Repositories help keep persistence concerns from spreading through the application. They should align with aggregate access patterns and business use cases. If a repository becomes a generic dump of query methods or returns `IQueryable`, it often stops protecting the domain boundary.

### Senior-Level Notes

The practical senior answer is often: "I want repositories for aggregate lifecycle, not for every reporting query in the system." That keeps DDD from becoming a performance or maintainability tax on read-heavy paths.

### Common Mistakes

- creating generic repositories that add little value over the ORM
- exposing persistence concerns through repository APIs
- forcing every query through the domain model

### Question

What is the difference between an application service and a domain service?

### Short Answer

An application service coordinates use cases, orchestration, and infrastructure interactions. A domain service contains domain logic that does not naturally belong to one entity or value object.

### Deep Explanation

Application services sit closer to the edge of the system. They handle transactions, authorization, integration calls, or workflow coordination. Domain services should remain focused on domain rules. If application services contain all business behavior, the model becomes anemic. If domain services start orchestrating infrastructure, the boundaries blur.

### Senior-Level Notes

A strong answer mentions that many teams overuse domain services. They are useful, but they should be rare compared to behavior living naturally on aggregates and value objects.

### Common Mistakes

- putting all logic into services and leaving entities as data bags
- treating services as interchangeable regardless of responsibility
- mixing domain logic with transport or persistence concerns

### Question

What is an anemic domain model, and why is it a problem?

### Short Answer

An anemic domain model is a model where entities mostly hold data while business rules live elsewhere in procedural service code. It is a problem because invariants become scattered and the model stops expressing business behavior.

### Deep Explanation

When state and behavior are separated too aggressively, every update path must remember the same rules manually. That increases duplication and makes invalid states easier to create. Rich domain models are not about clever object-oriented code. They are about keeping rules near the data they govern.

### Senior-Level Notes

Senior candidates should also mention that not every system needs a rich domain model. If the domain is simple, a heavily modeled approach may just move simple CRUD logic into ceremony-heavy classes.

### Common Mistakes

- assuming every thin entity is automatically bad
- forcing behavior into entities even when the domain is trivial
- mistaking repository or service bloat for DDD

### Question

How does DDD handle integration between bounded contexts?

### Short Answer

DDD favors explicit integration between bounded contexts through contracts, translation layers, events, or APIs rather than sharing one internal model. Each context protects its own language and invariants.

### Deep Explanation

Bounded contexts evolve independently. If they share internal models directly, one team's changes leak into another team's domain. Anti-corruption layers, published events, or stable integration DTOs help isolate those changes. This is especially important during large-system evolution or microservice decomposition.

### Senior-Level Notes

The better answer acknowledges messy reality: once contexts are separate, they will drift temporarily, messages will retry, and repair paths matter. Pretending otherwise is usually how teams end up with brittle cross-context integrations.

### Common Mistakes

- sharing database tables as an integration strategy
- exposing internal domain models directly to other services
- pretending all contexts can stay perfectly synchronized in real time

## Advanced Senior-Level Questions

### Question

How do you keep aggregate boundaries useful as traffic and write contention grow?

### Short Answer

I revisit aggregate boundaries when they start creating unnecessary contention, oversized loads, or awkward transactional behavior. The goal is to keep the aggregate large enough to protect invariants and small enough to support throughput.

### Deep Explanation

Boundaries that looked clean at small scale can become painful when many users or workers modify the same aggregate root. Sometimes the correct move is to shrink the aggregate, move some behavior to separate consistency boundaries, or accept eventual consistency for non-core side effects. That is still DDD, not abandoning DDD.

### Senior-Level Notes

The senior answer usually acknowledges that aggregate design is partly a performance and concurrency decision, not only a modeling purity decision.

### Common Mistakes

- refusing to revisit aggregate boundaries after the workload changes
- protecting non-critical consistency with oversized aggregates
- splitting aggregates so far that true invariants leak outward

### Question

What is the difference between a domain event and an integration event?

### Short Answer

A domain event expresses something meaningful that happened inside a bounded context. An integration event is the external contract another context or service consumes. They are related, but I do not assume they should always be the same object or payload.

### Deep Explanation

Inside a model, domain events help make important state changes explicit. Across boundaries, integration events need stability, versioning discipline, and consumer awareness. Conflating them too early can leak internal model details outside the boundary and make future changes painful.

### Senior-Level Notes

The stronger answer mentions translation. Internal events can be rich and domain-specific; external events should be intentional contracts.

### Common Mistakes

- publishing internal domain events directly to other services
- treating all events as the same abstraction
- ignoring versioning and consumer impact

### Question

When does CQRS fit naturally with DDD, and when does it become unnecessary complexity?

### Short Answer

CQRS fits when the write model and read model genuinely have different shapes, scaling needs, or consistency expectations. It becomes unnecessary when the system is simple enough that one clear model serves both sides without pain.

### Deep Explanation

DDD often makes the write side richer because it protects business rules. Read paths, however, may want flatter, query-optimized models. CQRS helps when that difference is real. If the system is mostly CRUD, separate command and query machinery can add indirection without enough benefit.

### Senior-Level Notes

A strong answer avoids hype. CQRS is not a maturity badge. It is a response to real complexity in reads, writes, or scaling characteristics.

### Common Mistakes

- introducing CQRS because the architecture diagram looks more advanced
- forcing every read through the domain model
- treating CQRS as synonymous with event sourcing

## Related Topics

- [../architecture/ddd.md](../architecture/ddd.md)
- [../architecture/clean-architecture.md](../architecture/clean-architecture.md)
- [../../distributed-systems/interview/microservices.md](../../distributed-systems/interview/microservices.md)
- [../../architecture/interview/architecture.md](../../architecture/interview/architecture.md)
