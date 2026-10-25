# Spec: <title>

**Status:** draft | approved
**Source:** <grill restatement, issue link, or conversation>

## Outcome
<What changes for whom, and why now. Two to four sentences.>

## Definition of done
Each line is a predicate with the check that decides it.
- [ ] <predicate> — check: `<command>` / <observable user action> / <metric query>
- [ ] ...

## Behavior
<Behavior and contracts: inputs, outputs, states, error cases. Use the domain's words. No file paths.>

## Test seams
| Seam | Catches | Misses |
|---|---|---|
| <e.g. HTTP API of the orders service> | <...> | <...> |

## Boundaries
- **Always:** <e.g. run typecheck before commit; keep public API stable>
- **Ask first:** <e.g. new dependency; schema migration; deleting data>
- **Never:** <e.g. disable a failing test; commit secrets>

## Commands
- test: `<detected>`
- lint/typecheck: `<detected>`
- run: `<detected>`

## Risks and riskiest unknown
<The one thing that would invalidate the plan, and how to test it first.>

## Out of scope
- <non-goal>

## Open
- <undecided item, with the default that applies if nobody decides>
