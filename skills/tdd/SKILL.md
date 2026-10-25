---
name: tdd
description: Test-driven implementation discipline for behavior changes. Use when implementing a feature or slice, fixing a bug with a known cause, or when the user mentions TDD, tests first, or red-green. Not for config, docs, or other non-behavioral edits.
---

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

# TDD

Small vertical steps. Each step: one failing test, the least code to pass it, run everything.

## 0. Discover, don't assume

Find the real commands from the repo (package scripts, Makefile, CI config, README). Run the suite once before changing anything and record the baseline: what passes, what already fails, how long it takes. Pre-existing failures are noted, not fixed.

## 1. Agree the seams

Test only at seams agreed in the spec, or confirmed with the user now. Prefer the highest seam that can observe the behavior. A test below the seam couples to implementation; a test above it is slow and vague.

If the right seam doesn't exist (the behavior can't be observed without reaching into internals), that is a design finding. Say so; consider a prefactor slice before continuing.

## 2. The loop

```
RED    write one test for the next behavior. Run it. Watch it fail
       for the expected reason (not an import error, not a typo).
GREEN  write the least code that passes. Run the whole relevant suite.
TIDY   optional, only while green: small refactors of code you just
       touched (rename, extract, dedupe). Re-run tests after each.
NEXT   pick the next behavior. Repeat.
```

- One behavior per test; one test per loop.
- Never write more than ~100 lines without running tests.
- A test that passes on its first run is suspicious: either the behavior already existed or the test doesn't test it. Break the code on purpose, confirm it goes red, then revert and `git diff` to confirm the revert landed.
- Structural refactors beyond TIDY (moving modules, changing interfaces) are not part of this loop. Note them; they become their own prefactor slice.

For a bug fix: write the reproduction test first (**prove it**), watch it fail with the user's exact symptom, then fix. If possible, have a sub-agent write the reproduction test without seeing your planned fix.

## 3. Guard the bar

Before every commit, scan your own diff for:

- `.skip`, `.only`, `xit`, `@ts-ignore`, `eslint-disable`, `# type: ignore`, `istanbul ignore`, `noqa`
- lowered thresholds (coverage, timeouts, perf budgets, retry counts)
- deleted or weakened assertions; tests changed to match new output without a stated reason
- stubs or mocks of the very thing under test

Tightening the bar is silent. Loosening it is loud: stop and tell the user what and why, and get approval.

## Test anti-patterns

- **Tautological**: the expected value is computed the same way as the code (`expect(add(a, b)).toBe(a + b)`). Use literal, independently known values.
- **Implementation-coupled**: asserts call counts, private state, or internal ordering instead of observable behavior.
- **Mocking what you own** at the seam you're testing. Mock only at system boundaries (network, clock, randomness, third-party services).
- **Horizontal**: writing all tests first, then all code. Lose the feedback; one at a time.

## Verification

- [ ] Every new behavior has a test that was seen failing before it passed.
- [ ] Full relevant suite green; pre-existing failures unchanged and listed.
- [ ] Guard-the-bar scan clean, or each loosening approved.
- [ ] Typecheck and lint pass with the repo's own commands.
- [ ] If working from `slice` tickets: ticket set to `done`, frontier updated, checkpoint run if due.

## Rationalizations

| Excuse | Reality |
|---|---|
| "I'll add tests after; it's faster." | You'll test what you built, not what was asked. |
| "This is too simple to test." | Then the test takes one minute. |
| "The test is flaky, I'll skip it." | Flaky is a bug. Use `debug`, or ask before skipping. |
| "I just updated the snapshot to match." | Say why the new output is correct, or it's a silent regression. |
