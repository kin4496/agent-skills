# Fix root causes

A symptom fix makes the error disappear here and reappear somewhere you aren't looking.

## The guard test
A guard (null check, try/catch, default value, retry) is a fix **only when the state it handles is legitimate**. Can this value really be null in a correct system? If yes, handle it. If no, the bug is wherever the null was created; go there.

## Pattern
1. Trace the bad value back to where it was created, not where it crashed.
2. Fix it there. If that's out of scope, fix it at the nearest boundary you own and record the real cause.
3. Ask what would have caught it earlier: a type, a validation at the boundary, an assertion, a metric. Propose the cheap always-on signal.

## Red flags
- `catch (e) {}` or catch-and-log with no recovery path.
- Retrying a deterministic failure.
- `?.` chains added until the error stops.
- "Works now" without being able to say why it failed.
