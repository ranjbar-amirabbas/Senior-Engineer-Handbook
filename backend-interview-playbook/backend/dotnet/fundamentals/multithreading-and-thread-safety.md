# Multithreading And Thread Safety

## Overview

Threading becomes important when a backend system does work concurrently across requests, background jobs, message consumers, and shared infrastructure. The main challenge is not creating concurrency. It is preserving correctness under concurrency.

## Core Concepts

- Threads are execution resources. Tasks are a higher-level abstraction the runtime can schedule.
- Shared mutable state is the main source of thread-safety bugs.
- Thread safety can come from immutability, isolation, synchronization, or idempotent design.
- `lock`, `SemaphoreSlim`, atomic operations, concurrent collections, and database constraints solve different problems.

## Why It Matters In Real Systems

Race conditions are rarely visible in single-user local testing. They appear in caches, background processing, duplicate message handling, and state transitions that depend on timing. A small concurrency bug in a payments or inventory workflow can become a serious business bug.

## Practical Example

Suppose a background worker updates a shared in-memory cache while requests read from it. If the update process mutates the same dictionary instance that readers use, requests can observe inconsistent state. Replacing the cache atomically with a new immutable snapshot is often safer than locking the entire read path.

## Common Pitfalls

- assuming thread safety only matters in explicit multithreading code
- protecting large I/O-heavy blocks with locks
- using in-memory synchronization for distributed coordination problems
- forgetting that `DbContext` is not thread-safe

## Interview Takeaways

- Start with ownership and state boundaries before discussing primitives.
- Show that you know how race conditions appear in real systems.
- Distinguish in-process thread safety from distributed consistency.

## Related Topics

- [async-await.md](async-await.md)
- [dependency-injection.md](dependency-injection.md)
- [../interview/async-concurrency-threading.md](../interview/async-concurrency-threading.md)
- [../../distributed-systems/fundamentals/consistency-and-reliability.md](../../distributed-systems/fundamentals/consistency-and-reliability.md)
