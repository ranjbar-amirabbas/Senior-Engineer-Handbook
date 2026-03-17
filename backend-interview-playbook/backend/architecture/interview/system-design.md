# System Design Interview Questions

## Intro

System design interviews test how you think under constraints. Strong answers show prioritization, trade-off reasoning, and operational awareness rather than attempting to design every possible subsystem at once.

## Use This File With

- [../fundamentals/system-design-basics.md](../fundamentals/system-design-basics.md)
- [../fundamentals/rest-api-design.md](../fundamentals/rest-api-design.md)
- [../examples/caching-strategies.md](../examples/caching-strategies.md)

## Requirements And Scope

### Question

What is the first thing you do in a system design interview?

### Short Answer

Clarify requirements, constraints, scale expectations, and success criteria before proposing architecture. Good design starts with the problem definition.

### Deep Explanation

Without understanding read-write ratio, consistency needs, latency targets, user behavior, and failure tolerance, the architecture is guesswork. Requirement clarification also signals maturity because it shows that design decisions are constraint-driven.

### Senior-Level Notes

The better answer usually includes examples of the questions you would ask first: traffic pattern, read-write ratio, latency target, consistency requirement, and failure tolerance. That sounds more credible than saying "I would clarify requirements."

### Common Mistakes

- jumping straight into components and technologies

### Question

How do you balance functional and non-functional requirements?

### Short Answer

Design must satisfy the product behavior first, but non-functional requirements such as latency, reliability, and operability shape how the behavior can be delivered.

### Deep Explanation

Two systems with similar user features may require different architectures if one must support global traffic, low latency, or strict auditability. Good design treats these concerns as first-class constraints rather than polish.

### Senior-Level Notes

Strong answers connect architecture choices directly to a specific non-functional requirement.

### Common Mistakes

- listing non-functional requirements without showing how they affect design

## Data And Scaling

### Question

When do you add caching to a system design?

### Short Answer

Add caching when repeated expensive reads justify the complexity, especially when data can tolerate some staleness or cache invalidation can be managed safely.

### Deep Explanation

Caching can reduce database load and latency, but it also introduces consistency and invalidation challenges. It should be used where the access pattern is favorable, not sprinkled everywhere without ownership.

### Senior-Level Notes

The stronger answer names the cache boundary explicitly: response cache, application cache, distributed cache, or read model. That shows you are thinking about ownership and invalidation, not treating caching as one generic box.

### Common Mistakes

- treating caching as a universal performance fix

### Question

How do you identify likely bottlenecks in a design?

### Short Answer

Look at the hottest read and write paths, shared dependencies, stateful components, and fan-out patterns. Bottlenecks often appear where many requests contend for the same limited resource.

### Deep Explanation

Databases, caches, queues, and external integrations can all become chokepoints. The right design conversation identifies which components are on the critical path and how workload shape affects them.

### Senior-Level Notes

Strong answers mention both throughput and tail latency, not only average response time.

### Common Mistakes

- discussing scalability only in terms of adding more app servers

### Question

What is the difference between horizontal and vertical scaling?

### Short Answer

Vertical scaling increases resources on a single node. Horizontal scaling adds more nodes. Horizontal scaling improves capacity and resilience in many cases, but only when the workload and state model allow it.

### Deep Explanation

Stateless frontends usually scale horizontally well. Stateful components such as databases require more careful strategies like replication, partitioning, caching, or workload separation. Scaling choices must match the system's bottlenecks.

### Senior-Level Notes

Senior candidates note that vertical scaling is often simpler operationally and may be perfectly valid up to a point.

### Common Mistakes

- presenting horizontal scaling as always better

## Reliability And Evolution

### Question

How do you design for failure?

### Short Answer

Assume dependencies fail or slow down, then design with timeouts, retries where safe, fallbacks, observability, and degradation paths.

### Deep Explanation

Designing for failure means thinking about what the user sees, what the system retries, what data may be inconsistent temporarily, and how operators will detect and recover from the problem.

### Senior-Level Notes

The practical senior answer usually includes one concrete containment tactic: timeouts plus circuit breaking, queueing non-critical work, degraded mode, or isolating the dependency from the user-critical path.

### Common Mistakes

- designing only the happy path

### Question

How do you decide between synchronous and asynchronous communication?

### Short Answer

Use synchronous communication when the caller needs an immediate answer to continue. Use asynchronous communication when decoupling, buffering, or eventual consistency is acceptable.

### Deep Explanation

Synchronous communication is simpler for request-response use cases but creates temporal coupling. Asynchronous communication improves resilience and throughput shaping, but adds coordination and observability complexity.

### Senior-Level Notes

Senior candidates often propose a hybrid design: synchronous on the user-critical path, asynchronous for downstream side effects.

### Common Mistakes

- assuming asynchronous means automatically better scalability

### Question

How do you evolve a system safely as it grows?

### Short Answer

Evolve it incrementally with backward-compatible contracts, observable rollout steps, and targeted refactoring around the real pain points rather than big-bang rewrites.

### Deep Explanation

Systems accumulate consumers, operational dependencies, and hidden coupling over time. Safe evolution requires understanding those dependencies, staging changes, and preserving service continuity while modernizing the design.

### Senior-Level Notes

Strong answers mention migration sequencing, feature flags, compatibility windows, and selective decomposition rather than rewriting everything.

### Common Mistakes

- proposing total rewrites as the default answer to scaling problems

### Question

What distinguishes a senior-level system design answer from a mid-level one?

### Short Answer

A senior-level answer is explicit about trade-offs, bottlenecks, failure modes, and operational impact. It treats architecture as a series of justified decisions under constraints.

### Deep Explanation

Mid-level answers often stop at component diagrams. Senior answers explain why those components exist, what will break first, how the team will observe and operate the system, and how the design can evolve safely.

### Senior-Level Notes

This is usually the real signal: can you make a reasonable first design, say what you are assuming, and explain what you would revisit once traffic, cost, or reliability data arrives.

### Common Mistakes

- overfocusing on tool names instead of design reasoning

## Advanced Senior-Level Questions

### Question

How do you design for multi-tenancy without overcomplicating the system too early?

### Short Answer

I start by clarifying the isolation requirement: logical isolation, noisy-neighbor control, compliance, and tenant-level customization. The architecture should match that requirement instead of assuming every multi-tenant system needs the strongest isolation model immediately.

### Deep Explanation

Some products work well with shared infrastructure plus tenant-aware data partitioning and rate controls. Others need stronger separation because of regulation, blast-radius concerns, or high-value tenants. The right answer depends on workload variance, operational cost, and the likelihood that isolation requirements will tighten later.

### Senior-Level Notes

The stronger answer usually includes migration thinking: how hard will it be to move a large tenant or a regulated tenant into a more isolated model later if the business needs it?

### Common Mistakes

- assuming all tenants should share everything forever
- overengineering tenant isolation before understanding the business
- ignoring noisy-neighbor behavior

### Question

How do you use SLOs or service objectives in system design?

### Short Answer

I use service objectives to decide where the design needs redundancy, performance budget, observability, and operational investment. They force the architecture to care about user-visible reliability instead of generic uptime language.

### Deep Explanation

Without explicit objectives, teams often argue about architecture in the abstract. Once latency, freshness, or availability targets are clear, the design conversation becomes sharper: which dependencies are on the critical path, which ones can degrade, and how much cost or complexity is justified to protect the objective.

### Senior-Level Notes

The best answer connects objectives to trade-offs. If the SLO is strict, the design probably needs simpler hot paths, tighter dependency control, and better operational guardrails.

### Common Mistakes

- quoting SLO terminology without showing how it changes design
- setting targets no one can measure
- optimizing the wrong parts of the system relative to the objective

### Question

How do you approach a brownfield system design interview when the system already exists and has problems?

### Short Answer

I resist the urge to redesign everything. I identify the highest-cost bottlenecks or failure modes, propose incremental changes, and explain how to improve the system while preserving service continuity.

### Deep Explanation

Most real systems are not greenfield. They have migration constraints, old clients, operational habits, and hidden dependencies. A strong brownfield answer shows how to sequence improvements, reduce risk, and create evidence that the architecture is actually getting better.

### Senior-Level Notes

The strongest answer usually mentions compatibility windows, feature flags, rollout metrics, and avoiding big-bang replacement unless the current system is truly unmaintainable.

### Common Mistakes

- proposing a rewrite as the first answer
- ignoring migration cost and operational continuity
- designing as if there are no existing consumers or contracts

## Related Topics

- [../fundamentals/system-design-basics.md](../fundamentals/system-design-basics.md)
- [rest-api.md](rest-api.md)
- [scalability.md](scalability.md)
- [../examples/api-idempotency.md](../examples/api-idempotency.md)
