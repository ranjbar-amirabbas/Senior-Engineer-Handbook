# Event Versioning

## Problem Context

Event contracts change over time, but consumers may be deployed independently and may lag behind producers.

## Recommended Approach

Prefer additive changes, preserve compatibility where possible, and manage schema evolution intentionally with clear ownership and rollout expectations.

## Example Walkthrough

If an event originally has `totalAmount` and later needs `currency`, adding the new field is usually safer than renaming or repurposing existing fields immediately. Consumers can adopt the new field gradually while old ones keep working.

## Trade-Offs

Maintaining compatibility slows cleanup and may require dual-read or dual-write periods. Breaking changes are sometimes unavoidable, but they require explicit migration planning.

## Common Mistakes

- changing event meaning without version discipline
- assuming all consumers upgrade together
- forgetting replay behavior when evolving schemas

## Related Topics

- [../fundamentals/event-driven-architecture.md](../fundamentals/event-driven-architecture.md)
- [../interview/kafka.md](../interview/kafka.md)
