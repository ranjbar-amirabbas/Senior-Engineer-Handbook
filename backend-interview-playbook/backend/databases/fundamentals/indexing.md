# Indexing

## Overview

Indexes are one of the most important database performance tools, but they are easy to misuse. An index can drastically improve one query while increasing write cost, storage usage, and maintenance overhead across the system.

## Core Concepts

- Indexes speed up data access by creating structures optimized for lookup and ordering.
- Clustered and nonclustered indexes serve different roles depending on the engine and schema.
- Composite index order matters because it affects which query predicates can use the index effectively.
- Covering indexes can reduce lookups, but wider indexes are also more expensive to maintain.

## Why It Matters In Real Systems

Most backend performance issues eventually involve data access patterns. If engineers add indexes mechanically, they risk optimizing one endpoint while degrading insert-heavy or update-heavy workflows. Indexing is therefore a trade-off, not a checkbox.

## Practical Example

If a query frequently filters by `TenantId` and `CreatedAtUtc` and orders by `CreatedAtUtc`, an index on `(TenantId, CreatedAtUtc)` is often much more useful than separate single-column indexes. The right index matches the query shape.

## Common Pitfalls

- indexing every column used in filtering
- ignoring query patterns when choosing composite order
- forgetting that writes must maintain indexes too
- assuming the ORM hides indexing concerns

## Interview Takeaways

- Explain how indexes help and what they cost.
- Mention selectivity, composite order, and write amplification.
- Show that you think in workloads, not isolated queries.

## Related Topics

- [query-optimization.md](query-optimization.md)
- [sql-server.md](sql-server.md)
- [../examples/indexing-case-studies.md](../examples/indexing-case-studies.md)
