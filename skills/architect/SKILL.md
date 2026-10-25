---
name: architect
description: Sketch types, signatures, and module boundaries before writing code, then implement against the sketch. Use when the user says "architect this" or "design this", or for non-trivial work that crosses function or module boundaries where jumping straight to code would lock in the wrong shape.
disable-model-invocation: true
---

# Architect

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

Design the shape first: types, signatures, module boundaries, with `not implemented` bodies and pseudocode for tricky logic. Then fill it in. The sketch is the contract.

Open a todo list with one entry per phase: Ground, Sketch, Agree, Implement, (Scrap).

## Phase A: Ground

Build a real model of every system the new code touches. Naming a file isn't grounding.

- Dispatch `Explore` sub-agents over the touched subsystems: data flow, owners of each piece of state, existing types to interoperate with, callers that can't break, conventions the nearest examples follow.
- If the design changes ownership or layering, also find *why* the current shape exists (`git log`, blame, the PR, ADRs). Then the old rationale is a constraint, not a guess.
- Read the spec if there is one, and the glossary. Use its terms in every name.

Skip only for true greenfield work with nothing to integrate.

## Phase B: Sketch

**Design it twice.** Get at least two structurally different candidates, even when the first looks fine. Whole-shape alternatives, not tweaks inside one shape.

Spawn 2-3 sketchers in parallel, each a fresh sub-agent with `references/sketcher-prompt.md`, the grounding notes, and one different constraint:
- **minimal**: the smallest public surface that could work
- **caller-first**: the ideal call sites for the most common caller
- **change-ready**: survives the most likely next requirement
- **pure core**: business logic as pure functions, a thin I/O shell

Sketchers write to separate scratch directories (`.scratch/architect/<slug>/<constraint>/`), never to the source tree.

Then, yourself:

1. **Screen** every candidate against `references/design-red-flags.md`. Assume the next contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles. Prefer the shape where a change that looks right from one file is right for the whole repo.
2. **Compare on interface depth.** Prefer the design that hides more behind a smaller, simpler public surface.
3. **Synthesize.** Pick a base, graft the best ideas from the others, and record what you took and rejected.
4. **Write** the rationale from `references/rationale-template.md` to `docs/design/<slug>.md`, and the sketch as real files in the source tree: types, signatures, doc comments with invariants, bodies that throw `not implemented`.

## Phase C: Agree

STOP. Show the usage sketch, the module map, and the rationale's trade-offs and open questions. The user approves the shape before any body is filled in.

Skip this checkpoint only if the user said up front to proceed without it. Pushback on the shape is new Phase A evidence: re-ground and re-sketch rather than patching.

Once approved, the sketch may be committed on its own ("scaffold first"), if the user works with commits.

## Phase D: Implement against the sketch

Fill bodies with the `tdd` skill, at seams the sketch exposes. If the work is large, suggest `/agent-skills:slice` with the sketch as input.

Deviations are signal, not friction. When a function needs a parameter or a type needs a field the sketch didn't plan, stop and decide which is wrong: the sketch, the requirement, or the implementation. Update the sketch and rationale if the sketch was wrong.

## Phase E: Scrap when the shape is wrong

The signal is a pattern, not one awkward case:

- The same workaround appears in unrelated places.
- Unrelated edge cases each need their own special branch.
- Types need escape hatches to compile (`any`, casts, optional fields that are always set).
- "We need a lock" on state the sketch said wasn't shared.
- Callers must know the abstraction's internal rules to use it.
- Two or more independent Phase D deviations with the same shape.

A few edge cases don't condemn a design; complexity in the data is not complexity in the design. When the pattern holds: tell the user, re-ground on what's been built, redesign as if the new constraints were day-one assumptions, and make the new sketch smaller than the old one before it grows. Then return to Phase B.

## Verification

- [ ] Usage (caller's view) was written before the types, and the two agree.
- [ ] At least two structurally different candidates existed; the rationale says why the base won.
- [ ] Every candidate was screened against the red flags; the chosen one's remaining flags are listed as accepted trade-offs.
- [ ] "Alternatives considered" is non-empty.
- [ ] The user approved the shape before bodies were filled (or explicitly waived it).
- [ ] Scratch sketches are deleted or ignored, not left in the source tree.
