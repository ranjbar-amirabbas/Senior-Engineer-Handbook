# Consistency And Reliability

## Overview

Consistency and reliability are central concerns in distributed systems because work spans networks, processes, and independently failing components. Systems must be designed around the reality that confirmation, ordering, and completion are often imperfect.

## Core Concepts

- Strong consistency is useful but expensive and often limited to small boundaries.
- Eventual consistency accepts temporary divergence in exchange for availability and decoupling.
- Reliability depends on retries, idempotency, timeouts, backoff, and observability working together.
- Exactly-once business outcomes usually require application-level design, not just broker-level features.

## Why It Matters In Real Systems

Many production incidents come from duplicate work, missing side effects, stale reads, or retry storms. These problems usually reflect weak consistency and reliability design rather than a single broken library.

## Practical Example

An order may be stored successfully while the event publication fails. Without an outbox pattern or equivalent design, downstream services never learn about the order. Reliability requires treating persistence and message publication as one logical workflow even when the infrastructure cannot commit both atomically.

## Common Pitfalls

- assuming retries are always safe
- promising exactly-once behavior without end-to-end design
- ignoring reconciliation and repair paths
- building synchronous chains for workflows that tolerate eventual consistency

## Interview Takeaways

- Distinguish local correctness from distributed correctness.
- Mention idempotency, retries, outbox, and reconciliation.
- Show that reliability comes from multiple layers working together.

## Related Topics

- [resiliency-patterns.md](resiliency-patterns.md)
- [../examples/outbox-pattern.md](../examples/outbox-pattern.md)
- [../examples/saga-vs-orchestration.md](../examples/saga-vs-orchestration.md)
