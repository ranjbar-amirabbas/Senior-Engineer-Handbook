# Design Patterns

## Overview

Design patterns are recurring solutions to common software design problems. In backend .NET systems, the value of patterns comes from improving clarity and extensibility, not from naming every class after a pattern catalog.

## Core Concepts

- Patterns should address a real problem such as variation, coordination, lifecycle, or complexity management.
- Common backend patterns include strategy, decorator, factory, repository, specification, mediator, and outbox.
- Patterns are easiest to misuse when they are applied as status markers rather than problem-solving tools.
- Composition and explicit behavior usually matter more than pattern purity.

## Why It Matters In Real Systems

Backend systems evolve under integration pressure, business rule growth, and operational needs. Patterns can make those changes safer when they reflect real variation points. They can also make the code harder to follow if they add indirection without value.

## Practical Example

The strategy pattern is useful when tax calculation varies by region or pricing varies by channel. A decorator is useful for adding logging, retries, or caching around a service. Both help when behavior truly varies. Neither helps when a single simple class would do.

## Common Pitfalls

- choosing a pattern before understanding the problem
- turning simple flows into abstract machinery
- introducing mediator-like indirection for every call path
- copying repository or unit-of-work patterns without checking whether EF Core already covers the need

## Interview Takeaways

- Name patterns only when you can explain the problem they solve.
- Show that you can judge when a pattern is too much.
- Connect patterns to maintainability, testing, and operational behavior.

## Related Topics

- [clean-architecture.md](clean-architecture.md)
- [ddd.md](ddd.md)
- [../../distributed-systems/examples/outbox-pattern.md](../../distributed-systems/examples/outbox-pattern.md)
