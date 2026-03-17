# ASP.NET Core

## Overview

ASP.NET Core is the main web framework for building modern .NET backend services. It provides hosting, dependency injection, configuration, logging, middleware, and endpoint handling in a unified application model.

## Core Concepts

- `WebApplicationBuilder` and `WebApplication` define the host and request pipeline.
- Middleware runs in order and can short-circuit or enrich request processing.
- Endpoints can be implemented with Minimal APIs or controllers depending on the use case.
- Built-in DI, configuration, logging, and health checks should be the default starting point before adding custom infrastructure.

## Why It Matters In Real Systems

Backend applications live at the boundary between network, application logic, and infrastructure. ASP.NET Core is where authentication, authorization, observability, caching, request limits, and error handling come together. Misunderstanding the framework often leads to incorrect middleware ordering, broken security behavior, or code that works locally but becomes hard to operate once proxies, retries, and real traffic patterns appear.

## Practical Example

A well-structured service keeps `Program.cs` readable, registers feature services intentionally, and uses centralized error handling. In practice, that usually means the HTTP layer stays thin and cross-cutting behavior is obvious at startup:

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
builder.Services.AddProblemDetails();
builder.Services.AddDbContext<AppDbContext>();

var app = builder.Build();
app.UseExceptionHandler();
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

The value is not the syntax. It is that a new engineer can see where routing, DI, auth, and error behavior are configured without hunting across the codebase.

## Common Pitfalls

- bloating `Program.cs` with business logic
- mixing Minimal APIs and controllers arbitrarily
- placing authentication and authorization in the wrong order
- burying important operational behavior inside extension methods no one can trace
- building custom infrastructure before using framework defaults well

## Interview Takeaways

- Explain request flow in terms of middleware and endpoint execution.
- Mention service registration, error handling, and operational concerns.
- Show that you understand when framework conventions help and when customization is justified.

## Related Topics

- [middleware-and-pipeline.md](middleware-and-pipeline.md)
- [entity-framework.md](entity-framework.md)
- [../fundamentals/dependency-injection.md](../fundamentals/dependency-injection.md)
- [../interview/aspnet-core.md](../interview/aspnet-core.md)
