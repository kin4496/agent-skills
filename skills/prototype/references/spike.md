# Spike

The smallest runnable thing that answers a behavioral, timing, performance, or integration question: "is polling at 1s fast enough", "does this library handle streaming uploads", "can these two services agree on the format".

## Build

1. Write the question and what answer would change the decision, before writing code: "If p95 < 300ms, we poll; otherwise we use websockets."
2. One script in `prototypes/<name>/`, using the lightest stack that exercises the real thing. Call the real library or service, not a mock of it; the mock would answer a different question.
3. Print exactly what decides the question: timings, sizes, outputs, error messages. No pass/fail assertions; the observation is the result.
4. For a comparison, run each approach under the same conditions in the same script, behind a flag or argument.

## Measure honestly

- Warm up before timing; run at least 5 times and report the median and range, not the best run.
- Name what limits the result (CPU, network, a rate limit, the test machine). If you can't name it, the result is INCONCLUSIVE.
- Say how the spike differs from production (data size, concurrency, network) and whether that could flip the answer.

## Report

The question, the command, the raw numbers or output, the limiter, and the decision it supports. Spike code is never promoted.
