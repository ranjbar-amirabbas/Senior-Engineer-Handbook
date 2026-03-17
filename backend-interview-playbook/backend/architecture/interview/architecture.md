# Architecture Interview Questions

## Intro

These questions focus on high-level design choices and engineering judgment.

## Trade-Offs

### Question

How do you choose between simplicity and flexibility in architecture?

### Short Answer

Default to the simplest design that meets current needs, then add flexibility where change is likely and expensive to retrofit.

### Deep Explanation

Every abstraction has a carrying cost. The goal is not maximum simplicity or maximum flexibility in isolation. It is the right balance for the system's expected evolution.

### Senior-Level Notes

Strong answers mention change frequency, team boundaries, and operational burden.

### Common Mistakes

- optimizing for hypothetical futures everywhere

### Question

What is coupling in backend architecture?

### Short Answer

Coupling is the degree to which one part of the system depends on another part's details, timing, or data shape.

### Deep Explanation

Coupling can be structural, temporal, or operational. Good architecture reduces unnecessary coupling so parts can change or fail more independently.

### Senior-Level Notes

Senior candidates mention that some coupling is legitimate. The goal is to manage it consciously.

### Common Mistakes

- treating all coupling as equally bad

### Question

What makes a boundary a good boundary?

### Short Answer

A good boundary aligns with responsibility, ownership, and change patterns while minimizing unnecessary coordination across it.

### Deep Explanation

Boundaries are useful when they contain coherent behavior and reduce accidental impact from unrelated changes. Weak boundaries mostly move complexity around.

### Senior-Level Notes

Strong answers mention language, workflow cohesion, and data ownership.

### Common Mistakes

- drawing boundaries based only on implementation convenience

### Question

How do you know an architecture is not working?

### Short Answer

Frequent cross-cutting changes, hard deployments, slow debugging, fragile integrations, and unclear ownership are common signals.

### Deep Explanation

Architecture problems often show up operationally and organizationally before they are obvious in diagrams. If every feature touches everything, the structure is not buying enough isolation.

### Senior-Level Notes

Senior candidates mention measurable symptoms rather than abstract dissatisfaction.

### Common Mistakes

- judging architecture only by aesthetics
