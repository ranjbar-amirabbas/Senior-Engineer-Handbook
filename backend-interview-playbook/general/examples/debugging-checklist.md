# Debugging Checklist

## Problem Context

Incidents and regressions become harder to resolve when teams respond with ad hoc guesses instead of a repeatable troubleshooting flow.

## Recommended Approach

Use a short checklist that forces evidence gathering before risky changes.

## Example Walkthrough

1. define the symptom clearly
2. identify when it started and what changed
3. check metrics, logs, and traces together
4. narrow the failing boundary: app, database, dependency, network, or deploy
5. reproduce if practical
6. test one hypothesis at a time
7. choose mitigation and then verify the effect

## Trade-Offs

A checklist does not replace expertise, but it improves consistency and reduces panic-driven decisions during incidents.

## Common Mistakes

- skipping evidence collection
- changing multiple things at once
- confusing symptom relief with root-cause resolution

## Related Topics

- [../fundamentals/debugging-and-troubleshooting.md](../fundamentals/debugging-and-troubleshooting.md)
- [../../backend/devops/fundamentals/monitoring-and-observability.md](../../backend/devops/fundamentals/monitoring-and-observability.md)
