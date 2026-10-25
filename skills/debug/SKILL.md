---
name: debug
description: Diagnosis discipline for bugs, flaky tests, and performance regressions. Use when the user says "debug", "diagnose", "why is this failing", or reports something broken, throwing, flaky, or slow, or after a fix attempt has failed.
---

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

# Debug

**The feedback loop is the skill.** With a tight signal that goes red on this bug, bisection and hypotheses become mechanical. Without one, no amount of reading code helps.

Redact secrets in everything you show; write `<REDACTED>`.

## Phase 1: Build a red-capable loop

Try, roughly in order: failing test at the reaching seam; curl/HTTP script; CLI with fixture diffed against a known-good snapshot; headless browser script; replay of a captured payload or trace; throwaway harness of the minimal subsystem; property/fuzz loop; `git bisect run` harness; differential run (old vs new, config A vs B); a scripted human-in-the-loop as last resort.

Then tighten: faster (narrow scope, cache setup), sharper (assert the exact symptom, not "didn't crash"), deterministic (pin time, seed RNG, isolate filesystem and network). For flaky bugs the goal is a higher reproduction rate, not a clean repro: loop, parallelize, add stress until it fails often enough to debug.

**Gate.** Name one command you have already run, with its output, that is:
- [ ] **Red-capable**: drives the real code path and asserts the user's exact symptom
- [ ] **Deterministic** (or a pinned, high reproduction rate)
- [ ] **Fast**: seconds
- [ ] **Agent-runnable**

No such command, no Phase 2. If you catch yourself building a theory from code before it exists, stop. If you truly can't build a loop, say so, list what you tried, and ask for access, a captured artifact, or permission to instrument.

## Phase 2: Reproduce and minimize

Confirm the loop shows the user's failure, not a nearby one. Then cut inputs, callers, config, and steps one at a time, re-running after each, until every remaining element is load-bearing.

## Phase 3: Hypothesize

Write 3 to 5 ranked hypotheses before testing any. Each must be falsifiable:

> If <X> is the cause, then <changing Y> makes the bug disappear / <changing Z> makes it worse.

A hypothesis without a prediction is a vibe; sharpen or drop it. Show the list to the user (they often re-rank instantly), but don't block on a reply.

## Phase 4: Instrument

Each probe tests one prediction. Change one variable at a time. Prefer a debugger or REPL; otherwise targeted logs at boundaries that separate hypotheses. Tag every debug log with a unique prefix like `[DEBUG-a4f2]` so cleanup is one grep. For performance: measure a baseline first, then bisect; logs mislead here.

## Phase 5: Fix at the root

- Write the regression test before the fix, at a seam that exercises the real bug pattern. If no correct seam exists, that is a finding; report it.
- A guard (null check, try/catch, retry) is a fix only if the state it handles is legitimate. Otherwise find where the bad state is created. Read `../principles/references/fix-root-causes.md`.
- Watch the test fail, apply the fix, watch it pass, re-run the original unminimized loop.

## Two failed fixes: attack the premise

If two fixes have failed, stop fixing. Write down the assumption both fixes shared ("the cache is invalidated on write", "this runs once"). Test that premise directly. Do not start a third fix until the premise is written and tested.

## Phase 6: Clean up

- [ ] Original loop no longer reproduces
- [ ] Regression test passes, or the missing seam is documented
- [ ] `grep` for the debug prefix returns nothing
- [ ] Throwaway harnesses deleted or clearly parked
- [ ] Commit/PR message names the hypothesis that turned out correct
- [ ] Suggest the cheap always-on signal (log, metric, assertion, type) that would have caught this earlier
