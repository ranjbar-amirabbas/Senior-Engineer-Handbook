# Data Structures Interview Questions

## Intro

These questions focus on practical data-structure judgment rather than academic puzzles alone.

## Use This File With

- [../fundamentals/data-structures.md](../fundamentals/data-structures.md)
- [../fundamentals/big-o.md](../fundamentals/big-o.md)

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

### Question

What does Big O tell you, and what does it not tell you?

### Short Answer

Big O tells you how work grows as input size increases. It does not tell you exact runtime, constant factors, cache behavior, I/O cost, or whether the code is on a path important enough to matter.

### Deep Explanation

Big O is great for spotting growth problems such as quadratic behavior. It is weaker for distinguishing between two small constant-time operations with very different real-world costs. That is why strong engineers use Big O for reasoning and profiling for confirmation.

### Senior-Level Notes

Senior candidates usually say both parts: complexity matters, but workload and measurement still decide whether the optimization is worth it.

### Common Mistakes

- treating Big O as a substitute for profiling
- assuming `O(1)` always means fast enough

### Question

What is amortized complexity, and why does it matter?

### Short Answer

Amortized complexity describes the average cost per operation over a long sequence of operations, even if some individual operations are expensive. It matters because many common structures, like dynamic arrays, behave this way in practice.

### Deep Explanation

Appending to a dynamic array is usually amortized `O(1)`. Most appends are cheap, but occasional resizes are expensive. Looking only at the worst single append would give the wrong intuition for normal usage.

### Senior-Level Notes

The better interview answer connects amortized complexity to real APIs such as `List<T>.Add` rather than defining it abstractly.

### Common Mistakes

- confusing amortized complexity with average case over random input
- discussing append performance without mentioning resize behavior

### Question

When is `O(n)` completely acceptable in backend code?

### Short Answer

It is acceptable when `n` is small, the path is cold, or the work happens infrequently enough that it does not affect latency, throughput, or cost meaningfully.

### Deep Explanation

Linear scans are often the cleanest solution for small collections or low-frequency maintenance logic. Complexity becomes a real problem when the path is hot, repeated heavily, or operating on large data sets. Context matters more than the symbol alone.

### Senior-Level Notes

Senior candidates usually mention workload shape: request path versus admin task, one hundred items versus one million items, once per deploy versus once per request.

### Common Mistakes

- trying to eliminate every `O(n)` operation on principle
- ignoring whether the code path is actually hot

## Related Topics

- [../fundamentals/data-structures.md](../fundamentals/data-structures.md)
- [../fundamentals/big-o.md](../fundamentals/big-o.md)
