---
name: blast-radius
description: Find what a change could break beyond its diff, and prove the one fact it is safe because of by running real code. Use for "blast radius of X", "what could this break", "is this safe to merge", or a small diff you don't trust.
disable-model-invocation: true
---

# Blast radius

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

Listing callers is not the job; grep does that in a second. The job is the breakage grep won't show.

## Steps

1. **Read the change.** The diff, every symbol it adds, changes, or deletes, and what now behaves differently, including what the diff doesn't spell out. Pull the PR and commits for context (`git log`, `gh pr view`).
2. **Find the one fact it's safe because of.** Most risky-looking changes are safe because of a single fact, like "this only evicts entries that are already expired". Find it. If it holds, most risks clear at once. Spend your time here.
3. **Look where grep stops.**
   - The library you call: read its source at the pinned version, and any local patch.
   - Timing: async ordering, teardown/unmount, retries, caching, transactions.
   - Data that crosses a boundary: API responses, DB columns, queue messages, files, wire formats read by another service or language.
   - Feature flags, config, environment differences.
   - Code three hops downstream of the obvious caller.
4. **Rate each risk honestly.** Real likelihood, real cost. Cite `file:line`. A search that finds nothing is still an answer; report it. Never invent a caller or an API.
5. **Prove the one fact.** Write a small script or test that calls the real code and fails loudly if the fact is false. Run it and paste the output.

## Proof ladder

For each fact the safety depends on, push it as far down as is cheap and say where it stopped:

1. You said so. Worthless alone.
2. You pointed at the line (`file:line`, or the library's source).
3. You walked the failure path step by step and showed it can't be reached.
4. You ran it: a script or test against the real code.
5. You reproduced it in the running app.

## Report

```
## Blast radius: <change>

**What it does:** <including the non-obvious part>

**Safe because:** <the one fact> — proof level <1-5>: <evidence or "UNPROVEN">

**Risks**
- <how it breaks> — <file:line> — likelihood <low|med|high>, cost <low|med|high> — check: <how>

**Cleared**
- <what you checked> — <why it's fine>

**Before you merge**
- <the cheapest test or repro that would catch the real bug; include the script you wrote>
```

Strip anything private before this goes anywhere public.
