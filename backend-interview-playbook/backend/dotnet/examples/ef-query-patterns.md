# EF Query Patterns

## Problem Context

Teams often start with correct EF Core queries that later become expensive as data volume grows. The common failure pattern is writing entity-heavy queries for read scenarios that only need a narrow output shape.

## Recommended Approach

Prefer explicit projection for read endpoints, use `AsNoTracking()` by default for read-only paths, and inspect generated SQL when the query shape becomes complex. Keep write-oriented aggregate loading separate from read-oriented list and report queries.

## Example Walkthrough

For an order list endpoint:

```csharp
var items = await dbContext.Orders
    .AsNoTracking()
    .Where(x => x.Status == OrderStatus.Paid)
    .OrderByDescending(x => x.CreatedAtUtc)
    .Select(x => new OrderListItemDto(
        x.Id,
        x.Number,
        x.Customer.Name,
        x.Total))
    .ToListAsync(cancellationToken);
```

This avoids loading full entities and limits columns to what the API needs. If the query grows more complex, check the generated SQL and execution plan rather than guessing from C# code alone.

## Trade-Offs

Projection improves read performance and contract clarity, but it can increase mapping code and may require separate query models from the write side. That trade-off is usually worthwhile for high-traffic endpoints.

## Common Mistakes

- calling `ToList()` too early
- serializing tracked entities directly
- using `Include` for every related field instead of shaping the result

## Related Topics

- [../framework/entity-framework.md](../framework/entity-framework.md)
- [../../databases/fundamentals/query-optimization.md](../../databases/fundamentals/query-optimization.md)
- [../interview/entity-framework.md](../interview/entity-framework.md)
