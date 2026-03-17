# Entity Framework

## Overview

Entity Framework Core is the default ORM for many .NET applications. It can be productive and powerful, but only when engineers understand how queries are translated, how tracking works, and where the abstraction leaks.

## Core Concepts

- `DbContext` represents a unit of work and is not thread-safe.
- LINQ queries against EF Core are translated by a provider and executed remotely.
- Tracking and no-tracking queries have different costs and use cases.
- Migrations, concurrency tokens, and transactions are part of production behavior, not just development workflow.

## Why It Matters In Real Systems

EF Core mistakes often look harmless in code review and expensive in production. Premature materialization, careless `Include` usage, long-lived contexts, and weak transaction boundaries become performance and correctness problems under real traffic.

## Practical Example

For a list endpoint, projection is usually better than loading whole entities. That is especially true when the endpoint returns a read model and will never call `SaveChanges()` on the results:

```csharp
var orders = await dbContext.Orders
    .AsNoTracking()
    .Where(x => x.Status == OrderStatus.Paid)
    .OrderByDescending(x => x.CreatedAtUtc)
    .Select(x => new OrderListItemDto(x.Id, x.Number, x.Total))
    .ToListAsync(cancellationToken);
```

This keeps the query explicit, reduces memory usage, and avoids dragging unnecessary entity graphs into the application. It also makes it easier to review the generated SQL and reason about which columns the endpoint actually needs.

## Common Pitfalls

- exposing `IQueryable` across architectural boundaries
- using tracked entities for read-only endpoints
- assuming the ORM can compensate for poor schema and indexing choices
- trying to solve every read use case with aggregate-shaped entity loading
- treating migrations as harmless generated code

## Interview Takeaways

- Talk about SQL generation, query shape, and lifecycle boundaries.
- Mention N+1 problems, over-fetching, and concurrency handling.
- Make it clear that EF Core is a tool with trade-offs, not magic.

## Related Topics

- [../interview/entity-framework.md](../interview/entity-framework.md)
- [../../databases/fundamentals/indexing.md](../../databases/fundamentals/indexing.md)
- [../../databases/fundamentals/query-optimization.md](../../databases/fundamentals/query-optimization.md)
- [../examples/ef-query-patterns.md](../examples/ef-query-patterns.md)
