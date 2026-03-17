# High Availability

## Overview

High availability is about keeping a system accessible and useful despite failures, deployments, and infrastructure disruption. It requires more than redundancy. It requires thoughtful failure detection, recovery, and dependency management.

## Core Concepts

- Redundancy, health checks, failover, and capacity headroom support availability.
- Stateless application nodes are easier to replace and scale than nodes with critical in-memory state.
- Dependencies affect availability. A highly available service built on fragile dependencies is still fragile.
- Graceful degradation is often as important as total uptime.

## Why It Matters In Real Systems

Availability incidents rarely come from a single dead server anymore. They come from unhealthy dependencies, cascading retries, partial zone failures, bad deploys, or slow recovery. Good availability design reduces both outage frequency and recovery time.

## Practical Example

Running multiple API instances behind a load balancer improves resilience, but if all of them depend on one saturated database or one external provider without fallback behavior, the actual availability remains limited.

## Common Pitfalls

- equating redundancy with availability
- forgetting dependency and data-store bottlenecks
- making deployments disruptive by default
- ignoring graceful degradation paths

## Interview Takeaways

- Explain availability in terms of system behavior under failure.
- Mention failover, health checks, dependency isolation, and degradation strategies.
- Show that uptime is an outcome of design and operations together.

## Related Topics

- [scalability-engineering.md](scalability-engineering.md)
- [../../distributed-systems/fundamentals/resiliency-patterns.md](../../distributed-systems/fundamentals/resiliency-patterns.md)
- [../examples/rate-limiting.md](../examples/rate-limiting.md)
