# Dependency Injection

## Overview

Dependency injection is the standard composition model in modern .NET applications. Its purpose is to assemble object graphs cleanly, control lifetimes, and keep dependencies explicit.

## Core Concepts

- Constructor injection is the default because it makes required dependencies obvious.
- Lifetimes matter: singleton, scoped, and transient have different ownership and concurrency implications.
- DI is about composition, not only testability.
- A good dependency graph keeps business logic separate from framework and infrastructure details.

## Why It Matters In Real Systems

Poor DI usage creates subtle production bugs. The most common examples are capturing scoped services in singletons, injecting too many collaborators into one class, or hiding global state behind service registration. Good DI makes the system easier to reason about at startup and at runtime.

## Practical Example

In an ASP.NET Core API, a `DbContext` is typically scoped to the request, while a stateless pricing calculator may be transient or singleton depending on its dependencies. If a singleton service caches a scoped `DbContext`, the application may fail unpredictably under concurrent requests.

## Common Pitfalls

- registering everything as transient without thought
- using a service locator instead of explicit dependencies
- turning classes with too many responsibilities into DI dumping grounds
- assuming any interface is automatically good design

## Interview Takeaways

- Explain why lifetimes matter operationally.
- Mention DI as a composition mechanism, not just a unit-testing trick.
- Connect bad DI design to runtime bugs, not only code style.

## Related Topics

- [../framework/aspnet-core.md](../framework/aspnet-core.md)
- [../framework/entity-framework.md](../framework/entity-framework.md)
- [../interview/aspnet-core.md](../interview/aspnet-core.md)
