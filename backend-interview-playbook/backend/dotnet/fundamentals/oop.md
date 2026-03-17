# Object-Oriented Programming

## Overview

Object-oriented programming is useful in backend systems when it helps model behavior, protect invariants, and isolate change. It becomes harmful when it introduces ceremony, weak abstractions, or inheritance-heavy designs that make the system harder to evolve.

## Core Concepts

- Encapsulation keeps state changes behind meaningful operations.
- Abstraction lets higher-level code depend on contracts instead of concrete details.
- Inheritance is one tool for reuse and specialization, but composition is often safer.
- Polymorphism is valuable when the variation point is real, such as multiple payment providers or storage strategies.

## Why It Matters In Real Systems

In backend services, OOP quality shows up in domain modeling, testability, and change safety. If entities are just data bags and all behavior lives in services, invariants drift. If inheritance is used for convenience, changes in one branch of the hierarchy can create surprising regressions elsewhere.

## Practical Example

An `Order` object that exposes `MarkAsPaid()` is usually clearer than exposing a public `Status` setter and relying on calling code to manage legal transitions. The method can enforce domain rules such as only allowing payment from the `Pending` state and recording a timestamp consistently.

## Common Pitfalls

- using inheritance to share incidental code
- creating interfaces for every class regardless of need
- moving business rules out of the model until it becomes anemic
- mistaking more objects for better design

## Interview Takeaways

- Emphasize encapsulation and composition over textbook pillar recitation.
- Explain when an abstraction earns its cost.
- Connect OOP to maintainability and domain boundaries, not only syntax.

## Related Topics

- [../architecture/ddd.md](../architecture/ddd.md)
- [../architecture/design-patterns.md](../architecture/design-patterns.md)
- [../interview/oop.md](../interview/oop.md)
