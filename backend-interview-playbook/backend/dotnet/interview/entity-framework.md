# Entity Framework Interview Questions

## Intro

Entity Framework questions test whether you understand persistence behavior beyond CRUD. Strong candidates explain where the abstraction helps, where it leaks, and how they would diagnose problems before blaming the ORM.

## Use This File With

- [../framework/entity-framework.md](../framework/entity-framework.md)
- [../../databases/fundamentals/query-optimization.md](../../databases/fundamentals/query-optimization.md)
- [../examples/ef-query-patterns.md](../examples/ef-query-patterns.md)

## Querying And Performance

### Question

What does change tracking do in EF Core, and when should you disable it?

### Short Answer

Change tracking lets EF Core detect modifications to loaded entities so it can generate updates during `SaveChanges()`. It is useful for write flows, but for read-only queries you should often use `AsNoTracking()` to reduce overhead.

### Deep Explanation

Tracking stores entity instances in the context and maintains original values or snapshots needed for detecting changes. That costs memory and CPU. In high-throughput read scenarios, tracking every entity provides no benefit and can become expensive, especially for large result sets or complex graphs.

### Senior-Level Notes

A strong answer also mentions `AsNoTrackingWithIdentityResolution()` as a middle ground when you want read-only results but still need consistent entity references across a query graph.

### Common Mistakes

- leaving tracking on by default for all queries
- assuming no-tracking always improves every scenario
- keeping contexts alive so long that tracked state grows uncontrollably

### Question

What is the N+1 query problem, and how do you prevent it in EF Core?

### Short Answer

The N+1 problem happens when one query loads a set of parent records and then additional queries are triggered for related data per record, often through lazy loading or repeated lookups. You prevent it with explicit loading strategies, projection, batching, and careful query review.

### Deep Explanation

The issue is not just query count. It is unpredictable latency, database pressure, and poor scaling. In EF Core, projection is often better than blindly using `Include` because it loads exactly the data the use case needs. Logging generated SQL and inspecting query patterns under realistic data volumes are important.

### Senior-Level Notes

The stronger interview answer usually sounds like this: "My first choice is usually projection, not `Include`, because list endpoints rarely need full entity graphs." That framing is practical and signals experience reviewing real slow endpoints.

### Common Mistakes

- enabling lazy loading casually in performance-sensitive services
- replacing one bad query with one even bigger bad query
- reviewing only correctness and not query shape

### Question

When should you project into DTOs instead of returning entities?

### Short Answer

Project into DTOs when you are reading data for output rather than performing domain updates. Projection reduces over-fetching, keeps query translation explicit, and avoids leaking persistence models into API contracts.

### Deep Explanation

Entities are good when the application needs change tracking and behavior around the aggregate. DTO projection is better for read models, list endpoints, dashboards, and reporting queries. It allows the database to return only required columns and prevents accidental coupling between API shape and database shape.

### Senior-Level Notes

This is also an architectural boundary decision. Returning entities from application services often invites accidental serialization of navigation properties and hidden lazy-loading problems.

### Common Mistakes

- using entities as API contracts
- selecting entire graphs for simple list endpoints
- mapping after materialization when projection could happen in SQL

### Question

How do you diagnose a slow EF Core query?

### Short Answer

Start by inspecting the generated SQL, execution plan, row counts, and indexes. Then check whether the problem is query translation, over-fetching, tracking overhead, poor schema design, or the database doing expensive work.

### Deep Explanation

EF Core is often blamed for problems that actually come from data volume, missing indexes, bad joins, or premature materialization. Good diagnosis means separating ORM overhead from database cost. You should verify whether filters are applied server-side, whether includes exploded the row count, and whether the shape can be rewritten using projection or split queries.

### Senior-Level Notes

Senior candidates usually describe an order of operations: inspect generated SQL, inspect the plan, check data volume, then decide whether the fix belongs in EF, the schema, or the query shape. That sounds far more realistic than "I would optimize the LINQ."

### Common Mistakes

- blaming the ORM before reading the SQL
- optimizing code paths without checking execution plans
- testing only with tiny local datasets

## Lifetime And Consistency

### Question

Why is `DbContext` usually registered as scoped?

### Short Answer

`DbContext` is usually scoped because it represents a unit of work aligned with a request or logical operation. It is not thread-safe and should not be shared across unrelated concurrent work.

### Deep Explanation

A scoped lifetime keeps tracked state bounded, enables transactional behavior within a request, and avoids cross-request state leakage. Sharing a context as a singleton causes concurrency issues and stale tracked entities. Creating too many disconnected contexts inside one operation can also break consistency and make transactions harder.

### Senior-Level Notes

A strong answer notes that background jobs and message consumers may create their own scopes per message or job, even though they are outside HTTP request handling.

### Common Mistakes

- registering `DbContext` as singleton
- sharing one context across multiple parallel operations
- creating contexts ad hoc without clear unit-of-work boundaries

### Question

How does EF Core handle optimistic concurrency?

### Short Answer

EF Core supports optimistic concurrency by comparing a concurrency token, such as a row version, between the loaded entity and the current database state during update. If the values differ, EF Core throws a concurrency exception instead of silently overwriting changes.

### Deep Explanation

Optimistic concurrency assumes collisions are less common than reads and writes. It works well in most business systems because it avoids heavy locking while still protecting against lost updates. The application then decides whether to retry, merge, or reject the operation depending on the use case.

### Senior-Level Notes

Senior candidates usually mention that optimistic concurrency is not a complete consistency strategy. You still need proper transactional boundaries and domain rules for invariants that span multiple entities or operations.

### Common Mistakes

- confusing optimistic concurrency with transaction isolation
- retrying automatically without understanding business impact
- ignoring conflict resolution strategy

### Question

Should you wrap every `SaveChanges()` call in an explicit transaction?

### Short Answer

Not necessarily. EF Core already wraps a single `SaveChanges()` call in a transaction for relational providers. Explicit transactions are useful when multiple operations must succeed or fail together.

### Deep Explanation

For a single atomic persistence step, the built-in transaction is usually enough. Explicit transactions become relevant when several `SaveChanges()` calls, raw SQL commands, or cross-repository operations must share one boundary. Overusing manual transactions increases complexity and can hold locks longer than needed.

### Senior-Level Notes

A strong answer distinguishes local database transactions from distributed consistency. Once multiple services or brokers are involved, patterns such as outbox or compensating actions become more realistic than distributed transactions.

### Common Mistakes

- adding explicit transactions everywhere by habit
- assuming one database transaction can guarantee consistency across services
- holding transactions open across slow network calls

### Question

How should EF Core migrations be used in team environments?

### Short Answer

Migrations should be treated as versioned schema changes that evolve with application code. Teams should review generated SQL, keep migrations small and intentional, and plan operationally for breaking data changes.

### Deep Explanation

Migrations are not only a developer convenience. They are part of deployment. Renames, large backfills, default value changes, and index rebuilds can have production impact. Mature teams separate schema evolution from risky data movement when needed and think about backward compatibility during rolling deployments.

### Senior-Level Notes

The best answers mention release choreography: additive migration first, code rollout second, cleanup later. That is the level where EF stops being a developer convenience and becomes part of production delivery engineering.

### Common Mistakes

- trusting generated migrations blindly
- mixing destructive schema changes with the same deploy that still runs old code
- ignoring rollback complexity

## Advanced Senior-Level Questions

### Question

When would you use split queries versus single queries in EF Core?

### Short Answer

I consider split queries when a single query with multiple includes would create a large cartesian explosion or duplicate too much data. I stay with a single query when consistency of one round trip and simpler execution shape matter more.

### Deep Explanation

Single queries reduce round trips, but complex joins can multiply rows and inflate memory use dramatically. Split queries can reduce that explosion by loading related data separately, but they also add extra round trips and can change transactional assumptions if you are not careful. The right choice depends on graph size, latency profile, and read pattern.

### Senior-Level Notes

A strong answer mentions that this is a query-shape decision, not an ideology. I would validate it against actual SQL, row counts, and realistic data volume.

### Common Mistakes

- assuming one mode is always better
- applying split queries without checking round-trip cost
- using includes when projection would remove the problem entirely

### Question

When do compiled queries or bulk operations matter in EF Core?

### Short Answer

They matter in high-frequency paths where query compilation overhead or row-by-row update behavior is measurable. I do not reach for them first, but they are reasonable once the normal query path is already clean and the workload justifies the extra specificity.

### Deep Explanation

Compiled queries can help when the same query shape is executed repeatedly at scale. Bulk-style operations such as set-based updates and deletes matter when loading entities only to update them one by one becomes wasteful. Both are optimizations that trade some simplicity for throughput and lower overhead.

### Senior-Level Notes

The stronger answer usually includes restraint: if the query is rare or the real bottleneck is indexing, compiled queries will not save you.

### Common Mistakes

- reaching for advanced EF features before fixing schema and query shape
- optimizing cold paths
- forgetting that set-based operations bypass some entity-level behavior

### Question

How do you handle high write contention with EF Core?

### Short Answer

I start by identifying whether the contention is really in the application model, the transaction scope, or the underlying table and index design. Then I use the right combination of smaller transactions, optimistic concurrency, queueing, or data-model changes instead of assuming EF is the root cause.

### Deep Explanation

Hot rows, long-running transactions, and wide updates can create painful contention regardless of ORM. EF Core can participate in the problem through tracking scope or update shape, but the real fix may be reducing write overlap, reshaping the workflow, or changing how the data is partitioned or versioned.

### Senior-Level Notes

The senior answer makes it clear that contention is a system problem, not only a persistence-library problem.

### Common Mistakes

- blaming EF Core without checking the database behavior
- retrying write conflicts blindly
- holding transactions open across unrelated work

## Related Topics

- [../framework/entity-framework.md](../framework/entity-framework.md)
- [../../databases/interview/sql.md](../../databases/interview/sql.md)
- [../../databases/fundamentals/indexing.md](../../databases/fundamentals/indexing.md)
- [../examples/ef-query-patterns.md](../examples/ef-query-patterns.md)
