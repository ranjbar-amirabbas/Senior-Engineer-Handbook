# Async, Concurrency, And Threading Interview Questions

## Intro

This file focuses on one of the most common weak spots in backend interviews. Strong candidates can explain what `async` does, what it does not do, and how concurrency problems show up in production systems instead of only in whiteboard examples.

## Use This File With

- [../fundamentals/async-await.md](../fundamentals/async-await.md)
- [../fundamentals/multithreading-and-thread-safety.md](../fundamentals/multithreading-and-thread-safety.md)
- [../../distributed-systems/fundamentals/consistency-and-reliability.md](../../distributed-systems/fundamentals/consistency-and-reliability.md)

## Async Basics

### Question

What is the difference between asynchronous programming and multithreading?

### Short Answer

Asynchronous programming is about not blocking while waiting for work to complete, especially I/O. Multithreading is about using multiple threads of execution. They overlap, but they are not the same thing.

### Deep Explanation

An async database call may use no dedicated thread while the database is processing the request. A CPU-bound workload, on the other hand, may need actual parallel execution on multiple threads. Confusing the two leads to poor design, such as wrapping I/O in `Task.Run()` or assuming `async` automatically makes CPU work faster.

### Senior-Level Notes

Senior candidates should mention scalability. Async I/O improves throughput in servers because request threads are not blocked during waits, allowing the thread pool to serve more work.

### Common Mistakes

- saying `async` means multithreaded
- using `Task.Run()` for ordinary I/O
- assuming `await` improves CPU performance

### Question

What happens when you mark a method as `async` and use `await`?

### Short Answer

The compiler transforms the method into a state machine that can suspend when an awaited operation is incomplete and resume later when it finishes. This allows the current thread to be released instead of blocked during the wait.

### Deep Explanation

The key behavior is suspension, not magic parallelism. The method returns a `Task` representing future completion. The caller can also await it, allowing async behavior to compose across layers. This is why "async all the way" is a useful guideline for I/O-heavy paths.

### Senior-Level Notes

Strong answers mention that async introduces its own overhead. It is valuable when it prevents blocking, not when added mechanically to trivial synchronous work.

### Common Mistakes

- describing `await` as a background thread switch
- using async pervasively without an I/O reason
- ignoring the task returned by async methods

### Question

Why is blocking on `Task.Result` or `Task.Wait()` dangerous?

### Short Answer

Blocking on async work can cause deadlocks in some environments and wastes threads in all environments. In server applications, it also reduces scalability by tying up thread pool threads during waits.

### Deep Explanation

Even where a classic synchronization-context deadlock is less likely, blocking still creates backpressure on the thread pool. Under load, enough blocked threads can cause thread pool starvation and degraded latency. The safer design is to stay asynchronous end to end when the operation is fundamentally asynchronous.

### Senior-Level Notes

The better answer sounds like production experience: "Even if I avoid the classic deadlock case, I still do not want request threads blocked waiting on I/O because that shows up as capacity loss and ugly latency under load."

### Common Mistakes

- assuming blocking is safe just because a deadlock does not happen locally
- mixing sync and async freely in hot paths
- calling async libraries from sync code without a clear boundary strategy

## Concurrency And Coordination

### Question

What is the difference between `Task`, `Thread`, and the thread pool?

### Short Answer

`Task` is an abstraction for asynchronous or concurrent work, `Thread` is an operating system execution thread, and the thread pool is a managed pool of reusable worker threads. Most backend code should work with tasks and let the runtime manage threads.

### Deep Explanation

Creating dedicated threads is expensive and usually unnecessary for standard application work. `Task` allows the runtime to schedule work efficiently, integrate with async composition, and manage concurrency. Explicit threads are reserved for specialized scenarios where you truly need thread-level control.

### Senior-Level Notes

Senior engineers talk about ownership and cost. When code creates threads casually, it often bypasses runtime optimizations and makes shutdown, cancellation, and resource usage harder to control.

### Common Mistakes

- treating `Task` as just a lighter thread
- creating raw threads for normal request processing
- ignoring cancellation and lifecycle behavior

### Question

What makes code thread-safe?

### Short Answer

Code is thread-safe when it behaves correctly under concurrent access without corrupting shared state or violating invariants. Thread safety usually comes from immutability, isolation, or controlled synchronization around shared mutable state.

### Deep Explanation

The most reliable approach is to avoid shared mutable state where possible. When sharing is necessary, mechanisms such as `lock`, `SemaphoreSlim`, atomic operations, concurrent collections, or message passing can coordinate access. The right choice depends on whether the issue is mutual exclusion, throttling, ordering, or memory visibility.

### Senior-Level Notes

Senior interviewers usually want to hear how you reduce shared mutable state before reaching for primitives. "I try to make ownership obvious first" is often a stronger answer than listing `lock`, `volatile`, and `ConcurrentDictionary`.

### Common Mistakes

- assuming thread safety only means adding locks
- overlooking shared state inside caches, singletons, or static fields
- ignoring performance impact of lock contention

### Question

When would you use `lock` versus `SemaphoreSlim`?

### Short Answer

Use `lock` for protecting short synchronous critical sections in one process. Use `SemaphoreSlim` when you need asynchronous coordination or want to limit concurrency rather than provide strict mutual exclusion only.

### Deep Explanation

`lock` is simple and efficient for synchronous in-process access to shared state, but it cannot be awaited. `SemaphoreSlim` supports `WaitAsync()` and is often used in async workflows, such as limiting concurrent calls to an external service. They solve related but different coordination problems.

### Senior-Level Notes

Senior answers mention scope and misuse. Neither primitive works for cross-process coordination, and neither should be used casually around large I/O-heavy blocks because that turns a small guard into a throughput bottleneck.

### Common Mistakes

- awaiting inside a `lock`
- using `SemaphoreSlim` as a fashionable replacement for every `lock`
- protecting too much code and creating contention hotspots

### Question

What is thread pool starvation, and how does it happen?

### Short Answer

Thread pool starvation happens when available worker threads are tied up faster than the runtime can supply more, causing queued work to wait excessively. It often happens when async-capable server code blocks on I/O or long-running work.

### Deep Explanation

Backend services rely heavily on the thread pool. If request handlers block on database calls, HTTP calls, or locks, the pool fills with waiting threads instead of serving active work. As queued continuations and requests pile up, latency spikes and the service can appear unstable even if the database or network is the real bottleneck.

### Senior-Level Notes

Strong candidates connect starvation to observability. Rising queue times, increased latency under moderate load, and many blocked stacks are practical indicators. The fix is usually reducing blocking, not simply increasing thread count.

### Common Mistakes

- diagnosing starvation as pure CPU saturation
- fixing symptoms with more hardware before removing blocking
- ignoring blocking calls inside libraries or logging paths

## Cancellation And Reliability

### Question

Why do cancellation tokens matter in backend systems?

### Short Answer

Cancellation tokens let work stop when the caller no longer needs the result or when the system is shutting down. They reduce wasted work, improve responsiveness, and help services release resources promptly.

### Deep Explanation

In web APIs, request cancellation can happen because the client disconnected or a timeout expired. In background processing, shutdown signals should stop new work and allow in-flight work to finish responsibly. Passing cancellation tokens through database calls, HTTP calls, and internal workflows makes the system behave better under failure and deployment events.

### Senior-Level Notes

The stronger answer mentions business boundaries. It is fine to cancel a read request aggressively. It is not fine to cancel halfway through a workflow that already committed an external side effect unless the rest of the design accounts for that.

### Common Mistakes

- accepting cancellation tokens but never using them
- cancelling operations at unsafe points in a business workflow
- treating cancellation as an error equivalent to a failure

### Question

How do race conditions show up in backend code?

### Short Answer

Race conditions happen when the correctness of the result depends on the timing of concurrent operations. In backend systems, they often show up in caches, background jobs, duplicate message handling, balance updates, and check-then-act logic.

### Deep Explanation

A classic example is reading a value, making a decision, and then updating it without protecting the invariant against concurrent changes. Another is two consumers processing the same logical event because delivery is at least once. Solving race conditions may require locks, database constraints, optimistic concurrency, idempotency, or redesigning ownership.

### Senior-Level Notes

The more senior answer usually expands the scope: race conditions are not only "two threads touched the same object." They also show up as duplicate message processing, optimistic concurrency conflicts, and multi-step workflows that assume stale reads are still current.

### Common Mistakes

- assuming single-instance local testing proves safety
- relying only on in-memory guards in distributed workflows
- ignoring duplicate delivery or retry behavior

## Advanced Senior-Level Questions

### Question

How do you limit concurrency in a backend service without hurting throughput unnecessarily?

### Short Answer

I limit concurrency at the boundary that is actually scarce, such as external API calls, database connections, or CPU-heavy work, instead of serializing everything. The goal is controlled throughput, not global slowness.

### Deep Explanation

Unbounded parallelism can overload dependencies and create retry storms or thread-pool pressure. Overly tight limits can waste capacity. Techniques such as bounded semaphores, channels, worker pools, or queue-based backpressure help when applied to the right resource.

### Senior-Level Notes

The stronger answer mentions that concurrency limits should be observable and workload-specific. If the limit is arbitrary and unmeasured, it is probably wrong.

### Common Mistakes

- launching unbounded parallel tasks for I/O-heavy work
- putting one global lock around unrelated operations
- setting concurrency limits without measuring queueing and latency

### Question

When would you use `Channel<T>` or a queue-like in-process pipeline?

### Short Answer

I use `Channel<T>` when I need bounded asynchronous producer-consumer coordination inside one process, especially for smoothing bursts or separating ingestion from processing. I do not use it as a substitute for durable messaging when losing the process would lose important work.

### Deep Explanation

Channels are useful for in-memory handoff, backpressure, and controlled worker pipelines. They are good for local concurrency control and decoupling stages inside one service. They are not durable, cross-process, or automatically recoverable after a crash.

### Senior-Level Notes

The strongest answer makes the boundary clear: in-process coordination versus durable workflow execution.

### Common Mistakes

- using in-process channels for must-not-lose business work
- forgetting bounded capacity and backpressure
- assuming queue-like abstractions automatically solve load problems

### Question

How do you think about backpressure in async systems?

### Short Answer

Backpressure is how a system avoids accepting more work than it can process safely. In async systems, it matters because non-blocking code can still overwhelm dependencies or build unbounded in-memory queues.

### Deep Explanation

Async removes blocking waits, not physical limits. If producers can enqueue faster than consumers can process, memory grows, lag increases, and recovery gets harder. Bounded queues, admission control, rate limits, and dependency-aware concurrency limits are common backpressure tools.

### Senior-Level Notes

The senior answer usually ties backpressure to graceful degradation: deciding what to slow down, reject, or defer so the whole service stays healthy.

### Common Mistakes

- assuming async code is naturally self-regulating
- allowing unbounded queues to accumulate silently
- treating backpressure as only an infrastructure problem

## Related Topics

- [../fundamentals/async-await.md](../fundamentals/async-await.md)
- [../fundamentals/multithreading-and-thread-safety.md](../fundamentals/multithreading-and-thread-safety.md)
- [../../distributed-systems/interview/distributed-systems.md](../../distributed-systems/interview/distributed-systems.md)
- [../examples/background-jobs.md](../examples/background-jobs.md)
