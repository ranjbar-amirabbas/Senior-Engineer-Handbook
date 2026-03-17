# Middleware And Pipeline

## Overview

Middleware is the mechanism ASP.NET Core uses to compose request and response behavior. Understanding pipeline order is essential for building secure, observable, and predictable APIs.

## Core Concepts

- Middleware runs in registration order on the way in and reverse order on the way out.
- Each middleware component can continue, modify, or short-circuit the request.
- Routing, authentication, authorization, exception handling, and endpoint execution all depend on correct ordering.
- Custom middleware should handle cross-cutting concerns, not application business logic.

## Why It Matters In Real Systems

Pipeline bugs are often subtle. A misplaced authentication middleware can break authorization. An incorrectly ordered exception handler can expose raw failures. Missing proxy header handling can affect redirect behavior, link generation, and security assumptions behind a load balancer.

## Practical Example

Typical API ordering looks like this:

1. forwarded headers when running behind a proxy
2. centralized exception handling
3. HTTPS redirection
4. routing or endpoint setup as needed
5. authentication
6. authorization
7. mapped endpoints

The important principle is dependency order, not memorizing one exact list for every service.

## Common Pitfalls

- putting business workflows into middleware
- adding middleware without understanding whether it runs before or after auth
- forgetting environment-specific behavior such as developer exception pages
- assuming all cross-cutting logic belongs in middleware rather than filters, handlers, or services

## Interview Takeaways

- Explain middleware as request composition.
- Mention why ordering matters operationally.
- Show that you know where middleware is appropriate and where it is not.

## Related Topics

- [aspnet-core.md](aspnet-core.md)
- [../interview/aspnet-core.md](../interview/aspnet-core.md)
- [../../architecture/fundamentals/rest-api-design.md](../../architecture/fundamentals/rest-api-design.md)
