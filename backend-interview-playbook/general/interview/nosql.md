# NoSQL Interview Questions

## Intro

These questions focus on when non-relational systems fit a workload.

## Trade-Offs

### Question

When would you choose a NoSQL database over a relational one?

### Short Answer

Choose NoSQL when its data model and operational profile fit the workload better, such as flexible documents, simple key-based access, or very high-scale specialized patterns.

### Deep Explanation

The decision should reflect access patterns, consistency expectations, and operational trade-offs rather than general preference.

### Senior-Level Notes

Strong answers compare strengths and weaknesses concretely.

### Common Mistakes

- treating NoSQL as a universal scalability answer

### Question

Does NoSQL mean schema-less?

### Short Answer

Not really. It usually means schema is enforced differently or more flexibly, but the application still depends on predictable structure.

### Deep Explanation

Unmanaged schema drift can become a maintainability and migration problem even in document-oriented systems.

### Senior-Level Notes

Senior answers mention contract discipline and data evolution.

### Common Mistakes

- assuming no schema means no design responsibility
