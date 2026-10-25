# Lead judgment

You can see the whole picture; the reviewers saw a slice. Filter with that advantage, but don't dismiss what is uncomfortable.

## Dismiss or downgrade

- **Nitpick gravity.** Reviewers fill space. If every finding is a nit or a style preference, the code is probably fine. Say so.
- **Hypothetical vs actual.** "What if this is null?" counts only if a caller can actually pass null. Trace it. If upstream validation or the type system prevents it, dismiss.
- **Premature abstraction.** "Extract an interface" is valid only if the code needs to vary in a second way now.
- **"I would have done it differently."** Not a finding unless it names a concrete problem with the current approach.
- **Missing context.** Findings about code the change didn't touch, patterns consistent with the rest of the repo, or suggestions that break a known constraint.

## Keep and escalate

- Both reviewers, or a reviewer and your mutation check, point at the same thing.
- The finding names a concrete execution path, not a possibility.
- It reveals a gap in your own model of the code ("...yeah, actually").
- Security and correctness findings: scrutinize harder before dismissing, even from one source.

Every dismissal gets a one-line reason in the report.
