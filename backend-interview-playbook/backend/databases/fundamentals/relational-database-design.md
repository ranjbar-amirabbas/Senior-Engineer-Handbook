# Relational Database Design

## Overview

Relational database design is the practice of modeling data, relationships, and constraints so the database supports correctness, queryability, and long-term change. Good design reduces application complexity. Poor design pushes complexity upward into code and operations.

## Core Concepts

- Tables model entities and relationships, but table structure should reflect real data behavior rather than convenience for one screen or report.
- Keys, foreign keys, uniqueness constraints, and check constraints protect invariants close to the data.
- Normalization reduces redundancy and update anomalies, while selective denormalization can improve specific read paths.
- Schema design should anticipate how the data will be queried, updated, and evolved.

## Why It Matters In Real Systems

Application code can compensate for some schema mistakes, but only at ongoing cost. Weak constraints allow bad data. Poorly chosen relationships create expensive joins. Denormalizing too early makes writes fragile. Over-normalizing can make high-traffic queries harder than they need to be.

## Practical Example

In an e-commerce system, `Orders`, `OrderItems`, and `Customers` are usually separate tables with foreign keys. Storing all order items as a serialized blob may look flexible at first, but it makes reporting, validation, and indexing much harder.

## Common Pitfalls

- designing tables around ORM convenience instead of domain rules
- omitting constraints because the application "already validates it"
- denormalizing before measuring a real bottleneck
- using generic tables with weak semantics

## Interview Takeaways

- Explain schema design in terms of correctness and change cost.
- Mention normalization, constraints, and query patterns together.
- Show that you know database design affects code quality directly.

## Related Topics

- [indexing.md](indexing.md)
- [transactions-and-isolation-levels.md](transactions-and-isolation-levels.md)
- [../interview/database-design.md](../interview/database-design.md)
