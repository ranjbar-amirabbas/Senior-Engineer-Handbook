# ASP.NET Core Interview Questions

## Intro

ASP.NET Core interview questions usually test whether you understand request processing, dependency injection, middleware order, API design, and production concerns. Strong answers are concrete and operational.

## Request Pipeline

### Question

How does a request flow through an ASP.NET Core application?

### Short Answer

A request enters the host, passes through middleware in order, gets matched to an endpoint, executes application logic, and then flows back through the middleware pipeline for the response. Middleware order is critical because each component can observe, modify, short-circuit, or depend on earlier work.

### Deep Explanation

Exception handling, HTTPS redirection, authentication, authorization, static files, routing, and endpoint execution all rely on predictable ordering. A mistake in that order can cause broken auth, missing headers, incorrect error handling, or unexpected bypass behavior.

### Senior-Level Notes

Senior candidates mention that middleware is part of system behavior, not just framework ceremony. Request tracing, correlation, and problem-details mapping usually belong at this boundary.

### Common Mistakes

- describing middleware as an unordered list
- placing authorization before authentication
- putting business logic into custom middleware without need

### Question

What is the role of dependency injection in ASP.NET Core?

### Short Answer

Dependency injection composes the application by wiring controllers, handlers, services, and infrastructure through explicit dependencies. It improves separation of concerns and makes object lifetimes manageable.

### Deep Explanation

ASP.NET Core uses built-in DI to create request-scoped services, long-lived singletons, and transient components. This helps keep framework boundaries clean and lets cross-cutting infrastructure such as logging, options, and HTTP clients be shared consistently. The value is not "testability only"; it is controlled composition.

### Senior-Level Notes

A strong answer includes lifetime correctness. Many production bugs come from capturing scoped services in singletons or misusing long-lived state.

### Common Mistakes

- reducing DI to unit-test convenience
- injecting too many dependencies into one class
- misconfiguring service lifetimes

### Question

When would you choose Minimal APIs versus controllers?

### Short Answer

Choose Minimal APIs for focused, low-ceremony HTTP surfaces. Choose controllers when you need richer MVC conventions, attribute-driven behavior, or the existing application is already organized around controllers.

### Deep Explanation

This is not about which style is more modern. It is about fit. Minimal APIs work well for small to medium APIs and clear endpoint grouping. Controllers remain useful for larger codebases that benefit from filters, conventions, or an established controller model.

### Senior-Level Notes

Senior candidates should say they would preserve consistency inside an existing codebase unless there is a meaningful reason to mix styles.

### Common Mistakes

- treating Minimal APIs as automatically superior
- choosing style based on trend rather than use case
- mixing patterns in the same feature without discipline

### Question

How should errors be handled in an ASP.NET Core API?

### Short Answer

Errors should be handled centrally and exposed as stable API responses, usually using Problem Details or a similar consistent contract. Internal exceptions should not leak directly to clients.

### Deep Explanation

Expected validation failures, authorization failures, not-found cases, and unexpected exceptions should be distinguished. Centralized handling improves consistency and observability. It also removes duplicated try-catch blocks from controllers or handlers.

### Senior-Level Notes

Senior answers mention logging policy. Not every 4xx is an error log, and not every exception is a client-visible message. The response contract and telemetry policy should align.

### Common Mistakes

- returning inconsistent error payloads
- catching exceptions everywhere
- leaking stack traces or internal messages
