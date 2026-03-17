# Testing Interview Questions

## Intro

Testing interviews assess how you build confidence in software changes. Strong answers show judgment about what to test, how deeply to test it, and how test strategy fits delivery speed and system risk.

## Use This File With

- [../fundamentals/testing-fundamentals.md](../fundamentals/testing-fundamentals.md)
- [../examples/test-strategy-examples.md](../examples/test-strategy-examples.md)
- [../../backend/devops/fundamentals/ci-cd.md](../../backend/devops/fundamentals/ci-cd.md)

## Test Strategy

### Question

What is the difference between unit, integration, and end-to-end tests?

### Short Answer

Unit tests verify isolated behavior quickly. Integration tests verify collaboration with real infrastructure or realistic components. End-to-end tests validate full workflows through the actual system boundaries.

### Deep Explanation

These categories differ in speed, realism, and maintenance cost. A healthy test strategy uses each where it provides the most confidence for the least cost, rather than trying to make one type do everything.

### Senior-Level Notes

The stronger answer usually ties each layer to a specific risk: business rule mistakes, integration breakage, or full workflow regression. That sounds more practical than simply naming the three categories.

### Common Mistakes

- treating the categories as interchangeable
- expecting unit tests to validate infrastructure behavior

### Question

What should be unit tested versus integration tested?

### Short Answer

Pure logic with minimal infrastructure dependency is a good unit-test target. Behavior that depends on databases, transactions, serialization, networking, or framework integration usually needs integration tests.

### Deep Explanation

The key question is what you are actually trying to prove. If the behavior depends on EF Core query translation, database constraints, or HTTP middleware, mocking it often removes the very thing that matters. Unit tests are best when they isolate logic cleanly.

### Senior-Level Notes

Strong answers mention that the boundary depends on architecture. A well-designed system usually makes some logic easy to test purely and some logic intentionally close to infrastructure.

### Common Mistakes

- mocking away all meaningful behavior
- forcing every test into one category

### Question

Why can heavy mocking make tests less valuable?

### Short Answer

Because tests may start validating the mocked setup rather than the real behavior of the system. This creates false confidence and brittle tests tied to implementation details.

### Deep Explanation

Mocking is useful for isolating slow or external dependencies, but overuse creates tests that break during harmless refactors and miss real integration failures. The right balance depends on whether the mocked behavior is incidental or central to correctness.

### Senior-Level Notes

Senior candidates often make this point in very practical terms: if a test has six mocks and still misses the real production failure mode, the issue may be the design boundary, not only the test style.

### Common Mistakes

- asserting call counts on trivial implementation details
- mocking frameworks or ORMs instead of testing realistic behavior

### Question

What is a good test pyramid or test portfolio?

### Short Answer

A good test portfolio has many fast focused tests, a smaller number of meaningful integration tests, and a limited set of end-to-end tests for critical flows.

### Deep Explanation

The exact shape varies by system, but the principle is balancing speed with realism. Too many slow broad tests reduce feedback speed. Too few realistic tests reduce trust.

### Senior-Level Notes

Strong answers avoid dogma and tailor the strategy to the system's risk profile.

### Common Mistakes

- quoting the pyramid without connecting it to the actual application

### Question

What makes a test suite trustworthy?

### Short Answer

A trustworthy test suite is stable, understandable, fast enough to run often, and aligned with the behaviors that matter most to the business and the system.

### Deep Explanation

Tests lose value when they are flaky, overly coupled to implementation, hard to diagnose, or too slow to influence daily development. Trust comes from signal quality, not volume alone.

### Senior-Level Notes

The practical senior answer usually includes an operational standard: flaky tests should be treated as a product defect in the delivery pipeline, not as harmless background noise the team learns to ignore.

### Common Mistakes

- tolerating flaky tests as normal background noise

### Question

How do you test asynchronous or eventually consistent workflows?

### Short Answer

Test the logic at the right boundary, use timeouts and polling carefully, and verify observable outcomes rather than relying only on synchronous assumptions.

### Deep Explanation

Asynchronous workflows often need integration-style tests that account for queues, retries, or delayed processing. The challenge is making them reliable enough to trust while still validating the distributed behavior that matters.

### Senior-Level Notes

Strong answers mention idempotency, fake clocks where appropriate, and avoiding brittle sleeps.

### Common Mistakes

- using arbitrary `Thread.Sleep` as the main synchronization strategy

### Question

How do tests fit into CI/CD?

### Short Answer

Tests are part of the delivery safety system. Fast tests should run early and often, while slower but high-value tests should guard promotion at the right stages.

### Deep Explanation

Not every test needs to run on every code path, but the pipeline should still give enough confidence before production. The key is aligning test depth with risk and release cost.

### Senior-Level Notes

The stronger answer usually describes a pipeline shape: fast deterministic checks early, slower integration coverage later, and only a small number of broad end-to-end flows guarding release.

### Common Mistakes

- putting every heavy test in the first pipeline stage

### Question

What is more important than code coverage percentage?

### Short Answer

Coverage quality and behavioral confidence are more important than raw percentage. High coverage can still miss the system's most important risks.

### Deep Explanation

Coverage can reveal blind spots, but it cannot prove meaningful assertions or realistic behavior. It should be a signal, not the target.

### Senior-Level Notes

Strong answers mention critical paths, defect history, and change risk.

### Common Mistakes

- chasing coverage numbers without improving test value

## Related Topics

- [../fundamentals/testing-fundamentals.md](../fundamentals/testing-fundamentals.md)
- [../examples/test-strategy-examples.md](../examples/test-strategy-examples.md)
- [../../backend/devops/interview/ci-cd.md](../../backend/devops/interview/ci-cd.md)
- [../../backend/dotnet/interview/entity-framework.md](../../backend/dotnet/interview/entity-framework.md)
