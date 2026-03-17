# Scalability Interview Questions

## Intro

These questions focus on how systems grow under load and how engineers reason about bottlenecks.

## Capacity

### Question

What do you scale first in a struggling system?

### Short Answer

You scale or optimize the actual bottleneck first, not the most visible component.

### Deep Explanation

If the database is saturated, adding more API nodes may just increase pressure. If the system is I/O-bound on an external dependency, scaling application CPU will not help.

### Senior-Level Notes

Strong answers mention measurement before intervention.

### Common Mistakes

- scaling by intuition

### Question

Why does tail latency matter?

### Short Answer

Because users feel slow outliers, and distributed systems amplify them through fan-out and retries.

### Deep Explanation

Average latency can look healthy while a small percentage of slow requests create serious reliability problems, especially on critical workflows.

### Senior-Level Notes

Senior candidates mention SLOs, percentile tracking, and backpressure.

### Common Mistakes

- focusing only on averages

### Question

How does caching improve scalability?

### Short Answer

It reduces repeated work on expensive dependencies such as databases or external APIs, improving latency and capacity when the access pattern supports it.

### Deep Explanation

Caching helps only when hit rates are meaningful and consistency trade-offs are understood. Otherwise it adds complexity without enough payoff.

### Senior-Level Notes

Strong answers mention cache invalidation and ownership.

### Common Mistakes

- adding caching before understanding the workload

### Question

What is a hot partition or hotspot?

### Short Answer

It is a skewed concentration of traffic or data on one node, shard, or partition that limits system throughput.

### Deep Explanation

Hotspots often appear when partition keys are poorly chosen or workloads are uneven. They can make a horizontally scaled system behave like a single-node bottleneck.

### Senior-Level Notes

Senior candidates mention workload distribution, partition-key choice, and mitigation strategies.

### Common Mistakes

- assuming even distribution automatically happens
