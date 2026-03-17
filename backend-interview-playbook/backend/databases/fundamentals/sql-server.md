# SQL Server

## Overview

SQL Server is a common relational database for .NET backends. Senior backend engineers do not need to be full-time DBAs, but they should understand how SQL Server behavior affects schema changes, indexing, transactions, and performance tuning.

## Core Concepts

- Execution plans explain how SQL Server actually runs a query.
- Indexes improve some workloads and hurt others through write cost and maintenance overhead.
- Locking, blocking, and isolation level choices influence concurrency behavior.
- Statistics, parameter sensitivity, and row count estimation affect query performance.

## Why It Matters In Real Systems

A backend can look healthy in code while the real problem lives in SQL Server: bad plans, missing indexes, blocking chains, or poorly sequenced schema changes. Engineers who can read the database signals diagnose problems faster and make safer changes.

## Practical Example

A query may be written correctly in C# and still perform poorly because the SQL plan scans a large table. Reviewing the actual execution plan may show a missing composite index or a predicate that prevents index use. The application fix then becomes targeted rather than speculative.

## Common Pitfalls

- treating the database as a black box behind the ORM
- adding indexes without considering write amplification
- ignoring execution plans and tuning only at the application layer
- making large schema changes without rollout planning

## Interview Takeaways

- Mention execution plans, locking, indexes, and operational caution.
- Show that you know the database is part of the system, not just storage.
- Focus on behavior under load, not vendor trivia.

## Related Topics

- [indexing.md](indexing.md)
- [query-optimization.md](query-optimization.md)
- [transactions-and-isolation-levels.md](transactions-and-isolation-levels.md)
