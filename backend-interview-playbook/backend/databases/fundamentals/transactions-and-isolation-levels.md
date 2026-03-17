# Transactions And Isolation Levels

## Overview

Transactions define boundaries for atomicity and consistency within a database. Isolation levels define how concurrent transactions are allowed to interact. Together, they shape correctness, contention, and throughput.

## Core Concepts

- Transactions group operations so they succeed or fail as a unit.
- Isolation levels trade off consistency guarantees against concurrency and blocking behavior.
- Common anomalies include dirty reads, non-repeatable reads, phantom reads, and lost updates.
- Isolation is not the same thing as application-level business correctness across multiple services.

## Why It Matters In Real Systems

Backend systems often fail at the boundaries between correctness and concurrency. Too little isolation can create anomalies. Too much isolation can reduce throughput or cause deadlocks and blocking. Senior engineers should know how to choose intentionally rather than defaulting blindly.

## Practical Example

A checkout workflow that decrements inventory and creates an order may work correctly inside one database transaction. Once payment, shipping, and notifications involve separate systems, a single database transaction no longer solves end-to-end consistency.

## Common Pitfalls

- assuming transactions solve distributed consistency automatically
- holding transactions open during network calls
- using stronger isolation than needed without measuring impact
- confusing optimistic concurrency with isolation level choices

## Interview Takeaways

- Explain what transactions guarantee and what they do not.
- Mention blocking, deadlocks, and trade-offs between safety and throughput.
- Distinguish local ACID guarantees from distributed workflows.

## Related Topics

- [relational-database-design.md](relational-database-design.md)
- [../../distributed-systems/examples/outbox-pattern.md](../../distributed-systems/examples/outbox-pattern.md)
- [../interview/transactions.md](../interview/transactions.md)
