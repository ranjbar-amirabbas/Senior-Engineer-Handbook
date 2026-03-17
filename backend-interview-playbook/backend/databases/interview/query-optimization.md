# Query Optimization Interview Questions

## Intro

These questions focus on how you investigate and improve database performance systematically.

## Tuning

### Question

What is the first thing you do when a query is slow?

### Short Answer

Read the actual query and execution plan before changing code. Optimization starts with evidence.

### Deep Explanation

Without the plan, you do not know whether the problem is scanning, joining, sorting, blocking, or simply returning too much data. The plan anchors the next step.

### Senior-Level Notes

Senior answers emphasize production-like data and parameter values.

### Common Mistakes

- tuning from the ORM side only

### Question

Why can `SELECT *` be harmful?

### Short Answer

It increases I/O, memory use, network cost, and coupling to schema shape. It often retrieves far more data than the caller needs.

### Deep Explanation

Extra columns can also prevent narrow covering indexes from being effective. Explicit projection is usually better for both clarity and performance.

### Senior-Level Notes

Strong candidates mention API contracts and ORM projection patterns.

### Common Mistakes

- treating `SELECT *` as harmless in production paths

### Question

How does pagination affect performance?

### Short Answer

Pagination limits result size, which reduces memory and network cost, but some pagination strategies degrade at high offsets. The right approach depends on access pattern and ordering guarantees.

### Deep Explanation

Offset-based pagination is simple but can become expensive for deep pages. Keyset or cursor pagination often performs better when stable ordering exists.

### Senior-Level Notes

Senior answers connect pagination to UX requirements, consistency, and index design.

### Common Mistakes

- using offsets for large result navigation without measurement

### Question

When does caching beat query optimization?

### Short Answer

Caching helps when the same expensive read is repeated often enough that recomputation is wasteful. It does not replace fixing fundamentally bad query patterns or broken data access design.

### Deep Explanation

Caches reduce load and latency, but they introduce invalidation complexity and consistency trade-offs. Optimize the query first when it is structurally wrong. Cache when repeated access justifies it.

### Senior-Level Notes

Strong answers mention cache hit ratio, invalidation, and read staleness.

### Common Mistakes

- using caching to hide every slow query
