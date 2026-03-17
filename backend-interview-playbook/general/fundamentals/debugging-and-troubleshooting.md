# Debugging And Troubleshooting

## Overview

Debugging is the process of narrowing uncertainty until the real cause of a problem is known. Strong backend engineers debug systematically rather than by trying random fixes until something changes.

## Core Concepts

- Start from symptoms, then gather evidence before changing the system.
- Reproduce the issue when possible, but do not depend on perfect reproduction before learning from telemetry.
- Narrow the search space across code, configuration, infrastructure, and dependencies.
- Distinguish correlation from causation, especially after deployments or incidents.

## Why It Matters In Real Systems

Backend incidents often span application code, databases, queues, proxies, and deployment changes. A disciplined troubleshooting approach reduces mean time to resolution and prevents repeated guess-driven incidents.

## Practical Example

If request latency spikes after a deployment, check release changes, traces, dependency timing, error rates, and resource saturation before rolling back blindly. A realistic sequence is:

1. confirm the user-visible symptom
2. compare pre- and post-deploy telemetry
3. find the slow boundary: app code, database, dependency, or infrastructure
4. choose mitigation only after the likely failure mode is narrowed

The problem may be application code, a probe change, a thread pool issue, or a slow downstream dependency. Good debugging reduces that uncertainty step by step.

## Common Pitfalls

- restarting systems before collecting enough evidence
- assuming the latest changed component is always the cause
- relying only on logs and ignoring metrics and traces
- changing multiple variables during the same investigation
- applying fixes that mask symptoms but preserve root cause

## Interview Takeaways

- Describe debugging as a hypothesis-driven process.
- Mention telemetry, rollback judgment, and narrowing scope.
- Show that good debugging is about method, not heroics.

## Related Topics

- [testing-fundamentals.md](testing-fundamentals.md)
- [../examples/debugging-checklist.md](../examples/debugging-checklist.md)
- [../../backend/devops/fundamentals/monitoring-and-observability.md](../../backend/devops/fundamentals/monitoring-and-observability.md)
