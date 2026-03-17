# Clean Architecture

## Overview

Clean Architecture is a structural approach that tries to keep business rules independent from external frameworks and infrastructure. Its value is greatest when it preserves boundaries and keeps change localized. Its downside is that it can become ceremony-heavy when applied mechanically.

## Core Concepts

- Inner layers should not depend on outer infrastructure details.
- Application logic should not be tightly coupled to HTTP, EF Core, or messaging frameworks.
- Boundaries are usually expressed through interfaces, DTOs, commands, or use-case services.
- The architecture is successful only if the boundaries are meaningful, not merely numerous.

## Why It Matters In Real Systems

Large backend systems change in uneven ways. APIs evolve, persistence choices change, integrations get replaced, and business rules become more complex. Clear boundaries reduce the blast radius of change. At the same time, too many abstractions can slow down delivery and obscure the real flow of the application.

## Practical Example

A request enters through an API endpoint, is translated into an application command, executes business logic through a domain model or service, and persists state through an abstraction implemented with EF Core. The benefit is not purity for its own sake. It is that the use case remains readable even if the outer technology changes.

## Common Pitfalls

- creating excessive layers for simple systems
- hiding straightforward flows behind generic abstractions
- confusing independence from frameworks with pretending frameworks do not exist
- copying template architecture without checking if the domain complexity justifies it

## Interview Takeaways

- Discuss trade-offs, not only the dependency rule.
- Mention when Clean Architecture is helpful and when it becomes overengineering.
- Show how you would keep use cases explicit and infrastructure replaceable enough, not perfectly abstract.

## Related Topics

- [ddd.md](ddd.md)
- [design-patterns.md](design-patterns.md)
- [clean-code.md](clean-code.md)
- [../../architecture/fundamentals/system-design-basics.md](../../architecture/fundamentals/system-design-basics.md)
