# CI/CD

## Overview

CI/CD is the set of practices and automation that moves code changes from commit to production safely and repeatedly. For backend teams, the value is reduced risk, faster feedback, and more reliable delivery.

## Core Concepts

- Continuous integration validates changes early with builds, tests, and quality checks.
- Continuous delivery keeps software in a releasable state with automated deployment steps and clear promotion gates.
- Pipelines should reflect risk: fast feedback early, deeper validation before release.
- Release safety depends on both automation and rollback or mitigation strategy.

## Why It Matters In Real Systems

Teams without disciplined CI/CD often batch risky changes, discover failures late, and make deployments stressful. A good pipeline shortens feedback loops and improves confidence in operational changes, not just application code.

## Practical Example

A typical backend pipeline builds the service, runs unit and integration tests, produces a container image, runs dependency or security checks, deploys to staging, and then promotes to production with approval or automated guardrails depending on the environment.

The important part is not the exact number of stages. It is that the pipeline catches cheap failures early and reserves slower, costlier checks for the points where they reduce release risk meaningfully.

## Common Pitfalls

- making the pipeline so slow that engineers bypass it mentally
- relying only on unit tests for release confidence
- treating deployment as separate from testing strategy
- coupling database-breaking changes to the same release without compatibility planning
- having no rollback or mitigation plan

## Interview Takeaways

- Explain the purpose of CI/CD in terms of feedback and risk reduction.
- Mention staging, deployment safety, and rollback thinking.
- Show that operational reliability is part of delivery quality.

## Related Topics

- [deployment-strategies.md](deployment-strategies.md)
- [docker.md](docker.md)
- [../interview/ci-cd.md](../interview/ci-cd.md)
