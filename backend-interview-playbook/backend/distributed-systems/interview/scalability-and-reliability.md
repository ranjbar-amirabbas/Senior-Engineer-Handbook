# Scalability And Reliability Interview Questions

## Intro

These questions explore how systems keep working as load and failure increase.

## Reliability

### Question

What is the difference between scalability and reliability?

### Short Answer

Scalability is about handling growth in workload. Reliability is about continuing to behave correctly and predictably despite failures and stress.

### Deep Explanation

A system can scale but still fail often, and a reliable system can still hit capacity limits. Mature designs address both.

### Senior-Level Notes

Strong answers mention that many incidents appear as reliability failures triggered by scale.

### Common Mistakes

- using the terms interchangeably

### Question

Why are timeouts important?

### Short Answer

Timeouts prevent resources from waiting forever on slow or broken dependencies. They define failure boundaries and help protect system capacity.

### Deep Explanation

Without timeouts, request threads, connections, or workers can accumulate behind a failing dependency and create cascading outages.

### Senior-Level Notes

Strong candidates mention that timeout values should be intentional and workload-specific.

### Common Mistakes

- leaving client defaults unchanged without understanding them

### Question

What is backpressure?

### Short Answer

Backpressure is a way of signaling or enforcing that producers slow down when consumers or downstream systems cannot keep up.

### Deep Explanation

Without it, queues grow, latency spikes, and overload propagates. Rate limits, bounded queues, and admission control are common forms.

### Senior-Level Notes

Senior answers connect backpressure to graceful degradation rather than only system protection.

### Common Mistakes

- assuming autoscaling alone solves overload

### Question

How do you make retries safer?

### Short Answer

Use retries only for transient failures, add backoff and jitter, cap attempts, and ensure the operation is idempotent or otherwise protected from duplication.

### Deep Explanation

Safe retries reduce recovery time without creating retry storms or duplicated side effects. Unsafe retries often worsen incidents.

### Senior-Level Notes

Strong answers mention telemetry and retry budgets.

### Common Mistakes

- adding unbounded retries in shared libraries
