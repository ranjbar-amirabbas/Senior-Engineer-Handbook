# Microservices Interview Questions

## Intro

Microservices interviews test whether you can reason about boundaries, autonomy, and failure, not whether you reflexively prefer distributed systems over monoliths. Strong answers sound selective and pragmatic, not enthusiastic by default.

## Use This File With

- [../fundamentals/microservices.md](../fundamentals/microservices.md)
- [../fundamentals/consistency-and-reliability.md](../fundamentals/consistency-and-reliability.md)
- [../examples/outbox-pattern.md](../examples/outbox-pattern.md)

## Service Boundaries

### Question

When do microservices make sense?

### Short Answer

Microservices make sense when the system has enough domain separation, scale, or team autonomy needs that independent deployment and ownership create more value than the operational complexity they introduce.

### Deep Explanation

They are most useful when different parts of the system evolve at different speeds, require distinct operational profiles, or are owned by separate teams. They are much less compelling when the main problem is just code organization inside one small product.

### Senior-Level Notes

The strongest answer usually includes a counterexample: "If one team owns the product and the workflows are tightly coupled, I would rather keep a modular monolith." That shows judgment instead of ideology.

### Common Mistakes

- presenting microservices as the default end state
- ignoring platform and observability maturity

### Question

What are the main disadvantages of microservices?

### Short Answer

They increase operational overhead, deployment complexity, debugging difficulty, and consistency challenges. A distributed system is harder to reason about than a well-structured monolith.

### Deep Explanation

You get more moving parts, more contracts, more pipelines, more network failure modes, and more observability demands. Data consistency becomes harder because transactions rarely span services cleanly.

### Senior-Level Notes

A strong answer mentions that poorly bounded microservices can be worse than a modular monolith because they preserve coupling while adding network cost.

### Common Mistakes

- discussing only scaling advantages
- ignoring developer productivity costs

### Question

What is a distributed monolith?

### Short Answer

A distributed monolith is a system split into multiple services that still depend on each other so tightly that they cannot change, deploy, or fail independently in practice.

### Deep Explanation

This often happens when services share databases, require synchronous cross-calls for every request, or coordinate releases closely. The result is distributed-system complexity without real autonomy.

### Senior-Level Notes

Senior candidates mention that service boundaries should reduce coupling, not just move it over the network.

### Common Mistakes

- assuming any service split is an architectural improvement

### Question

How do you choose service boundaries?

### Short Answer

Choose boundaries around business capabilities, ownership, and change patterns rather than around CRUD entities or technical layers alone.

### Deep Explanation

Good boundaries minimize cross-service coordination for core workflows and align with language used by the business. They should also support coherent data ownership. If every business action crosses multiple service boundaries, the system is probably sliced poorly.

### Senior-Level Notes

Senior answers usually include a warning sign: if every user action requires synchronous calls across several services, the boundary is probably serving the diagram more than the business.

### Common Mistakes

- splitting by technical concern like "user service", "validation service", "email service" without domain clarity

## Data And Communication

### Question

Should microservices share a database?

### Short Answer

Generally no, because shared databases undermine service autonomy and create hidden coupling. Each service should own its data and expose integration through APIs or events.

### Deep Explanation

Shared databases make schema changes harder, blur ownership, and encourage bypassing service contracts. That said, migrations from a monolith may pass through intermediate states, but the target should still be explicit ownership boundaries.

### Senior-Level Notes

Senior candidates usually acknowledge transitional reality while still defending the architectural principle.

### Common Mistakes

- treating shared database access as harmless because it is convenient

### Question

How do microservices maintain consistency without distributed transactions?

### Short Answer

They usually rely on local transactions within each service, asynchronous messaging, idempotent consumers, and compensating actions or reconciliation where necessary.

### Deep Explanation

The system stops trying to make one atomic commit across all participants. Instead, it coordinates through messages, outbox patterns, sagas, and business-aware recovery logic. This accepts eventual consistency while keeping services independently operable.

### Senior-Level Notes

The more realistic answer says what replaces the missing transaction: local commit plus message publication discipline, idempotent consumers, compensations where needed, and enough telemetry to detect drift.

### Common Mistakes

- assuming eventual consistency means no need for recovery design

### Question

When should communication be synchronous versus asynchronous?

### Short Answer

Use synchronous communication when the caller needs an immediate answer to proceed. Use asynchronous communication when decoupling, buffering, or eventual consistency is acceptable or desirable.

### Deep Explanation

Synchronous calls are simpler for request-response flows but increase temporal coupling and failure propagation. Asynchronous messaging improves resilience and autonomy but adds complexity in coordination, observability, and user feedback.

### Senior-Level Notes

Senior candidates mention that one workflow often uses both patterns at different steps.

### Common Mistakes

- assuming asynchronous is always more scalable and therefore always better

### Question

How do you debug an issue across multiple microservices?

### Short Answer

Use correlation IDs, distributed tracing, structured logs, metrics, and a clear understanding of request or event flow across service boundaries.

### Deep Explanation

In a distributed environment, logs from one service rarely tell the whole story. You need shared identifiers, trace spans, and stable contracts so you can reconstruct the path of a request or event through the system.

### Senior-Level Notes

In a senior interview, this answer gets stronger if you mention what you would actually open first: trace view, correlated logs, dependency latency, queue lag, and recent deploy history. That sounds like incident experience, not theory.

### Common Mistakes

- relying on manual log searching without correlation data

## Advanced Senior-Level Questions

### Question

How do you keep one failing service from taking down the rest of a microservice system?

### Short Answer

I try to stop failure propagation early with timeouts, bounded retries, fallbacks where appropriate, and by keeping non-critical work off the synchronous user path. The goal is failure isolation, not pretending dependencies never fail.

### Deep Explanation

In tightly coupled systems, one slow dependency can exhaust request threads, connection pools, or worker capacity across multiple services. Good microservice design limits that blast radius by controlling fan-out, degrading gracefully, and pushing side effects into asynchronous flows when the business can tolerate it.

### Senior-Level Notes

The strongest answer usually includes a concrete example, such as letting checkout continue while deferring analytics or notifications, instead of speaking only in general resiliency terms.

### Common Mistakes

- retrying every downstream call on the hot path
- assuming service boundaries automatically create fault isolation
- designing no degraded mode for non-critical dependencies

### Question

When is an API gateway or BFF useful, and when does it become another bottleneck?

### Short Answer

It is useful when clients need aggregation, centralized auth concerns, or channel-specific shaping. It becomes a bottleneck when it turns into a giant orchestration layer that knows too much about every service.

### Deep Explanation

Gateways can simplify clients and centralize cross-cutting policy. They become dangerous when business workflows drift upward into the gateway and create a new shared dependency that every team must coordinate through. Then you have simply moved coupling to a different box.

### Senior-Level Notes

The stronger answer mentions ownership. A gateway is healthiest when it stays thin and focused on edge concerns.

### Common Mistakes

- putting core business orchestration into the gateway
- using the gateway to hide poor service boundaries
- assuming one global gateway must serve every use case

### Question

How do you approach decomposing a monolith into microservices safely?

### Short Answer

I decompose incrementally around clear business seams, starting with boundaries that reduce coordination pain or deployment friction. I do not begin by extracting services just because the monolith is large.

### Deep Explanation

Safe decomposition usually means understanding the domain, the integration patterns, and the operational model before moving code. The first extraction should create a cleaner ownership boundary and a manageable contract. If it creates more synchronous coupling than it removes, it was probably the wrong first cut.

### Senior-Level Notes

The senior answer mentions migration mechanics: strangler patterns, compatibility periods, observability, and explicit criteria for whether the extraction actually improved the system.

### Common Mistakes

- extracting low-value services first because they seem easy
- moving code without a clear data-ownership plan
- measuring success by service count instead of reduced coupling

## Related Topics

- [../fundamentals/microservices.md](../fundamentals/microservices.md)
- [../fundamentals/event-driven-architecture.md](../fundamentals/event-driven-architecture.md)
- [../examples/outbox-pattern.md](../examples/outbox-pattern.md)
- [../../architecture/fundamentals/monolith-vs-microservices.md](../../architecture/fundamentals/monolith-vs-microservices.md)
