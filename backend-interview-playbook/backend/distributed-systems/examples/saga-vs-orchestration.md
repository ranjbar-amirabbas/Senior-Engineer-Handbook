# Saga Vs Orchestration

## Problem Context

A business workflow spans multiple services, and no single database transaction can cover the entire operation.

## Recommended Approach

Use choreography when services can react to events independently and the workflow remains understandable. Use orchestration when the workflow needs explicit central coordination, clearer state management, or tighter operational visibility.

## Example Walkthrough

In an order workflow, choreography might publish `OrderPlaced`, `PaymentAuthorized`, and `InventoryReserved` events that each service reacts to. Orchestration would use a dedicated coordinator that sends commands, tracks state, and issues compensating actions when a step fails.

## Trade-Offs

Choreography reduces central coupling but can become hard to trace as the workflow grows. Orchestration improves visibility and control but introduces a coordinator that must be designed and operated carefully.

## Common Mistakes

- choosing choreography for a workflow no one can mentally follow
- turning the orchestrator into a god service
- skipping compensation design entirely

## Related Topics

- [outbox-pattern.md](outbox-pattern.md)
- [../fundamentals/event-driven-architecture.md](../fundamentals/event-driven-architecture.md)
