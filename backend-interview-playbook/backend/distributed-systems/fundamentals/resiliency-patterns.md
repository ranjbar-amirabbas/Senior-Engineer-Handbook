# Resiliency Patterns

## Overview

Resiliency patterns are techniques for handling failures gracefully in distributed systems. They do not eliminate failure. They reduce the blast radius and help the system recover predictably.

## Core Concepts

- Timeouts prevent resources from waiting indefinitely.
- Retries can recover transient failures but must be bounded and idempotent-safe.
- Circuit breakers stop the system from repeatedly calling a failing dependency.
- Bulkheads, rate limits, fallback behavior, and backpressure protect the system under stress.

## Why It Matters In Real Systems

Without resiliency controls, one failing dependency can consume threads, exhaust connection pools, trigger retry storms, and cascade through the system. Reliability is often more about controlled failure behavior than normal-case speed.

## Practical Example

If a payment provider starts timing out, the application should time out calls quickly enough, avoid unbounded retries, surface the failure clearly, and protect unrelated requests from being starved by the failing path.

## Common Pitfalls

- adding retries without jitter or caps
- retrying non-idempotent operations blindly
- using circuit breakers as a substitute for root-cause analysis
- forgetting observability around failure modes

## Interview Takeaways

- Explain which failure mode each pattern addresses.
- Mention that patterns interact and can make things worse if misconfigured.
- Connect resiliency to user impact and operational stability.

## Related Topics

- [consistency-and-reliability.md](consistency-and-reliability.md)
- [../../architecture/fundamentals/high-availability.md](../../architecture/fundamentals/high-availability.md)
- [../interview/scalability-and-reliability.md](../interview/scalability-and-reliability.md)
