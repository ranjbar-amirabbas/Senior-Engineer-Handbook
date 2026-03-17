# Testing Fundamentals

## Overview

Testing helps teams change software with confidence. For backend systems, the goal is not maximum test count. It is the right balance of fast feedback, realistic validation, and trustworthy coverage.

## Core Concepts

- Unit tests validate small pieces of behavior quickly and locally.
- Integration tests validate collaboration with real infrastructure or realistic substitutes.
- End-to-end tests validate full user or system flows but are slower and costlier to maintain.
- Test strategy should follow system risk and architecture, not ideology.

## Why It Matters In Real Systems

A backend system with weak tests becomes expensive to change. A system with the wrong kinds of tests becomes slow and brittle without improving confidence. Mature teams choose tests based on failure modes they actually care about.

## Practical Example

A pricing rule can be verified well with unit tests because the logic is isolated and deterministic. A persistence workflow that depends on transactions and database constraints needs integration tests because mocking the database misses the real behavior that matters.

That distinction is usually where teams either gain confidence or lose it. If you mock away EF Core query behavior, transaction handling, or serialization boundaries, the tests may stay green while the production path remains unverified.

## Common Pitfalls

- over-mocking infrastructure-sensitive code
- relying only on end-to-end tests for confidence
- treating code coverage as a quality proxy
- accepting flaky tests as the cost of having a large suite
- writing tests that are hard to understand or trust

## Interview Takeaways

- Explain tests in terms of confidence, speed, and realism.
- Mention boundaries where mocks stop being enough.
- Show that testing is part of design, not an afterthought.

## Related Topics

- [debugging-and-troubleshooting.md](debugging-and-troubleshooting.md)
- [../interview/testing.md](../interview/testing.md)
- [../examples/test-strategy-examples.md](../examples/test-strategy-examples.md)
