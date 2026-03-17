# Rate Limiting

## Problem Context

APIs and internal services can be overwhelmed by abusive, buggy, or simply high-volume callers. Without protection, overload can degrade the entire system.

## Recommended Approach

Apply rate limiting at the right boundary with clear policies per client, tenant, or endpoint class. Combine it with observability and user-facing feedback rather than silent failure.

## Example Walkthrough

A public API may apply per-api-key limits with burst allowance and return `429 Too Many Requests` along with retry guidance. Internal rate limiting can also protect expensive downstream dependencies from traffic spikes.

## Trade-Offs

Rate limiting protects system health, but aggressive limits can hurt legitimate high-volume clients or batch workloads. Policies should reflect business priorities.

## Common Mistakes

- using one global limit for all workloads
- rate limiting without clear client identity
- hiding rate-limit behavior from consumers

## Related Topics

- [../fundamentals/high-availability.md](../fundamentals/high-availability.md)
- [../../distributed-systems/fundamentals/resiliency-patterns.md](../../distributed-systems/fundamentals/resiliency-patterns.md)
