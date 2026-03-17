# Kubernetes

## Overview

Kubernetes is an orchestration platform for deploying and managing containers at scale. Its value comes from scheduling, service discovery, rollout control, and operational automation, but it also introduces a significant abstraction layer that teams must understand.

## Core Concepts

- Pods are the basic deployable units, usually holding one application container plus any tightly coupled sidecars.
- Deployments manage rollout and replacement for stateless workloads.
- Services provide stable discovery and load balancing to pod sets.
- Liveness, readiness, and startup probes affect traffic routing and recovery behavior.

## Why It Matters In Real Systems

Kubernetes can improve standardization and resilience, but it also creates new operational failure modes. Misconfigured probes, resource limits, and rollout settings can make healthy applications unstable or hide real problems behind restarts.

## Practical Example

A backend API may run as a Deployment behind a Service with readiness probes ensuring it does not receive traffic before startup is complete. Resource requests and limits should reflect realistic workload behavior so the scheduler and runtime make sensible decisions.

## Common Pitfalls

- using Kubernetes without enough observability
- treating restarts as fixes instead of symptoms
- misconfiguring probes and causing self-inflicted outages
- ignoring resource requests and limits

## Interview Takeaways

- Explain the core objects in terms of behavior and operations.
- Mention rollout, probes, and resource management.
- Show that orchestration adds both capability and complexity.

## Related Topics

- [docker.md](docker.md)
- [deployment-strategies.md](deployment-strategies.md)
- [../examples/kubernetes-debugging.md](../examples/kubernetes-debugging.md)
