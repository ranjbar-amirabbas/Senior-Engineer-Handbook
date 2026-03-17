# REST API Design

## Overview

REST API design is about building clear, predictable HTTP interfaces that are easy to consume, evolve, observe, and secure. Good API design reduces accidental complexity for both clients and backend teams.

## Core Concepts

- Resource modeling should reflect stable business concepts rather than internal tables.
- HTTP methods, status codes, and error contracts should be consistent and intention-revealing.
- Idempotency, pagination, filtering, and versioning are practical design concerns, not afterthoughts.
- API design should separate public contract shape from internal persistence shape.

## Why It Matters In Real Systems

APIs are long-lived contracts. A poorly designed API creates client coupling, inconsistent behavior, and migration pain. Small inconsistencies around status codes, naming, or error responses multiply quickly as the surface area grows.

## Practical Example

`POST /orders` should return a stable creation response and a location for the created resource when appropriate. Validation errors should use a consistent error contract rather than one-off ad hoc JSON shapes. If the operation may be retried by clients, an idempotency strategy should be part of the design.

For example, if payment creation can be retried by a mobile client after a timeout, the API contract should define how duplicate requests are recognized and what response the client receives on a retry. That is API design, not only backend implementation detail.

## Common Pitfalls

- designing endpoints directly from database tables
- returning inconsistent error shapes
- overloading one endpoint with multiple behaviors
- exposing internal workflow complexity directly in the public contract
- ignoring backward compatibility and client migration

## Interview Takeaways

- Connect API design to consumer experience and long-term maintenance.
- Mention idempotency, versioning, and consistent error handling.
- Show that HTTP semantics are tools, not trivia.

## Related Topics

- [system-design-basics.md](system-design-basics.md)
- [../examples/api-idempotency.md](../examples/api-idempotency.md)
- [../../dotnet/framework/aspnet-core.md](../../dotnet/framework/aspnet-core.md)
