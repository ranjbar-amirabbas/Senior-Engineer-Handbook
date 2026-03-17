# Event-Driven Architecture

## Overview

Event-driven architecture uses events to communicate that something meaningful happened in the system. It is useful when producers and consumers should be decoupled in time, ownership, or rate of change.

## Core Concepts

- Events represent facts that already happened, not commands disguised with different wording.
- Asynchronous communication reduces temporal coupling but introduces delivery, ordering, and consistency concerns.
- Event contracts must evolve carefully because many consumers may depend on them.
- Event-driven systems work best when consumers are idempotent and observable.

## Why It Matters In Real Systems

Event-driven designs can improve scalability and decouple services, but they make debugging and correctness harder if the team ignores eventual consistency and duplicate delivery. The complexity moves from direct invocation to coordination and observability.

## Practical Example

After an order is paid, the order service emits an `OrderPaid` event. Inventory, fulfillment, and analytics consume it independently. This reduces synchronous coupling, but each consumer must handle retries, lag, and version compatibility.

## Common Pitfalls

- publishing events without a clear ownership model
- using events where synchronous confirmation is actually required
- confusing commands and events
- ignoring schema evolution and replay scenarios

## Interview Takeaways

- Explain the decoupling benefits and the consistency cost.
- Mention idempotency, retries, lag, and contract evolution.
- Show that event-driven architecture is a coordination model, not only a broker choice.

## Related Topics

- [kafka.md](kafka.md)
- [consistency-and-reliability.md](consistency-and-reliability.md)
- [../examples/event-versioning.md](../examples/event-versioning.md)
