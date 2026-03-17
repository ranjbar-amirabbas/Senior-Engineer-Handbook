# SQL Interview Questions

## Intro

SQL interviews usually test whether you can reason about query behavior, not only write syntax from memory. Senior candidates are expected to connect SQL choices to indexing, execution plans, data modeling, and concurrency in a way that sounds like they have debugged real production queries.

## Use This File With

- [../fundamentals/query-optimization.md](../fundamentals/query-optimization.md)
- [../fundamentals/indexing.md](../fundamentals/indexing.md)
- [../examples/sql-performance-checklist.md](../examples/sql-performance-checklist.md)

## Query Fundamentals

### Question

What is the difference between `WHERE` and `HAVING`?

### Short Answer

`WHERE` filters rows before grouping, while `HAVING` filters groups after aggregation. Use `WHERE` whenever possible because it reduces the data set earlier.

### Deep Explanation

This distinction matters because filtering earlier often reduces the amount of work the database must do. If a condition does not depend on an aggregate, putting it in `HAVING` can force unnecessary grouping and sorting work.

### Senior-Level Notes

A strong answer mentions that correctness and performance are both affected. The clause placement can change plan shape, especially on large datasets.

### Common Mistakes

- using `HAVING` for non-aggregate predicates
- explaining the syntax without the execution implication

### Question

What is the difference between `INNER JOIN` and `LEFT JOIN`?

### Short Answer

`INNER JOIN` returns only matching rows from both sides. `LEFT JOIN` returns all rows from the left side and matching rows from the right, with nulls when no match exists.

### Deep Explanation

The choice is about data semantics, not just syntax. `LEFT JOIN` is appropriate when the left side should still appear without related data, such as customers with optional addresses. It can also influence query plans and row counts in ways that matter for downstream filtering and aggregation.

### Senior-Level Notes

Senior candidates usually point out that many incorrect reports come from accidental inner joins that silently drop rows.

### Common Mistakes

- using left joins by habit without checking cardinality
- forgetting that later filters on right-side columns can effectively turn a left join into an inner join

### Question

When are window functions useful?

### Short Answer

Window functions are useful when you need calculations across related rows without collapsing them into one grouped row, such as ranking, running totals, and per-partition comparisons.

### Deep Explanation

They solve problems that are awkward or inefficient with self-joins and correlated subqueries. For example, finding the latest order per customer or assigning row numbers within partitions is often clearer and faster with window functions.

### Senior-Level Notes

A strong answer mentions readability and plan quality. Window functions are often the right tool for analytical query patterns inside operational systems as well.

### Common Mistakes

- using grouped aggregates when row-level detail must remain
- avoiding window functions entirely because they look advanced

### Question

What causes duplicate rows after joins?

### Short Answer

Duplicate-looking rows usually come from join cardinality, not from the database being wrong. If one side has multiple matching rows, the result set expands accordingly.

### Deep Explanation

This often happens when engineers expect one-to-one relationships but join across one-to-many or many-to-many data. The fix is not automatically `DISTINCT`. The fix is understanding the relationship and deciding whether aggregation, filtering, or a different query shape is appropriate.

### Senior-Level Notes

Senior answers emphasize data modeling. Unexpected duplicates often reveal either a query misunderstanding or a schema/design assumption that was never explicit.

### Common Mistakes

- masking issues with `DISTINCT`
- blaming the database instead of reviewing cardinality

## Performance And Tuning

### Question

How do you investigate a slow SQL query?

### Short Answer

Inspect the query text, actual execution plan, row counts, index usage, and parameter values. Then determine whether the cost comes from scans, bad joins, sorting, blocking, or simply too much data.

### Deep Explanation

Performance tuning starts with evidence. You need to know whether the optimizer chose a poor plan, whether statistics are misleading, whether the query shape prevents index use, or whether the workload itself is too broad. Small-code rewrites without plan analysis often miss the real bottleneck.

### Senior-Level Notes

The strongest version of this answer is procedural: capture the exact SQL, get the actual plan, verify row counts and indexes, and only then talk about fixes. That sounds like someone who has done this under pressure.

### Common Mistakes

- tuning by intuition
- ignoring execution plans
- testing only on tiny datasets

### Question

Why might an index not be used even when one exists?

### Short Answer

An index may not be used if the predicate is not selective enough, the query shape prevents efficient seeks, statistics mislead the optimizer, or the needed columns make another plan cheaper overall.

### Deep Explanation

Functions on indexed columns, incompatible sort requirements, non-sargable predicates, or wide result projections can all make an index less attractive. This is why indexing is about workload fit, not simply index presence.

### Senior-Level Notes

Senior answers usually mention that "the index exists" is not the same thing as "the optimizer should use it." Parameter sensitivity, low selectivity, or a wide projection can all make another plan more sensible.

### Common Mistakes

- assuming every index should appear in the plan
- adding more indexes without changing the query shape
- ignoring statistics and selectivity

### Question

What is a sargable query, and why does it matter?

### Short Answer

A sargable query is one that allows the database engine to use indexes efficiently, typically by comparing indexed columns directly rather than wrapping them in functions or incompatible expressions.

### Deep Explanation

For example, filtering by a date range is usually more index-friendly than applying a function to the column in the predicate. Sargability matters because it determines whether the engine can seek into an index or must scan far more data.

### Senior-Level Notes

Strong candidates connect this to API design and reporting requirements. Sometimes the business requirement is fine, but the query can still be reshaped to preserve index usage.

### Common Mistakes

- memorizing the term without recognizing the pattern
- focusing only on indexes and ignoring predicate shape

### Question

When is denormalization justified?

### Short Answer

Denormalization is justified when a measured read-performance or reporting need outweighs the added complexity of keeping redundant data consistent. It should be a deliberate trade-off, not a shortcut around poor query design.

### Deep Explanation

Denormalization can reduce join cost, simplify read models, or support analytical workloads. But it introduces write complexity, synchronization risk, and migration burden. In many cases, better indexing, caching, or specialized read models are safer first steps.

### Senior-Level Notes

The more senior answer usually adds a decision sequence: first fix the query, then fix indexes, then consider caching or read-model separation, and only then denormalize if the workload still justifies it.

### Common Mistakes

- denormalizing too early
- ignoring synchronization cost
- using denormalization to compensate for unclear requirements

## Advanced Senior-Level Questions

### Question

When would you choose keyset pagination over offset pagination?

### Short Answer

I choose keyset pagination when the dataset is large, the user usually moves forward through results, and stable ordering exists. I keep offset pagination when random page access matters more than deep-page performance.

### Deep Explanation

Offset pagination is easy to implement and easy for clients to understand, but deep offsets can become expensive because the database still has to skip a lot of rows. Keyset pagination scales better for scrolling or feed-like access patterns, but it requires a reliable sort key and a different client contract.

### Senior-Level Notes

The better answer explicitly mentions product behavior. Pagination is not only a query problem; it is also an API and UX decision.

### Common Mistakes

- defaulting to offset pagination for very large ordered datasets
- choosing keyset pagination without a stable sorting strategy
- ignoring client contract implications

### Question

How do you reason about parameter-sensitive query plans?

### Short Answer

I watch for the case where one query shape behaves very differently for different parameter values. If the same cached plan is good for one case and terrible for another, I treat that as a plan-selection problem, not just a generic slow-query problem.

### Deep Explanation

Some predicates are highly selective for one value and broad for another. A plan chosen for the selective case may be awful for the broad case, or vice versa. This is why I want the exact parameters, the actual plan, and enough workload context before deciding whether the fix belongs in indexing, query shape, or execution strategy.

### Senior-Level Notes

The senior answer is careful here: do not recite vendor-specific tuning tricks first. Show that you understand why the plan is unstable.

### Common Mistakes

- treating all slow executions of the same query as the same problem
- tuning only against one parameter shape
- jumping to hints before understanding the workload

### Question

How do you decide whether an index is worth its write cost?

### Short Answer

I compare how much important read traffic benefits against how much insert, update, and maintenance cost the index adds. An index is worth it when it improves the right workload enough to justify the extra write and storage overhead.

### Deep Explanation

Indexes are not free. On a write-heavy table, every extra index increases work during inserts and updates. On the other hand, a single well-designed composite index can remove a major read bottleneck. The decision depends on read frequency, latency sensitivity, and how hot the write path already is.

### Senior-Level Notes

The stronger answer frames this as workload prioritization. Not every query deserves its own index if the table is write-sensitive.

### Common Mistakes

- indexing every frequently filtered column independently
- optimizing rare reporting paths at the expense of hot writes
- ignoring storage and maintenance cost

## Related Topics

- [../fundamentals/indexing.md](../fundamentals/indexing.md)
- [../fundamentals/query-optimization.md](../fundamentals/query-optimization.md)
- [database-design.md](database-design.md)
- [../examples/sql-performance-checklist.md](../examples/sql-performance-checklist.md)
