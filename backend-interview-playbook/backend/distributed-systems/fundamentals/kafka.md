# Kafka

## Overview

Kafka is a distributed event streaming platform commonly used for durable messaging, event pipelines, and high-throughput asynchronous integration. It is powerful, but it comes with explicit trade-offs around ordering, partitioning, delivery semantics, and operations.

## Core Concepts

- Topics are divided into partitions, and partitioning is central to scaling and ordering behavior.
- Ordering is guaranteed only within a partition, not across a whole topic.
- Consumers track offsets and typically operate with at-least-once delivery semantics.
- Retention, replay, and consumer group behavior make Kafka different from a simple queue.

## Why It Matters In Real Systems

Teams often adopt Kafka for scale or decoupling and then struggle with key selection, duplicate processing, lag, and schema evolution. Senior engineers should understand the implications of the model before choosing the platform.

## Practical Example

If all events for a given `OrderId` use the same partition key, a consumer can process that order's event stream in order. If the key is chosen poorly, related events may land in different partitions and ordering assumptions break.

## Common Pitfalls

- assuming Kafka provides global ordering
- ignoring partition-key design
- treating at-least-once delivery as exactly-once business behavior
- using Kafka where a simpler queue would have been enough

## Interview Takeaways

- Explain partitions, ordering scope, offsets, and delivery semantics.
- Mention observability, lag, replay, and schema discipline.
- Show that platform choice should match system needs.

## Related Topics

- [event-driven-architecture.md](event-driven-architecture.md)
- [consistency-and-reliability.md](consistency-and-reliability.md)
- [../interview/kafka.md](../interview/kafka.md)
