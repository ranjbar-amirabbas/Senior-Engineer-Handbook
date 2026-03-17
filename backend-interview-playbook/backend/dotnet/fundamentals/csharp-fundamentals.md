# C# Fundamentals

## Overview

C# is the main language surface most .NET backend engineers work with, but interview performance usually depends on understanding the semantics behind the syntax. Strong backend engineers know how type semantics, memory behavior, nullability, and language features affect correctness and maintainability.

## Core Concepts

- Value types and reference types behave differently when copied, mutated, boxed, or stored in collections.
- Nullability annotations communicate intent and reduce runtime surprises during refactoring.
- Exceptions, disposal, records, pattern matching, and generics are not isolated language features. They shape how APIs are designed and consumed.
- Modern C# encourages clear expression, but clarity still matters more than feature density.

## Why It Matters In Real Systems

Backend services do not fail because an engineer forgot the syntax for `switch` expressions. They fail because object ownership is unclear, null handling is inconsistent, resources are leaked, or abstractions hide real costs. Language choices compound across codebases, especially in large teams.

## Practical Example

Consider an API endpoint that returns order summaries. If the response model uses nullable properties without clear intent, consumers and maintainers have to guess. If a large mutable `struct` is used to optimize allocations, it may be copied repeatedly and become harder to reason about than a small immutable class or record.

```csharp
public sealed record OrderSummary(Guid Id, string Number, decimal Total, string Currency);
```

This communicates immutability and value-based equality clearly. It is usually a better fit for a transport model than a mutable entity type.

## Common Pitfalls

- confusing stack-versus-heap rules with actual runtime behavior
- using `!` to silence nullable warnings instead of fixing contracts
- choosing advanced syntax when straightforward code would be clearer
- assuming new language features automatically improve design

## Interview Takeaways

- Explain semantics before syntax.
- Mention how copying, nullability, disposal, and immutability affect APIs.
- Show that you understand where language decisions become production concerns.

## Related Topics

- [oop.md](oop.md)
- [async-await.md](async-await.md)
- [../framework/entity-framework.md](../framework/entity-framework.md)
- [../interview/dotnet.md](../interview/dotnet.md)
