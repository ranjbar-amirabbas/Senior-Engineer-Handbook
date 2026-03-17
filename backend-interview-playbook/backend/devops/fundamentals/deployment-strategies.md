# Deployment Strategies

## Overview

Deployment strategy is how a team rolls new software into production while managing risk. The best strategy depends on traffic patterns, rollback needs, stateful dependencies, and blast-radius tolerance.

## Core Concepts

- Rolling deployments replace instances gradually.
- Blue-green deployments switch traffic between old and new environments.
- Canary deployments expose a small portion of traffic to the new version first.
- Feature flags separate deployment from release, which reduces risk for some changes.

## Why It Matters In Real Systems

Deployments are a common outage source. Safe rollout strategy limits the blast radius of bad releases and improves recovery speed. The deployment pattern must match both the application and the operational environment.

## Practical Example

A canary release can route a small percentage of traffic to a new API version while monitoring error rate and latency. If the service behaves poorly, traffic is rolled back before the full user base is affected.

## Common Pitfalls

- treating deployment strategy as a platform-only concern
- rolling out schema-breaking application changes without compatibility planning
- relying on rollback without testing rollback paths
- using complex rollout strategies without clear monitoring gates

## Interview Takeaways

- Explain why rollout strategy affects reliability.
- Mention monitoring, compatibility, and rollback.
- Show that deployment safety is part of system design.

## Related Topics

- [ci-cd.md](ci-cd.md)
- [kubernetes.md](kubernetes.md)
- [../../databases/interview/database-design.md](../../databases/interview/database-design.md)
