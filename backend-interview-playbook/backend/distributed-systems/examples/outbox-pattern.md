# Outbox Pattern

## Problem Context

A service updates its local database and also needs to publish an event. If the database commit succeeds and event publication fails, the system becomes inconsistent.

## Recommended Approach

Write the outgoing event to an outbox table in the same local transaction as the business data, then publish from the outbox asynchronously with retries and observability.

## Example Walkthrough

An order service stores the order row and an `OrderCreated` outbox record together. A background publisher reads pending outbox rows, publishes them to the broker, and marks them as dispatched. If publication fails, the record stays pending and can be retried safely.

## Trade-Offs

The outbox pattern improves reliability, but it adds publisher infrastructure, operational monitoring, and eventual consistency lag between the local commit and downstream consumers.

## Common Mistakes

- publishing directly after commit without durable recovery
- forgetting idempotency on the consumer side
- letting the outbox table grow without maintenance

## Related Topics

- [../fundamentals/consistency-and-reliability.md](../fundamentals/consistency-and-reliability.md)
- [../../databases/fundamentals/transactions-and-isolation-levels.md](../../databases/fundamentals/transactions-and-isolation-levels.md)
