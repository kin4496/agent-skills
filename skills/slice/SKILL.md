---
name: slice
description: Break an approved spec into vertical-slice tickets that each fit one fresh context window, with explicit blocking edges and the riskiest slice first. Use when the user says "break this down", "make tickets", "plan the tasks", or a spec is too big for one session.
disable-model-invocation: true
---

# Slice

Turn a spec into an ordered set of tickets an agent can pick up cold.

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

## Before you start

- Read the spec. No approved spec, or open decisions that block slicing? Stop and say which, then suggest `/agent-skills:spec` or `/agent-skills:grill`.
- If `tasks/` (or the tracker) already holds a plan with unfinished items, **never overwrite it**. Stop and ask: extend, replace, or abandon.

## Slicing rules

1. **Vertical, not horizontal.** Each slice cuts through every layer it needs (data, logic, interface, test) and delivers one observable behavior. "Add DB tables" then "add API" then "add UI" is horizontal; reject it.
2. **One fresh context window per slice.** If a slice's title needs "and", or it has more than three acceptance criteria, split it.
3. **Riskiest first.** The slice that could invalidate the plan goes first, even if it's not the natural start. A thin tracer bullet through the whole path beats a polished first layer.
4. **Prefactor separately.** "Make the change easy, then make the easy change." A refactor that unlocks the feature is its own slice, before it.
5. **Wide refactors go expand -> migrate -> contract.** Add the new path, move callers in batches, then delete the old path. Each step is a slice that leaves main green.
6. **Explicit edges.** Every slice names what blocks it. Slices with no blockers form the ready frontier and can run in parallel.
7. **Checkpoints.** After every two or three slices, add a checkpoint: run the full suite, re-read the spec, confirm the plan still holds.

## Ticket format

Write to `tasks/<spec-slug>/NN-<slug>.md` (or the repo's tracker):

```
# NN: <one behavior, no "and">
Status: todo | in-progress | done
Blocked by: <NN, NN | none>
Spec: <path to spec>

## Behavior
<What a user or caller can observe after this slice. No file paths.>

## Acceptance
- [ ] <predicate> — check: <command or observation>   (max 3)

## Notes
<Context pointers: which spec section, which ADR, which prior slice. Point, don't paste.>
```

Then write `tasks/<spec-slug>/README.md` listing slices in order with their blockers, and mark the ready frontier.

STOP. Show the ordered list and the frontier. Wait for approval before implementing.

## Red flags

- A first slice that delivers nothing a user or test can observe.
- A slice whose acceptance is "code compiles" or "tests pass" with no named behavior.
- Twelve slices for a two-day feature. Merge until each slice is worth a fresh context.

## Working the slices

After approval, take one ready slice at a time (or the user's pick): set it `in-progress`, implement with `tdd`, then set it `done`, update the README frontier, and run the checkpoint if one is due. One slice per fresh context when slices are large. Suggest `/agent-skills:review` at checkpoints and at the end.
