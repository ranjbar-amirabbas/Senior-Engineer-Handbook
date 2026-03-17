# OOP Interview Questions

## Intro

Object-oriented questions are usually used to test whether you understand modeling and maintainability rather than whether you can recite definitions. Strong answers show where OOP helps and where it becomes accidental complexity.

## Principles

### Question

What are the main pillars of object-oriented programming?

### Short Answer

The main pillars are encapsulation, abstraction, inheritance, and polymorphism. In practice, encapsulation and composition usually matter more in backend systems than inheritance-heavy designs.

### Deep Explanation

Encapsulation keeps invariants protected. Abstraction allows systems to depend on contracts instead of details. Inheritance enables specialization, while polymorphism allows one interface to support multiple implementations. Mature codebases use these ideas selectively rather than forcing everything into class hierarchies.

### Senior-Level Notes

Senior candidates often point out that the interview value is not naming the four pillars. It is showing how they influence maintainability and change tolerance in real code.

### Common Mistakes

- giving textbook definitions only
- overusing inheritance
- ignoring composition

### Question

What is composition over inheritance, and why is it recommended?

### Short Answer

Composition means building behavior by assembling collaborating objects instead of forcing behavior through class inheritance. It is recommended because it usually leads to looser coupling and fewer brittle hierarchies.

### Deep Explanation

Inheritance creates strong compile-time coupling and can push unrelated classes into the same base design. Composition keeps responsibilities more explicit and easier to swap. In dependency-injected .NET applications, composition is usually the more practical tool for assembling behavior.

### Senior-Level Notes

A strong answer notes that inheritance is still useful when there is a true is-a relationship with stable shared behavior. The problem is not inheritance itself but casual or convenience-driven inheritance.

### Common Mistakes

- treating inheritance as code reuse first
- rejecting inheritance entirely
- ignoring testability and change impact

### Question

What is polymorphism in practical backend design?

### Short Answer

Polymorphism allows different implementations to be used through a common contract. In backend systems, it often appears through interfaces, strategy implementations, and infrastructure adapters.

### Deep Explanation

For example, a payment workflow may depend on an `IPaymentGateway` contract with different implementations for providers, test doubles, or region-specific rules. This supports substitution and reduces direct coupling. The key is that the abstraction should reflect a real variation point, not be introduced mechanically everywhere.

### Senior-Level Notes

Senior candidates usually mention that unnecessary abstraction creates indirection without value. Polymorphism is powerful when behavior truly varies or may need isolation for testing or deployment reasons.

### Common Mistakes

- creating interfaces for every class by default
- confusing abstraction with extra files
- ignoring whether the variation point is real

### Question

How do OOP principles influence domain modeling?

### Short Answer

OOP principles help put state and behavior together, which is useful for enforcing invariants and representing business concepts clearly. Good domain modeling uses OOP to express the problem space, not just to organize folders.

### Deep Explanation

When entities expose every setter publicly and logic lives elsewhere, the model stops protecting the business rules. Encapsulation keeps invalid state transitions harder to create. Abstraction and composition also help isolate external systems from the domain.

### Senior-Level Notes

This is where DDD and OOP intersect. Strong answers mention that objects should earn their complexity by protecting rules, not simply exist because the language is object-oriented.

### Common Mistakes

- treating entities as property bags
- assuming more classes means better design
- ignoring aggregate and boundary design
