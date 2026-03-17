# CI/CD Interview Questions

## Intro

These questions focus on how teams automate safe delivery and validate changes before production.

## Delivery

### Question

Why is CI important?

### Short Answer

CI gives fast feedback on whether changes build, integrate, and pass key checks before they spread further.

### Deep Explanation

Without CI, integration failures are discovered later and in larger batches, which increases risk and slows delivery.

### Senior-Level Notes

Strong answers mention confidence, not only automation.

### Common Mistakes

- reducing CI to "runs tests"

### Question

What makes a good deployment pipeline?

### Short Answer

A good pipeline is fast enough for frequent use, strict enough to catch meaningful failures, and aligned with the risk profile of the service.

### Deep Explanation

The pipeline should validate the right things at the right stage and support safe promotion or rollback.

### Senior-Level Notes

Senior candidates mention build reproducibility, environment parity, and rollback strategy.

### Common Mistakes

- making one slow monolithic pipeline for every check

### Question

Why are staging environments not enough by themselves?

### Short Answer

Because staging rarely matches production perfectly in traffic, data shape, and operational conditions.

### Deep Explanation

Staging is valuable, but production-safe rollout strategies and observability are still required because some issues appear only under real conditions.

### Senior-Level Notes

Strong answers mention canaries and production telemetry.

### Common Mistakes

- assuming a green staging deploy guarantees a safe production deploy

### Question

What should happen when a deployment fails?

### Short Answer

The team should have a defined mitigation path such as rollback, traffic shift, feature disablement, or controlled forward fix based on blast radius and data implications.

### Deep Explanation

Failure handling is part of deployment design. Waiting to invent the recovery path during an incident is avoidable risk.

### Senior-Level Notes

Strong answers mention data compatibility and operational decision-making.

### Common Mistakes

- relying on rollback even when schema changes make it unsafe
