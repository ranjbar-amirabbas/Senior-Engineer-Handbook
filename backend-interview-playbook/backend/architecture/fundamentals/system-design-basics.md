# System Design Basics

## Overview

System design is the practice of shaping a system so it meets functional and non-functional requirements under realistic constraints. For backend engineers, that means thinking about data flow, capacity, latency, reliability, operability, and change over time.

## Core Concepts

- Start with requirements: throughput, latency, consistency, durability, and operational expectations.
- Identify the main components, data flows, storage choices, and failure boundaries.
- Design for the hottest paths first, but do not ignore operational support and recovery.
- Trade-offs matter more than drawing more boxes.

## Why It Matters In Real Systems

A design that looks elegant on a whiteboard can fail because it ignored one practical constraint: write amplification, cache invalidation, deployment complexity, or debugging cost. System design is therefore a decision-making skill, not a diagramming exercise.

## Practical Example

For a notification system, the first questions are not "Which queue?" They are volume, delivery guarantees, fan-out pattern, retry behavior, and whether delivery must be immediate or merely timely. Those answers drive the architecture.

## Common Pitfalls

- jumping to technologies before clarifying requirements
- optimizing every part equally instead of focusing on bottlenecks
- ignoring observability and failure handling
- designing only for the happy path

## Interview Takeaways

- Clarify requirements first.
- Explain trade-offs explicitly.
- Mention scaling, reliability, and operational visibility together.

## Related Topics

- [rest-api-design.md](rest-api-design.md)
- [scalability-engineering.md](scalability-engineering.md)
- [../interview/system-design.md](../interview/system-design.md)
