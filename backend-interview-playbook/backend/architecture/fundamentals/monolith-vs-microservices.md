# Monolith Vs Microservices

## Overview

Monolith versus microservices is not a maturity ladder. It is a trade-off between simplicity and distributed autonomy. The right choice depends on domain boundaries, team size, deployment needs, and operational maturity.

## Core Concepts

- A modular monolith can preserve simplicity while still maintaining internal boundaries.
- Microservices improve independent deployment and ownership only when boundaries are meaningful.
- Distributed systems add network failure, asynchronous consistency, and observability complexity.
- Architecture choice should reflect current constraints and likely evolution, not fashion.

## Why It Matters In Real Systems

Many teams split too early and end up with distributed complexity they cannot support. Others stay monolithic too long and struggle with team scaling or conflicting release needs. The decision affects engineering speed, incident response, and long-term change cost.

## Practical Example

A product with one team and tightly coupled workflows often benefits more from a well-structured monolith than from multiple services. A larger platform with separate teams and distinct domain capabilities may justify service decomposition when ownership and contracts are clear.

## Common Pitfalls

- treating microservices as the default target
- confusing a tangled monolith with all monoliths
- splitting by technical layer instead of business capability
- ignoring platform and operational readiness

## Interview Takeaways

- Present both options as valid in context.
- Mention modular monoliths as a serious option.
- Explain architecture in terms of coupling, ownership, and operational cost.

## Related Topics

- [system-design-basics.md](system-design-basics.md)
- [scalability-engineering.md](scalability-engineering.md)
- [../../distributed-systems/fundamentals/microservices.md](../../distributed-systems/fundamentals/microservices.md)
