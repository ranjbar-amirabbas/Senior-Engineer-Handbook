# Background Jobs

## Problem Context

Backend systems often need work that should not run on the request path: sending emails, processing reports, expiring sessions, or synchronizing external systems. The danger is treating all background work as simple fire-and-forget code.

## Recommended Approach

Use in-process hosted services only for small, well-bounded background work tied to the application lifecycle. For durable or business-critical workflows, prefer a queue-backed worker model with retries, idempotency, and clear observability.

## Example Walkthrough

An ASP.NET Core service can use `BackgroundService` for periodic cache refreshes or cleanup work. Each iteration should create a scope for scoped dependencies, respect cancellation tokens, and record useful telemetry. If the process dies mid-work, the system should tolerate that or move to a more durable architecture.

## Trade-Offs

Hosted services are simple to deploy because they live with the application. They are weaker for high-volume or must-not-lose workflows because process restarts and scaling events complicate durability and coordination.

## Common Mistakes

- starting fire-and-forget tasks from controllers
- capturing scoped services directly in singleton hosted services
- using in-process jobs for workflows that require durable guarantees

## Related Topics

- [../fundamentals/async-await.md](../fundamentals/async-await.md)
- [../framework/aspnet-core.md](../framework/aspnet-core.md)
- [../../distributed-systems/examples/outbox-pattern.md](../../distributed-systems/examples/outbox-pattern.md)
