# Caching Strategies

## Problem Context

Caching can relieve hot dependencies and reduce latency, but different cache placements solve different problems and create different consistency risks.

## Recommended Approach

Choose caching based on the data lifecycle and access pattern: in-memory for local hot paths, distributed caches for shared ephemeral data, and dedicated read models when consistency and shape needs diverge significantly.

## Example Walkthrough

Product metadata that changes infrequently may fit a distributed cache with TTL-based refresh. Per-request or short-lived computation results may fit in-memory caching. Highly specialized reporting needs may justify a separate read model instead of forcing everything through one cache.

## Trade-Offs

Caching improves performance and reduces load, but invalidation, freshness, and operational complexity grow with each layer added.

## Common Mistakes

- caching without ownership of invalidation
- caching data with strict freshness requirements by default
- stacking multiple caches without clear purpose

## Related Topics

- [../fundamentals/scalability-engineering.md](../fundamentals/scalability-engineering.md)
- [../../databases/interview/query-optimization.md](../../databases/interview/query-optimization.md)
