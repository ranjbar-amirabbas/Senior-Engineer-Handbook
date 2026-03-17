# Test Strategy Examples

## Problem Context

Teams often know testing vocabulary but struggle to choose the right level of test for a given backend change.

## Recommended Approach

Match the test type to the behavior you need to trust, then keep the suite balanced enough for fast feedback and realistic confidence.

## Example Walkthrough

Example 1: pricing logic with pure inputs and outputs is a strong unit-test target.

Example 2: EF Core persistence with unique constraints and transaction behavior needs integration tests.

Example 3: checkout flow with payment, inventory, and notifications may justify a small number of end-to-end tests plus broader integration coverage around key boundaries.

## Trade-Offs

More realism increases confidence but also slows execution and increases setup complexity. The right strategy is selective, not maximal.

## Common Mistakes

- defaulting to mocks for infrastructure-heavy behavior
- relying only on end-to-end tests for critical systems
- treating every bug as a reason to add the broadest possible test

## Related Topics

- [../fundamentals/testing-fundamentals.md](../fundamentals/testing-fundamentals.md)
- [../interview/testing.md](../interview/testing.md)
