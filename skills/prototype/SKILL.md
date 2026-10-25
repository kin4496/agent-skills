---
name: prototype
description: Build throwaway code to settle one design question that talking can't. Use when the user says "prototype", "spike", "try a few options", "mock it up", wants to feel out a state model or UI before committing, or a grill/spec question stays stuck between alternatives.
disable-model-invocation: true
---

# Prototype

Operating rules: if not already read this session, read `../using-agent-skills/SKILL.md` (relative to this skill's directory) first.

A prototype is a throwaway instrument that answers one question. You own the decision, not the code.

## 1. Name the decision

Write one line: "This prototype decides <X> between <options>." Examples: which layout for the settings page; whether this order state machine survives cancel-after-ship; whether polling at 1s feels live enough.

No decision, no prototype. If the user wants a feature, not an answer, suggest `/agent-skills:spec` instead.

## 2. Pick the branch

| Question | Branch | Artifact |
|---|---|---|
| Does this logic / state model / data shape hold up? | `references/logic.md` | One self-contained HTML file a non-developer can click through |
| What should this look like? | `references/ui.md` | 3 (max 5) structurally different variants on one route, switched by `?variant=` |
| Does this behavior, timing, or approach work? | `references/spike.md` | The smallest script that exercises the question and prints what you need to see |

Read only the branch you picked. If the question is ambiguous and the user isn't around, pick by the surrounding code (backend module -> logic, page or component -> UI, performance or integration -> spike) and state the assumption at the top of the artifact.

## Rules for every branch

1. **Throwaway and labeled.** The word `prototype` appears in every path or filename. Logic and spike artifacts go in a scratch directory (`prototypes/<name>/`, git-ignored if the repo allows). UI variants live on the real route they're for, following the project's routing convention.
2. **One command to run.** Double-click an HTML file, or one task-runner command. Write the command at the top of the artifact.
3. **In-memory by default.** If persistence is the question, use a scratch DB or file named `PROTOTYPE-wipe-me`.
4. **No polish.** No tests, no error handling beyond what keeps it running, no abstractions, no generalizing for "later".
5. **Make the state visible.** After every action or variant switch, show the full relevant state or output.
6. **Variants disagree about structure.** Different layout, hierarchy, model, or approach, not different colors or names. If two come out similar, redo one with an explicit "do not use <the shared idea>" constraint.

## 3. Observe on the matching surface

The observation is the test. For UI, screenshot each variant (browser tool if available) and drive the main interaction. For logic, click through every walkthrough yourself first. For a spike, run it and capture the timing or output. Something you didn't observe is INCONCLUSIVE.

## 4. Report and STOP

```
## Prototype: <decision>
Run: <command or file path>
Variants: <A: one line> / <B: ...> / <C: ...>
Evidence: <screenshots, walkthrough results, timings>
Trade-offs: <per variant>
Recommendation: <pick, and why>
This is throwaway code. Do not ship it.
```

STOP. The user decides. Their best feedback is usually "header from B, sidebar from C"; that combination is the decision.

## 5. Capture and clean up

- Record the decision and the question it settled in the spec or ticket. If it passes the ADR test in `domain-modeling`, offer an ADR.
- A logic prototype's pure module may be lifted into real code, but only through `tdd`: write the tests, then move it.
- UI and spike code is never promoted. Rebuild the winner properly via `spec` / `tdd`.
- Ask before deleting. Default: commit the prototype to a `prototype/<name>` branch as a primary source, remove it from the working branch, and link the branch from the spec or ticket.

## Rationalizations

| Excuse | Reality |
|---|---|
| "It works, let's just ship the prototype." | It was written without tests or error handling on purpose. Rebuild it. |
| "One variant is enough, I know what they want." | Then you don't need a prototype. Ask the question instead. |
| "I'll add tests so it's not wasted." | A prototype that needs tests is no longer a prototype. |
