# Distributed Systems Interview Questions

## Intro

These questions test whether you understand what changes when systems cross process and network boundaries.

## Core Concepts

### Question

Why are distributed systems harder than single-process systems?

### Short Answer

Because components communicate over unreliable networks and fail independently. Latency, timeouts, partial failure, ordering, and coordination all become first-class design problems.

### Deep Explanation

In a local process, method calls are fast and failure boundaries are limited. In distributed systems, every network call adds variability and failure modes. The design must tolerate retries, duplicate work, and inconsistent views of system state.

### Senior-Level Notes

Strong answers mention observability and recovery, not only theory.

### Common Mistakes

- reducing the answer to "network is slower"

### Question

What is eventual consistency?

### Short Answer

Eventual consistency means different parts of the system may temporarily disagree but converge over time if no new updates occur. It is a practical trade-off in many distributed designs.

### Deep Explanation

It appears when services own separate data stores or communicate asynchronously. The system accepts temporary lag in exchange for autonomy, availability, or scalability.

### Senior-Level Notes

Senior candidates mention where eventual consistency is acceptable and where user experience or business rules require stronger guarantees.

### Common Mistakes

- using eventual consistency as an excuse for undefined behavior

### Question

Why are retries dangerous?

### Short Answer

Retries can duplicate side effects, amplify load, and worsen failures if they are applied without idempotency, limits, and backoff.

### Deep Explanation

Retries are helpful for transient failures, but they also change traffic patterns. If the downstream dependency is overloaded, aggressive retries can turn a temporary slowdown into a system-wide incident.

### Senior-Level Notes

Strong answers mention jitter, retry budgets, and idempotent operation design.

### Common Mistakes

- saying "just retry" without constraints

### Question

What is idempotency in distributed workflows?

### Short Answer

Idempotency means repeated execution of the same logical operation produces the same effective result. It is critical when timeouts, retries, or duplicate message delivery can occur.

### Deep Explanation

A payment charge, shipment creation, or event consumption path should be designed so the same request key or message does not produce duplicate real-world effects. This often requires durable deduplication or unique business keys.

### Senior-Level Notes

Senior candidates distinguish transport-level deduplication from business-level correctness.

### Common Mistakes

- assuming broker features alone make the workflow idempotent
