# Async And Await

## Overview

`async` and `await` are central to scalable backend development in .NET. They help services avoid blocking threads during I/O, which improves throughput and responsiveness under load.

## Core Concepts

- Async is primarily about non-blocking waits, especially for I/O-bound work.
- `Task` represents future completion and allows asynchronous operations to compose.
- `await` suspends the method until the awaited operation completes without blocking the current thread.
- Async does not automatically create parallelism or make CPU-bound work faster.

## Why It Matters In Real Systems

Web APIs spend a large part of their time waiting on databases, caches, and remote services. If those waits block threads, throughput collapses under load and the thread pool becomes a bottleneck. Async improves server scalability by releasing threads during those waits.

## Practical Example

```csharp
public async Task<OrderDto?> GetOrderAsync(Guid id, CancellationToken cancellationToken)
{
    return await dbContext.Orders
        .Where(x => x.Id == id)
        .Select(x => new OrderDto(x.Id, x.Number, x.Total))
        .SingleOrDefaultAsync(cancellationToken);
}
```

The main gain here is not speed of the database call itself. It is that the request thread is not blocked while the database is working.

## Common Pitfalls

- blocking on `Task.Result` or `Task.Wait()`
- wrapping ordinary I/O calls in `Task.Run()`
- mixing sync and async carelessly
- forgetting to pass cancellation tokens through I/O boundaries

## Interview Takeaways

- Explain async in terms of scalability and resource usage.
- Separate async from multithreading.
- Mention deadlocks, blocking, thread pool starvation, and cancellation as practical concerns.

## Related Topics

- [multithreading-and-thread-safety.md](multithreading-and-thread-safety.md)
- [../interview/async-concurrency-threading.md](../interview/async-concurrency-threading.md)
- [../examples/background-jobs.md](../examples/background-jobs.md)
