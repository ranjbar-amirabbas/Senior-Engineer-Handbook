# Distributed Systems Fundamentals

## Overview

A distributed system is a system whose components communicate over a network and fail independently. That simple fact changes almost every engineering assumption around consistency, latency, ordering, and recovery.

## Core Concepts

- Network calls are slower and less reliable than in-process calls.
- Partial failure is normal: one component can fail while others keep running.
- Time and ordering are less trustworthy across machines than within one process.
- Consistency, availability, and latency trade-offs appear in both architecture and implementation details.

## Why It Matters In Real Systems

Many backend systems become distributed before teams fully model the consequences. A remote call gets treated like a method call, retries create duplicates, or independent systems drift out of sync. Senior engineers need a mental model for failure and recovery, not just a list of patterns.

## Practical Example

An order service calls payment, inventory, and notification services. Even if each service is correct, the overall workflow can still fail at any boundary. The design must decide how to handle timeouts, retries, duplicate messages, partial completion, and observability across services.

## Common Pitfalls

- treating remote calls as reliable local calls
- assuming retries are always safe
- ignoring idempotency and duplicate delivery
- coupling services too tightly through synchronous chains

## Interview Takeaways

- Define distributed systems in terms of independent failure and network boundaries.
- Mention timeouts, retries, observability, and consistency trade-offs.
- Show that reliability is a design concern, not only an infrastructure concern.

## Related Topics

- [microservices.md](microservices.md)
- [consistency-and-reliability.md](consistency-and-reliability.md)
- [resiliency-patterns.md](resiliency-patterns.md)
