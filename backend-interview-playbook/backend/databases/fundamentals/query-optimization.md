# Query Optimization

## Overview

Query optimization is the process of reducing the cost of data retrieval and modification by improving query shape, indexing, schema alignment, and execution characteristics. Good optimization starts with measurement, not guesswork.

## Core Concepts

- Read the actual query and the execution plan before changing code.
- Query cost depends on row counts, joins, sorts, filters, and index usage.
- Data volume and skew change behavior significantly compared with local development datasets.
- Optimization often means reducing work, not just running the same work faster.

## Why It Matters In Real Systems

Slow queries create tail latency, timeouts, blocking, and infrastructure waste. They can also cascade into connection pool pressure and unstable application behavior. Teams that optimize only in application code often miss the real source of cost.

## Practical Example

A search endpoint may time out because it loads too many rows, sorts on a non-indexed expression, and projects more columns than needed. A realistic review sequence is:

1. capture the exact SQL and parameters
2. inspect the actual execution plan
3. confirm whether the query is scanning, sorting, or spilling
4. reduce the result shape or add the index that matches the query pattern

That sequence is usually more effective than tweaking application code first.

## Common Pitfalls

- optimizing without production-like data volume
- focusing on ORM syntax instead of SQL behavior
- adding indexes without understanding the execution plan
- changing several things at once and losing the causal signal
- overlooking pagination and result-size controls

## Interview Takeaways

- Start with diagnosis: SQL text, plan, row counts, indexes.
- Mention over-fetching, cardinality, and realistic workload testing.
- Show that performance tuning is iterative and evidence-driven.

## Related Topics

- [indexing.md](indexing.md)
- [transactions-and-isolation-levels.md](transactions-and-isolation-levels.md)
- [../interview/query-optimization.md](../interview/query-optimization.md)
