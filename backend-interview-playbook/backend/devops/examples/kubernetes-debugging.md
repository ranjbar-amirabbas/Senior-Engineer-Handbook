# Kubernetes Debugging

## Problem Context

Kubernetes issues are often diagnosed at the wrong layer. Teams either blame the application for platform issues or blame the platform for application bugs.

## Recommended Approach

Debug methodically across both layers: application logs and metrics, pod events, probe status, resource pressure, configuration, and dependency health.

## Example Walkthrough

If a pod is restarting, check:

1. the container exit reason
2. recent deployment or configuration changes
3. readiness and liveness probe failures
4. memory pressure or OOM events
5. dependency timeouts or startup issues in application logs

## Trade-Offs

A structured debugging flow takes discipline, but it reduces guesswork and shortens incident duration.

## Common Mistakes

- restarting or redeploying before collecting enough evidence
- checking only Kubernetes events or only application logs
- ignoring node-level resource contention

## Related Topics

- [../fundamentals/kubernetes.md](../fundamentals/kubernetes.md)
- [../fundamentals/monitoring-and-observability.md](../fundamentals/monitoring-and-observability.md)
