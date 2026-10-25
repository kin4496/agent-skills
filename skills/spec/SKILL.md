---
name: spec
description: Turn an agreed goal into a written, testable spec with a falsifiable definition of done, test seams, and Always/Ask/Never boundaries. Use after grilling, when the user says "write a spec" or "PRD", or before any change touching more than a few files.
disable-model-invocation: true
---

# Spec

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

Synthesize, don't interview. If key decisions are still open, list them and suggest the user run `/agent-skills:grill` first.

## Process

1. **Gather.** Read the grill restatement (or the conversation), relevant code, and any glossary or ADRs. Write the spec in the glossary's terms; if you need a term it doesn't have, apply `domain-modeling`. Detect the stack and commands (test, lint, typecheck, build) from the repo; don't guess `npm test`.
2. **Make vague targets measurable.** "Faster" becomes "p95 < 200ms on X". "Robust" becomes "returns 4xx, not 500, for inputs A, B, C". If you can't make it measurable, list it under Open.
3. **Agree test seams.** Propose where tests will observe behavior. Use the highest seam that can see the behavior; the ideal number is one. For each seam: what it catches, what it misses.
4. **Write** the spec from `references/spec-template.md` to `docs/specs/<slug>.md` (or the tracker the repo uses).
5. **Self-check** against the checklist below.

STOP after writing. Show the path, the Definition of Done, and the proposed seams. Approval comes next turn; approving the spec approves the seams. When the user approves, set `Status: approved` in the file.

## Writing rules

- **Durability over precision.** Describe behavior and contracts, not file paths or line numbers; those rot. Exception: a small decision-carrying snippet from a prototype.
- **Definition of done is a predicate.** Something a command or an observer can say true or false to. "Works well" is not a predicate.
- **Out of scope is mandatory.** It is the most useful section for an agent.
- **Riskiest unknown first.** If something might make the whole plan wrong, the spec says how to test it before anything else.

## Verification

- [ ] Every success criterion is a falsifiable predicate with a named check.
- [ ] Test seams are listed with catches/misses.
- [ ] Boundaries have all three tiers filled (Always / Ask first / Never).
- [ ] Out of scope is non-empty.
- [ ] No file paths except decision-carrying snippets.
- [ ] Open questions are listed, not silently assumed.

## Red flags

- Spec contains the implementation, step by step. That's a plan, and it belongs in `slice`.
- Success criteria that only the agent's own new tests can check. Prefer an external check (existing suite, real data, user action).
- Writing the spec and starting code in the same turn.

Next: suggest `/agent-skills:architect` if the shape crosses module boundaries and isn't obvious, `/agent-skills:slice` if the work won't fit one session; otherwise implement with `tdd`.
