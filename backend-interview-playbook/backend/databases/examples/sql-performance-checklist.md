# SQL Performance Checklist

## Problem Context

Teams need a repeatable way to review slow SQL paths without jumping straight to guesses.

## Recommended Approach

Use a lightweight checklist that starts with measurement and narrows the failure mode before changing schema or code.

## Example Walkthrough

Review these points:

1. capture the exact SQL and parameter values
2. inspect the actual execution plan
3. check row counts and result size
4. verify relevant indexes and statistics
5. look for scans, sorts, spills, and blocking
6. confirm whether the query shape can be simplified
7. test against realistic data volume

## Trade-Offs

A checklist improves consistency, but it does not replace expertise. The goal is to prevent obvious misses and make tuning conversations more disciplined.

## Common Mistakes

- skipping directly to infrastructure scaling
- optimizing without production evidence
- treating the ORM as the only layer worth inspecting

## Related Topics

- [../fundamentals/query-optimization.md](../fundamentals/query-optimization.md)
- [../interview/sql.md](../interview/sql.md)
