# Domain-Driven Design

## Overview

Domain-driven design helps teams model complex business behavior explicitly. It is most valuable when the system contains rich rules, conflicting language across domains, or workflows where correctness depends on preserving business invariants.

## Core Concepts

- Ubiquitous language keeps developers and domain experts aligned on terminology.
- Entities, value objects, aggregates, repositories, and domain services are modeling tools, not mandatory layers.
- Bounded contexts separate parts of the system that need different models and language.
- DDD is strongest when it clarifies complexity, not when it adds ritual to simple CRUD systems.

## Why It Matters In Real Systems

Without a deliberate model, business rules spread across controllers, services, handlers, and database scripts. That makes change expensive and errors easier to introduce. DDD provides a way to keep rules close to the concepts they belong to and avoid one giant shared model for the whole company.

## Practical Example

In an ordering domain, `Order` may be the aggregate root. It controls line items, status transitions, and payment-related invariants. Billing and fulfillment may each have their own bounded contexts with different views of the same business process, rather than sharing one all-purpose `Order` type everywhere.

## Common Pitfalls

- treating DDD as synonymous with layered architecture
- forcing every project into aggregates and repositories when the domain is simple
- creating oversized aggregates that destroy concurrency and throughput
- using the same model across multiple bounded contexts

## Interview Takeaways

- Explain where DDD fits and where it does not.
- Focus on language, boundaries, and invariants.
- Mention that DDD is about modeling choices, not a fixed folder structure.

## Related Topics

- [clean-architecture.md](clean-architecture.md)
- [design-patterns.md](design-patterns.md)
- [../interview/ddd.md](../interview/ddd.md)
- [../../distributed-systems/fundamentals/microservices.md](../../distributed-systems/fundamentals/microservices.md)
