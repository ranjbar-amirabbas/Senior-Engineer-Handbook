# Kafka Interview Questions

## Intro

Kafka questions test whether you understand the platform model, not just vocabulary. Strong candidates can explain partitions, ordering, consumer groups, lag, and why broker guarantees do not automatically become business guarantees.

## Use This File With

- [../fundamentals/kafka.md](../fundamentals/kafka.md)
- [../fundamentals/event-driven-architecture.md](../fundamentals/event-driven-architecture.md)
- [../examples/event-versioning.md](../examples/event-versioning.md)

## Core Model

### Question

How does Kafka differ from a traditional queue?

### Short Answer

Kafka is a distributed log-based platform built around retained ordered partitions and consumer offsets, while a traditional queue is often centered on one-time delivery and message removal semantics. Kafka is better for replayable event streams and multiple independent consumers.

### Deep Explanation

Kafka stores records for a retention period and lets consumer groups track their own progress. That makes replay and fan-out easier than in many queue systems. It also means operational concerns like lag, retention, and partition planning matter more.

### Senior-Level Notes

Senior candidates should explain fit, not superiority. Sometimes a simpler queue is the better tool.

### Common Mistakes

- treating Kafka as just a faster queue

### Question

What is a partition, and why does it matter?

### Short Answer

A partition is an ordered shard of a Kafka topic. It matters because partitioning drives scalability, parallelism, and the scope of ordering guarantees.

### Deep Explanation

Events are ordered within a partition, not across the whole topic. Consumers in a group process partitions in parallel, so partition count and key design directly affect throughput and correctness assumptions.

### Senior-Level Notes

Strong answers mention that partition-key choice is a business decision as much as a technical one because it determines which records stay ordered together.

### Common Mistakes

- assuming ordering across the whole topic

### Question

How do you choose a partition key?

### Short Answer

Choose a key that keeps events requiring ordering together while still distributing load reasonably across partitions. The wrong key breaks either correctness or scalability.

### Deep Explanation

If all order lifecycle events must stay ordered, `OrderId` is often a good key. If one hot key dominates traffic, that partition becomes a bottleneck. Partitioning is therefore a trade-off between ordering locality and load distribution.

### Senior-Level Notes

The stronger answer usually sounds like this: "I pick a key by starting with the ordering requirement, then I check whether that key will create hotspots." That is much more realistic than talking about partitioning only in abstract scalability terms.

### Common Mistakes

- choosing random keys when ordering matters
- choosing one global key and killing parallelism

### Question

What delivery guarantee does Kafka usually provide to consumers?

### Short Answer

In most practical consumer designs, Kafka provides at-least-once delivery unless the entire processing path is designed carefully to avoid duplicates.

### Deep Explanation

If a consumer processes a message and fails before committing the offset, the message can be delivered again. This is normal. Consumers must therefore be idempotent or have durable deduplication for sensitive side effects.

### Senior-Level Notes

Strong answers separate broker semantics from business outcome guarantees.

### Common Mistakes

- equating committed offsets with exactly-once business behavior

## Operations And Design

### Question

What is consumer lag, and why is it important?

### Short Answer

Consumer lag is the gap between the latest produced offset and the consumer's committed or processed position. It indicates how far behind the consumer is.

### Deep Explanation

Lag matters because it reflects system health, processing capacity, and the user-visible freshness of downstream behavior. Rising lag can indicate slow consumers, dependency issues, partition imbalance, or an event storm.

### Senior-Level Notes

The useful senior answer ties lag to user impact. "Lag is not only a broker metric; it tells me whether downstream business behavior is late." That framing shows operational maturity.

### Common Mistakes

- treating lag as only an infrastructure metric

### Question

How do you handle schema evolution in Kafka events?

### Short Answer

Use version-tolerant contracts, evolve schemas additively when possible, and ensure consumers can tolerate older or newer shapes during rollout.

### Deep Explanation

Events often outlive the code that produced them. If a producer removes or repurposes fields carelessly, consumers can fail or misbehave. Schema registries and compatibility rules help, but disciplined contract evolution is still required.

### Senior-Level Notes

A strong answer mentions that versioning is not only syntax compatibility. Semantics must remain understandable as well.

### Common Mistakes

- changing field meaning without changing the contract
- assuming all consumers upgrade at once

### Question

What problems can consumer group rebalancing cause?

### Short Answer

Rebalancing can temporarily pause consumption, move partitions between instances, and cause duplicate work or warm-up costs if consumers do not handle assignment changes carefully.

### Deep Explanation

Rebalances happen when instances join, leave, or change health. During that time, partitions move and in-memory state tied to a partition may need to be rebuilt. This affects latency and operational stability.

### Senior-Level Notes

The practical answer is that rebalancing hurts most when consumers keep meaningful local state or take a long time to warm up. That is why deployments and autoscaling policies matter to Kafka consumers more than teams often expect.

### Common Mistakes

- ignoring rebalancing impact during autoscaling or deployments

### Question

When is Kafka the wrong choice?

### Short Answer

Kafka is the wrong choice when the problem is simple command-style messaging, low operational tolerance, or modest scale that does not justify the platform complexity.

### Deep Explanation

Kafka is powerful, but it adds cluster operations, partition management, lag monitoring, and schema-discipline obligations. For straightforward background jobs or low-volume queues, simpler tools may be easier to run and reason about.

### Senior-Level Notes

The strongest answer names an alternative. Saying "I would rather use a simpler queue here because the workload is low volume and command-oriented" shows much better judgment than simply listing Kafka drawbacks.

### Common Mistakes

- choosing Kafka for prestige or premature scale concerns

## Advanced Senior-Level Questions

### Question

What does "exactly once" mean in Kafka discussions, and why is it often misunderstood?

### Short Answer

It usually refers to a constrained processing guarantee inside Kafka-aware producer and consumer flows, not a blanket promise that your entire business workflow can never duplicate side effects. Business-level exactly-once behavior still requires application design.

### Deep Explanation

Kafka can help coordinate producer and consumer semantics, but if the consumer updates an external database, calls a payment provider, or sends email, those side effects still need idempotency or reconciliation. That is why I treat exactly-once claims carefully and ask which boundary the guarantee actually covers.

### Senior-Level Notes

The strongest answer separates broker semantics from end-to-end business correctness immediately.

### Common Mistakes

- repeating "Kafka supports exactly once" without naming the boundary
- assuming broker guarantees remove the need for idempotent consumers
- overselling the operational simplicity of stronger guarantees

### Question

How do you design retry and dead-letter handling for Kafka consumers?

### Short Answer

I decide first which failures are transient, which are poison messages, and what operational action each class should trigger. Retries and dead-letter topics are useful only when the team knows how messages leave those states.

### Deep Explanation

Blind retries can amplify incidents. Immediate dead-lettering can hide recoverable transient faults. Good designs classify failure types, bound retry behavior, preserve enough context for investigation, and define whether messages are replayed, corrected, or intentionally dropped.

### Senior-Level Notes

The senior answer usually includes ownership: a dead-letter topic is not a trash can. It needs visibility and an operating process.

### Common Mistakes

- retrying poison messages forever
- creating dead-letter topics with no replay or triage workflow
- mixing transient and permanent failure handling together

### Question

How do retention and topic design affect operations?

### Short Answer

Retention and topic boundaries affect storage cost, replay ability, recovery options, and how easy the platform is to operate. Topic design is not just a developer naming exercise.

### Deep Explanation

Long retention increases replay flexibility but also increases storage and recovery considerations. Topic boundaries influence ownership, access control, schema discipline, and consumer behavior. If topics are too broad, consumers become coupled. If they are too fragmented, operations and governance become messy.

### Senior-Level Notes

The better answer makes it clear that topic design is both a product-contract decision and a platform-operations decision.

### Common Mistakes

- creating topics without clear ownership or retention rationale
- keeping long retention by default without operational review
- fragmenting topics so much that governance becomes harder than consumption

## Related Topics

- [../fundamentals/kafka.md](../fundamentals/kafka.md)
- [../fundamentals/consistency-and-reliability.md](../fundamentals/consistency-and-reliability.md)
- [../examples/event-versioning.md](../examples/event-versioning.md)
- [microservices.md](microservices.md)
