# Indexing Case Studies

## Problem Context

Index discussions stay abstract until they are attached to workload patterns. The goal is to reason from query shape to index design instead of collecting generic rules.

## Recommended Approach

Review the query, its filters, ordering, and projected columns. Then design indexes around the highest-value workloads rather than trying to optimize every path equally.

## Example Walkthrough

Case 1: a multi-tenant audit table is frequently filtered by `TenantId` and recent time windows. A composite index starting with `TenantId` and then `CreatedAtUtc` is often more useful than separate indexes.

Case 2: an orders table is often queried by `CustomerId` and sorted by `CreatedAtUtc`. A composite index aligned with both operations can remove expensive sorting and improve seek behavior.

## Trade-Offs

Each new index improves some reads but increases storage, insert cost, update cost, and maintenance time. Indexing is therefore a prioritization problem.

## Common Mistakes

- adding indexes from memory without plan review
- keeping old unused indexes forever
- optimizing rarely used admin queries at the cost of hot write paths

## Related Topics

- [../fundamentals/indexing.md](../fundamentals/indexing.md)
- [../fundamentals/query-optimization.md](../fundamentals/query-optimization.md)
