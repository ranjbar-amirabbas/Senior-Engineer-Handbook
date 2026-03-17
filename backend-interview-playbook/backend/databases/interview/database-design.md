# Database Design Interview Questions

## Intro

Database design questions test whether you can model data in a way that stays correct and maintainable as the system grows.

## Modeling

### Question

How do you decide between normalization and denormalization?

### Short Answer

Start normalized for correctness and change safety, then denormalize deliberately when measured read or reporting needs justify it. The choice is a workload trade-off, not a philosophy contest.

### Deep Explanation

Normalization reduces redundancy and update anomalies. Denormalization can reduce join cost and simplify specific read paths. The right answer depends on access patterns, write complexity, and how the team will keep redundant data synchronized.

### Senior-Level Notes

Senior answers mention operational cost. Denormalization affects migrations, backfills, and data repair workflows.

### Common Mistakes

- treating denormalization as premature optimization or as default design

### Question

Why do constraints belong in the database even if the application validates input?

### Short Answer

Because the database is the final authority on persisted data. Constraints protect against bugs, concurrent race conditions, and alternate write paths that bypass application validation.

### Deep Explanation

Application checks are important for user experience and early feedback, but only database constraints can protect data integrity across all execution paths. They also document intent close to the data itself.

### Senior-Level Notes

Senior candidates often mention unique indexes, foreign keys, and check constraints as low-cost defenses with long-term value.

### Common Mistakes

- assuming validation at the API boundary is enough

### Question

How do you model many-to-many relationships?

### Short Answer

Use a join table, and promote it to a richer entity when the relationship itself has attributes or lifecycle. The design should reflect whether the association is just linkage or a domain concept of its own.

### Deep Explanation

A plain join table is fine for simple tagging. But if the relationship carries metadata such as role, effective dates, or status, it should usually become a first-class modeled record.

### Senior-Level Notes

Strong answers connect the schema choice to domain meaning and query shape.

### Common Mistakes

- hiding a rich relationship inside a generic join table

### Question

How do you evolve a schema safely in production?

### Short Answer

Use additive changes first, deploy backward-compatible code, migrate data safely, and remove old structures only after traffic and dependencies have moved off them.

### Deep Explanation

Schema evolution is a deployment concern, not just a migration file. Rolling deployments, old workers, and long-running jobs may still depend on the old shape. Safe evolution often happens in multiple steps.

### Senior-Level Notes

Senior candidates mention sequencing, rollback strategy, and avoiding destructive one-shot changes during busy deploys.

### Common Mistakes

- bundling incompatible schema and application changes together
