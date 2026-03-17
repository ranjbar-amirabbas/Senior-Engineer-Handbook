# Transactions Interview Questions

## Intro

Transactions questions test whether you understand local consistency, concurrency, and the limits of ACID in broader systems.

## Consistency

### Question

Why are transactions useful?

### Short Answer

They ensure a set of database operations succeeds or fails together, which protects consistency within a defined boundary.

### Deep Explanation

Without a transaction, partial updates can leave data in impossible states when failures occur midway through a workflow. Transactions reduce that risk inside one database boundary.

### Senior-Level Notes

Senior candidates mention that the useful boundary is usually smaller than the full business workflow in distributed systems.

### Common Mistakes

- assuming transactions solve cross-service consistency

### Question

What is the difference between pessimistic and optimistic concurrency?

### Short Answer

Pessimistic concurrency prevents conflicts by locking earlier. Optimistic concurrency detects conflicts later and rejects or resolves them when writes collide.

### Deep Explanation

Optimistic concurrency is common in business systems because collisions are less frequent than reads. Pessimistic approaches can reduce anomalies but at the cost of more blocking and less concurrency.

### Senior-Level Notes

Strong answers connect the choice to contention level and business semantics.

### Common Mistakes

- treating one approach as always superior

### Question

Why do deadlocks happen?

### Short Answer

Deadlocks happen when concurrent transactions each hold resources the other needs and neither can proceed. The database chooses one transaction to roll back.

### Deep Explanation

They are usually caused by inconsistent access order, long transactions, or contention-heavy patterns. The fix is often shortening transactions, reducing overlap, or standardizing resource access order.

### Senior-Level Notes

Senior answers mention retries with care and root-cause reduction instead of treating deadlocks as random noise.

### Common Mistakes

- ignoring repeated deadlocks as unavoidable

### Question

What is snapshot isolation useful for?

### Short Answer

Snapshot-style isolation helps readers avoid blocking on writers by reading a versioned view of committed data, improving concurrency in many workloads.

### Deep Explanation

It reduces some locking pain but comes with storage and behavior trade-offs. It is not a universal fix for every consistency problem.

### Senior-Level Notes

Strong candidates note that isolation settings should match workload behavior and be validated under load.

### Common Mistakes

- enabling higher isolation without understanding side effects
