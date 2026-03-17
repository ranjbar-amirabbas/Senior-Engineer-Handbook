# API Idempotency

## Problem Context

Clients retry requests when connections fail, responses time out, or intermediaries behave unpredictably. Without idempotency, duplicate requests can create duplicate business actions.

## Recommended Approach

For sensitive create-like operations, accept a client-supplied idempotency key, store the result of the logical operation durably, and return the same effective outcome for repeated requests with the same key.

## Example Walkthrough

A payment API receives `POST /payments` with an idempotency key. The service stores the key and the resulting payment identifier inside the transaction that creates the payment record. If the client retries, the API returns the same business result instead of charging again.

## Trade-Offs

Idempotency improves safety but adds storage, key-lifecycle management, and edge-case handling when repeated requests are not actually equivalent.

## Common Mistakes

- assuming HTTP verb semantics alone solve retry safety
- storing idempotency keys without tying them to the real business outcome
- ignoring expiry and key reuse rules

## Related Topics

- [../fundamentals/rest-api-design.md](../fundamentals/rest-api-design.md)
- [../../distributed-systems/interview/distributed-systems.md](../../distributed-systems/interview/distributed-systems.md)
