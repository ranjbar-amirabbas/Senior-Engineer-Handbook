# Microservices

## Overview

Microservices are an architectural approach where independently deployable services own specific business capabilities. They can improve autonomy and scalability, but they also introduce operational and integration complexity.

## Core Concepts

- Service boundaries should follow business capabilities and ownership, not arbitrary technical slicing.
- Independent deployment is valuable only when teams can also own operations, observability, and contracts.
- Data ownership should stay within service boundaries rather than being shared casually across databases.
- Synchronous and asynchronous communication choices affect coupling and failure propagation.

## Why It Matters In Real Systems

Microservices can help large organizations move faster, but they can also multiply failure modes, deployment overhead, and debugging difficulty. Senior engineers should understand that microservices are not a default upgrade from a monolith.

## Practical Example

Splitting a monolith into order, catalog, payment, and shipping services may improve team autonomy. But if checkout still requires a long synchronous chain across all four services, you have probably moved coupling onto the network rather than removing it. In that case, a modular monolith or a smaller number of clearer service boundaries may be the better architecture.

## Common Pitfalls

- splitting too early before clear service boundaries exist
- sharing one database across services
- replacing one modular monolith with a distributed monolith
- using asynchronous messaging without clear ownership or observability
- underestimating observability and platform requirements

## Interview Takeaways

- Explain both the benefits and the operational cost.
- Focus on boundaries, ownership, and failure handling.
- Make it clear that monolith versus microservices is a context decision.

## Related Topics

- [event-driven-architecture.md](event-driven-architecture.md)
- [consistency-and-reliability.md](consistency-and-reliability.md)
- [../../architecture/fundamentals/monolith-vs-microservices.md](../../architecture/fundamentals/monolith-vs-microservices.md)
