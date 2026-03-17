# Data Structures Interview Questions

## Intro

These questions focus on practical data-structure judgment rather than academic puzzles alone.

## Practicality

### Question

How do you choose between a list and a hash set?

### Short Answer

Choose based on access pattern. Use a list for ordered sequential access and a hash set when fast membership checks matter more than ordering.

### Deep Explanation

The real decision depends on lookup frequency, uniqueness requirements, and collection size. Backend code often benefits more from the right simple structure than from complex optimization.

### Senior-Level Notes

Strong answers mention that measurement still matters in hot paths.

### Common Mistakes

- quoting Big O without discussing the workload

### Question

When does algorithmic complexity matter in backend work?

### Short Answer

It matters when a path is hot enough or data is large enough that inefficient operations affect latency, cost, or capacity.

### Deep Explanation

Many code paths are fine with straightforward implementations. Hot loops, batch jobs, and repeated request-path operations are where complexity mistakes show up.

### Senior-Level Notes

Senior answers connect algorithmic thinking to production impact.

### Common Mistakes

- optimizing everything equally
