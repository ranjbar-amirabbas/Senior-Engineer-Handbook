# API Versioning

## Problem Context

Public or widely used internal APIs eventually change. Without a versioning strategy, teams either break clients or freeze the contract indefinitely.

## Recommended Approach

Version APIs deliberately when compatibility matters. Favor additive evolution first, keep old versions stable for a defined period, and separate versioning strategy from casual endpoint sprawl.

## Example Walkthrough

Suppose `v1` returns `fullName` while a new design wants structured `firstName` and `lastName`. A safe approach is to introduce `v2` with the new shape, maintain `v1` until clients migrate, and track version usage so deprecation is evidence-based rather than assumed.

## Trade-Offs

Supporting multiple versions increases maintenance cost, testing scope, and documentation burden. Not versioning increases coordination cost with consumers and makes safe evolution harder. The right choice depends on contract stability requirements and client control.

## Common Mistakes

- versioning every minor internal change automatically
- leaving old versions undocumented and unmonitored
- changing semantics without changing the contract version

## Related Topics

- [../../architecture/fundamentals/rest-api-design.md](../../architecture/fundamentals/rest-api-design.md)
- [../../architecture/examples/api-idempotency.md](../../architecture/examples/api-idempotency.md)
- [../framework/aspnet-core.md](../framework/aspnet-core.md)
