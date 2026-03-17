# REST API Interview Questions

## Intro

These questions focus on contract design, HTTP semantics, and practical API trade-offs.

## API Design

### Question

What makes an API easy to consume?

### Short Answer

Consistency, clear naming, predictable error handling, stable contracts, and sensible pagination and filtering rules.

### Deep Explanation

Consumers struggle less when resources behave consistently and error responses are structured in a standard way. Good APIs reduce guesswork.

### Senior-Level Notes

Senior candidates mention operational concerns such as traceability, versioning, and idempotency.

### Common Mistakes

- focusing only on endpoint naming

### Question

Why does idempotency matter in APIs?

### Short Answer

Because clients retry requests when networks fail or responses are uncertain. Idempotency prevents duplicate side effects for the same logical operation.

### Deep Explanation

This is especially important for create or payment-like operations where duplicate execution has real business cost.

### Senior-Level Notes

Strong answers mention idempotency keys and durable enforcement.

### Common Mistakes

- assuming HTTP verb alone guarantees safe retry behavior

### Question

How should APIs report errors?

### Short Answer

They should return consistent, stable error contracts with meaningful status codes and actionable client-facing detail without leaking internals.

### Deep Explanation

A consistent error model helps both humans and clients handle failure predictably. Random JSON shapes and inconsistent status code usage create long-term integration pain.

### Senior-Level Notes

Senior answers mention Problem Details or an equivalent consistent contract.

### Common Mistakes

- returning stack traces or framework-specific internals

### Question

When should you version an API?

### Short Answer

Version when compatibility matters and you need to evolve a contract in ways that could break existing consumers.

### Deep Explanation

Not every internal change requires versioning, but changes to resource shape, semantics, or client expectations often do. Good versioning is deliberate rather than automatic.

### Senior-Level Notes

Strong answers mention additive change first and deprecation planning.

### Common Mistakes

- versioning every tiny change or never versioning at all
