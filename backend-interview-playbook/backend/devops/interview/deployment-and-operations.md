# Deployment And Operations Interview Questions

## Intro

These questions focus on the practical side of running backend systems safely.

## Operations

### Question

Why are health checks useful?

### Short Answer

They help platforms and operators determine whether a service should receive traffic and whether it can recover automatically from failure.

### Deep Explanation

Health checks support routing, restart, and automation decisions, but only when they reflect meaningful service health.

### Senior-Level Notes

Strong answers mention shallow versus dependency-aware checks.

### Common Mistakes

- implementing checks that are always green or always too strict

### Question

What is graceful shutdown?

### Short Answer

Graceful shutdown lets an application stop taking new work and finish or cancel in-flight work safely before termination.

### Deep Explanation

This matters during deployments and scaling events so requests, jobs, or message processing do not fail mid-flight unnecessarily.

### Senior-Level Notes

Senior candidates mention signal handling, cancellation, and load balancer coordination.

### Common Mistakes

- ignoring termination behavior until production

### Question

Why are logs alone not enough for operations?

### Short Answer

Because logs are detailed but incomplete without metrics and traces to show trends, saturation, and cross-service flow.

### Deep Explanation

Operations requires both point-in-time detail and aggregate/system-wide visibility. Logs are one input, not the whole observability story.

### Senior-Level Notes

Strong answers mention correlation IDs and percentile-based metrics.

### Common Mistakes

- relying on manual log searching as the main debugging strategy
