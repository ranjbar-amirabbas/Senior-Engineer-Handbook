# Event-Driven Architecture Interview Questions

## Intro

These questions focus on the design and trade-offs of asynchronous event-based systems.

## Messaging

### Question

What is the difference between an event and a command?

### Short Answer

A command asks for an action to be performed. An event states that something already happened.

### Deep Explanation

Commands imply intended behavior and ownership by a specific handler. Events describe facts that other parts of the system may react to independently.

### Senior-Level Notes

Strong candidates mention that disguising commands as events creates coupling confusion.

### Common Mistakes

- using event terminology for imperative workflows

### Question

Why is idempotency important for consumers?

### Short Answer

Because messages may be redelivered, retried, or processed after uncertain failures. Consumers must avoid producing duplicate side effects.

### Deep Explanation

At-least-once delivery is common, so consumers need durable safeguards such as deduplication keys, unique constraints, or state-aware processing.

### Senior-Level Notes

Senior answers connect idempotency to business consequences like duplicate charges or duplicate shipments.

### Common Mistakes

- trusting broker guarantees alone

### Question

What makes event debugging difficult?

### Short Answer

Asynchronous execution, lag, retries, independent consumers, and weak traceability make it hard to reconstruct what happened and why.

### Deep Explanation

Good debugging requires correlation IDs, payload visibility, traceable retries, dead-letter handling, and clear ownership of event contracts.

### Senior-Level Notes

Strong answers emphasize observability as part of the architecture, not an afterthought.

### Common Mistakes

- assuming logs in one service are enough

### Question

When is event-driven architecture a poor fit?

### Short Answer

It is a poor fit when the workflow needs immediate synchronous confirmation, the team lacks operational maturity, or the complexity outweighs the decoupling benefit.

### Deep Explanation

Not every workflow benefits from asynchronous coordination. Sometimes direct request-response is simpler, clearer, and operationally safer.

### Senior-Level Notes

Senior candidates show restraint and context-based design judgment.

### Common Mistakes

- using events by default for all inter-service communication
