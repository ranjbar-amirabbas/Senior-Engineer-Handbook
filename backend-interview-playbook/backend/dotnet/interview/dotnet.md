# .NET Interview Questions

## Intro

This file covers core .NET knowledge that senior backend engineers are often expected to explain clearly: runtime behavior, language choices, resource management, and common design decisions. In real interviews, the strong answer is usually short first, then grounded in runtime behavior, and then tied back to maintainability or production impact.

## Use This File With

- [../fundamentals/csharp-fundamentals.md](../fundamentals/csharp-fundamentals.md)
- [../fundamentals/dependency-injection.md](../fundamentals/dependency-injection.md)
- [../framework/entity-framework.md](../framework/entity-framework.md)

## Runtime And Language Basics

### Question

What is the difference between value types and reference types in C#?

### Short Answer

Value types store their data directly, while reference types store a reference to an object allocated elsewhere, usually on the managed heap. The difference matters for copying behavior, nullability, allocation patterns, and performance.

### Deep Explanation

When you assign one value type to another, the value is copied. With reference types, the reference is copied, so both variables point to the same object. In practice, this affects mutation semantics and memory pressure. `struct` can be efficient for small immutable data, but large mutable structs can create subtle bugs and copy costs.

### Senior-Level Notes

A senior answer should mention that stack-versus-heap is not the whole story. JIT optimizations, boxing, closures, and async state machines affect where data ends up. The key interview signal is understanding semantics and trade-offs, not repeating simplified rules.

### Common Mistakes

- saying value types always live on the stack
- ignoring boxing and unboxing
- treating structs as a free performance win

### Question

How does garbage collection work in .NET, and what problems does it not solve?

### Short Answer

The .NET garbage collector automatically reclaims memory for unreachable managed objects, typically using generational collection. It reduces manual memory management errors, but it does not manage unmanaged resources or eliminate performance problems caused by excessive allocation.

### Deep Explanation

The GC assumes most objects die young, so it collects newer generations more frequently. That works well for short-lived request data, but heavy allocation can still cause pauses, CPU overhead, and memory pressure. It also does nothing for open sockets, file handles, native buffers, or database connections, which must still be disposed correctly.

### Senior-Level Notes

The senior version of this answer usually includes one concrete symptom: allocation-heavy request paths, large object churn, or background processing spikes causing latency or CPU surprises. That sounds more credible than saying "GC can pause the app" in the abstract.

### Common Mistakes

- saying GC prevents memory leaks in all forms
- forgetting unmanaged resources
- discussing GC as if it were free

### Question

When would you use an abstract class instead of an interface?

### Short Answer

Use an interface when you need a contract that multiple unrelated types can implement. Use an abstract class when you need shared state or shared behavior and there is a meaningful inheritance relationship.

### Deep Explanation

Interfaces define capabilities. Abstract classes define a base type with partial implementation. In modern C#, interfaces can have default members, but that does not make them a replacement for inheritance. Abstract classes are still useful when lifecycle management, protected helpers, or shared invariants belong in a base implementation.

### Senior-Level Notes

A strong answer mentions design stability. Interfaces are better for substitution and testing boundaries. Abstract classes can become rigid if they encode too much policy and force inheritance where composition would be cleaner.

### Common Mistakes

- always choosing interfaces for testability without thinking about design
- using inheritance only to reuse a few lines of code
- ignoring composition as an alternative

### Question

What problem do nullable reference types solve?

### Short Answer

Nullable reference types add compiler-supported nullability analysis so code expresses whether a reference is expected to be null. They reduce a large class of runtime null-reference bugs by shifting them into compile-time feedback.

### Deep Explanation

Before nullable reference types, all reference types were effectively nullable, and intent lived only in comments or conventions. With nullable annotations, APIs can state whether null is valid, and the compiler can warn when code violates those assumptions. This improves contracts across application layers, especially DTOs, domain models, and service boundaries.

### Senior-Level Notes

This is not just syntax hygiene. It improves maintainability in large codebases because the compiler helps preserve intent during refactors. Strong candidates also mention that adopting it incrementally requires discipline around annotations and null-forgiving operators.

### Common Mistakes

- treating nullable warnings as noise
- using `!` everywhere instead of fixing contracts
- assuming runtime behavior changes automatically

## Resource Management And API Design

### Question

Why do `IDisposable` and `IAsyncDisposable` matter in backend applications?

### Short Answer

They define deterministic cleanup for resources that the garbage collector does not manage well, such as file handles, network streams, and database connections. `IAsyncDisposable` matters when cleanup itself is asynchronous, such as flushing buffered I/O over the network.

### Deep Explanation

The GC handles memory reclamation, but it is non-deterministic. Backend services cannot wait arbitrarily long to release scarce resources. `using` and `await using` make cleanup explicit and predictable. This becomes especially important in high-concurrency services where connection pools and file handles are limited.

### Senior-Level Notes

In interviews, it is useful to mention that lifetime ownership should be clear. Objects created by DI containers are often disposed by the container, while manually created resources are the caller's responsibility.

### Common Mistakes

- saying GC makes disposal unnecessary
- disposing services owned by the container
- forgetting asynchronous cleanup paths

### Question

What is the difference between `IEnumerable<T>` and `IQueryable<T>`?

### Short Answer

`IEnumerable<T>` represents in-memory iteration, while `IQueryable<T>` represents a query that can be translated by a provider such as EF Core. The distinction matters because operations on `IQueryable<T>` may become SQL, while operations on `IEnumerable<T>` run in process.

### Deep Explanation

With `IQueryable<T>`, expression trees are captured so the provider can translate them. If you materialize too early with `ToList()`, later filtering happens in memory instead of in the database. That can turn a selective SQL query into a full table load followed by application-side filtering.

### Senior-Level Notes

The stronger answer is usually architectural, not theoretical: "I am fine composing `IQueryable<T>` close to the persistence layer, but I do not want controllers or upper application layers accidentally changing SQL shape." That shows you understand both the power and the risk.

### Common Mistakes

- saying `IQueryable<T>` is always better
- materializing too early
- exposing provider-specific query composition across architectural boundaries

### Question

When should you use records in C#?

### Short Answer

Records are useful when value-based equality and concise immutable-style modeling are desired, especially for DTOs, commands, events, and simple value objects. They are less appropriate when the type has rich identity-based lifecycle behavior.

### Deep Explanation

A record emphasizes that the data carried by the object matters more than object identity. That fits message contracts and immutable configuration-like models. For entities with lifecycle and mutable behavior, regular classes are often clearer because identity and invariants matter more than structural equality.

### Senior-Level Notes

A strong answer distinguishes domain entities from transport models. Using records blindly for everything can create accidental equality semantics or mutation confusion when teams do not understand the generated behavior.

### Common Mistakes

- using records only because they are newer syntax
- ignoring equality semantics
- modeling mutable entities as records without clear intent

## Error Handling And Engineering Judgment

### Question

How should exceptions be used in backend applications?

### Short Answer

Exceptions should represent exceptional or unexpected conditions, not normal control flow. Backend applications should handle them centrally, log them appropriately, and map them to stable API or application-level responses.

### Deep Explanation

Throwing exceptions for expected validation failures or common branching paths makes systems noisy and more expensive under load. Good design distinguishes domain validation, recoverable operational failures, and true unexpected errors. At the application edge, central exception handling keeps responses consistent and avoids duplicated try-catch blocks.

### Senior-Level Notes

In a senior interview, it helps to separate three cases clearly: validation and domain failures, operational failures such as timeouts, and true bugs. That framing leads naturally to different logging, retry, and client-response behavior instead of one generic "catch and log" answer.

### Common Mistakes

- using exceptions for standard branching logic
- swallowing exceptions without enough context
- leaking internal details to clients

### Question

What makes code feel idiomatic in modern .NET rather than merely functional?

### Short Answer

Idiomatic .NET code uses the framework conventions well: async APIs where I/O is asynchronous, dependency injection for composition, clear nullability contracts, predictable disposal, structured logging, and standard collection and naming patterns. It fits the runtime and ecosystem rather than fighting them.

### Deep Explanation

Many codebases work but carry unnecessary friction because they ignore platform conventions. Examples include blocking on async work, building custom DI containers, returning ad hoc error payloads, or tightly coupling business logic to HTTP concerns. Idiomatic code is easier to read, test, debug, and evolve because it matches how the rest of the .NET ecosystem behaves.

### Senior-Level Notes

This is where judgment shows up. Senior engineers know which conventions are worth following by default and when breaking them is justified. The key is to optimize for maintainability and predictability, not novelty.

### Common Mistakes

- treating personal preference as architecture
- inventing custom infrastructure where platform defaults are sufficient
- ignoring consistency across a codebase

## Advanced Senior-Level Questions

### Question

When do allocation and memory optimizations actually matter in a .NET backend service?

### Short Answer

They matter when a path is hot enough that allocation rate affects latency, CPU, or GC pressure. I would not optimize allocations by default, but I would absolutely care in high-throughput request paths, serializers, parsers, and background workers processing large volumes.

### Deep Explanation

Many allocation discussions are premature. The right trigger is evidence: high GC time, allocation-heavy traces, large object churn, or throughput collapse under load. Once that signal exists, the work usually focuses on reducing needless intermediate objects, avoiding excessive string manipulation, and keeping data shapes appropriate for the workload.

### Senior-Level Notes

The strongest answer makes the trade-off explicit: allocation optimizations often reduce readability, so they belong in measured hotspots, not across the whole codebase.

### Common Mistakes

- optimizing allocations without profiling
- assuming lower allocation always means better overall design
- making ordinary business code harder to maintain for marginal wins

### Question

How do you think about compatibility when building shared .NET libraries used by multiple services?

### Short Answer

I think about shared libraries as contracts, not just code reuse. Once several services depend on them, breaking changes, transitive dependency problems, and rollout sequencing become more important than saving a few duplicated lines.

### Deep Explanation

Shared libraries can improve consistency for cross-cutting concerns such as logging, auth helpers, or common primitives. They become risky when they centralize volatile business logic or force synchronized upgrades across many services. The more widely used the package is, the more conservative I become about public surface changes and dependency churn.

### Senior-Level Notes

A senior answer usually mentions versioning discipline, narrow scope, and resisting the temptation to turn a shared package into a distributed monolith in NuGet form.

### Common Mistakes

- moving unstable business logic into shared packages
- publishing breaking changes casually
- ignoring transitive dependency and upgrade impact

### Question

When are reflection-heavy patterns a problem in .NET systems?

### Short Answer

They become a problem when startup time, runtime overhead, debuggability, trimming, or AOT compatibility matters. Reflection is useful, but I want it to be intentional because it hides costs and often makes behavior harder to trace.

### Deep Explanation

Reflection can simplify frameworks, registration, serialization, and plugin models. The downside is that it can add startup scanning cost, complicate static analysis, and create surprises when the application is trimmed or optimized for native deployment. In many cases, explicit registration or source generation is a better long-term fit.

### Senior-Level Notes

The better interview answer is not "reflection is slow." It is "reflection is fine when the flexibility is worth the cost, but I do not want critical paths or core app wiring to depend on hidden magic if observability and performance matter."

### Common Mistakes

- treating reflection as either always bad or always harmless
- ignoring startup and deployment implications
- choosing cleverness over traceability

## Related Topics

- [../fundamentals/csharp-fundamentals.md](../fundamentals/csharp-fundamentals.md)
- [../fundamentals/oop.md](../fundamentals/oop.md)
- [../framework/aspnet-core.md](../framework/aspnet-core.md)
- [../framework/entity-framework.md](../framework/entity-framework.md)
