# NoSQL

## Overview

NoSQL is an umbrella term for non-relational data stores such as document, key-value, column-family, and graph databases. These systems trade some relational guarantees or query patterns for other benefits such as flexible schema, horizontal scale, or low-latency key access.

## Core Concepts

- Data model choice should reflect access patterns, consistency needs, and operational requirements.
- NoSQL does not mean "no schema." It usually means schema is enforced differently.
- Query flexibility, transactional guarantees, and indexing vary widely across NoSQL systems.
- Many systems use NoSQL selectively for specific workloads rather than as a full replacement for relational data.

## Why It Matters In Real Systems

NoSQL choices are often justified by access pattern fit rather than trend. A cache, session store, event store, or high-volume lookup service may benefit from a non-relational model, while core transactional workflows often still fit relational systems better.

## Practical Example

A document store may work well for product catalogs with flexible attributes and read-heavy retrieval by aggregate document. It is less ideal if the workload requires rich ad hoc joins and strict relational integrity across many entities.

## Common Pitfalls

- choosing NoSQL before clarifying the workload
- assuming schema flexibility removes the need for discipline
- underestimating migration and query trade-offs

## Interview Takeaways

- Explain NoSQL as a category of trade-offs, not a single technology.
- Compare it to relational systems in terms of workload fit.
- Show that you understand consistency and query implications.

## Related Topics

- [web-servers.md](web-servers.md)
- [../interview/nosql.md](../interview/nosql.md)
