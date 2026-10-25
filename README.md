# agent-skills

**English** | [한국어](README.ko.md)

Personal engineering skills for Claude Code, covering planning, implementation, and review. Distilled from three collections:

- [cursor/plugins `pstack`](https://github.com/cursor/plugins/tree/main/pstack): evidence rigor (three-way verdicts, proof ladder, review lead-judgment).
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills): skill anatomy (red flags, rationalizations, verification checklists), doubt-driven review, guard-the-bar.
- [mattpocock/skills](https://github.com/mattpocock/skills): short, sharp workflows (grilling rounds, red-capable debug loop, seams, vertical slices, two-axis review).

## Skills

| Stage | Skill | Invoked by | What it does |
|---|---|---|---|
| Router | `using-agent-skills` | model | Picks the skill; shared operating rules |
| Plan | `grill` | user | Interview in rounds of recommended-answer questions until the goal is confirmed |
| Plan | `grill-with-docs` | user | `grill` + `domain-modeling`: the interview also writes GLOSSARY.md and ADRs |
| Plan | `domain-modeling` | model | Challenge terms, probe with scenarios, keep GLOSSARY.md and ADRs current |
| Plan | `prototype` | user | Throwaway artifact to settle one design question: logic demo, UI variants, or spike |
| Plan | `spec` | user | Testable spec: falsifiable done, seams, Always/Ask/Never, out of scope |
| Plan | `architect` | user | Sketch types and module boundaries first (2+ candidates, red-flag screen), then implement against the sketch |
| Plan | `slice` | user | Vertical-slice tickets, one context window each, riskiest first |
| Build | `tdd` | model | Red-green at agreed seams; guard the bar |
| Build | `debug` | model | Red-capable loop gate, falsifiable hypotheses, attack the premise |
| Build | `principles` | model | Model the domain, fix root causes, subtract, design twice, build the lever |
| Review | `review` | user | Intent lock, isolated standards + spec reviewers, mutation check, lead judgment |
| Review | `doubt` | model | Fresh-context adversarial check of a claim; VERIFIED / NOT VERIFIED / INCONCLUSIVE |
| Review | `blast-radius` | user | The one fact a change is safe because of, proven by running code |

Typical flow: `grill -> (prototype) -> spec -> (architect) -> slice -> tdd (per slice) -> review`.

## Provenance

Where each skill's ideas come from. pstack = cursor/plugins `pstack`, addy = addyosmani/agent-skills, matt = mattpocock/skills.

| Skill | Main base | Also from |
|---|---|---|
| `using-agent-skills` | addy `using-agent-skills` (router, core behaviors) | pstack `poteto-mode` (claim labels, pause on irreversible writes); matt `ask-matt/PHASE-BOUNDARIES` (context boundaries) |
| `grill` | matt `grilling` (design tree, frontier rounds, recommended answers) | addy `interview-me` (hypothesis + confidence, "should want", explicit yes, predict-3 stop test); pstack `sharpen` (four questions) |
| `grill-with-docs` | matt `grill-with-docs` (composition of grilling + domain-modeling) | — |
| `domain-modeling` | matt `domain-modeling` (challenge terms, scenarios, cross-check code, inline glossary, three-part ADR test, GLOSSARY/ADR formats) | addy `documentation-and-adrs` (what qualifies for an ADR); pstack `principle-model-the-domain` (use the domain's words) |
| `prototype` | matt `prototype` (logic HTML demo, UI `?variant=` switcher, throwaway rules, capture on a branch) | pstack `poteto-mode/playbooks/prototype` (no decision no prototype, spike branch, observe on the matching surface, recommendation report); addy `frontend-ui-engineering` ("prototypes become production" → never promote, rebuild via tdd); pstack `benchmark-checklist` (spike measurement) |
| `spec` | addy `spec-driven-development` (measurable targets, Always/Ask/Never, STOP gate) | matt `to-spec` (test seams, no file paths); pstack `figure-it-out` (falsifiable done, riskiest unknown first) |
| `architect` | pstack `architect` (ground, design twice, red-flag screen, usage first, interface depth, scrap signals, rationale template) | matt `codebase-design` (design-it-twice constraints as parallel sub-agents) |
| `slice` | matt `to-tickets` (tracer-bullet slices, blocking edges, prefactor, expand-contract) | addy `planning-and-task-breakdown` ("and" split rule, checkpoints, never overwrite a plan) |
| `tdd` | matt `tdd` (agreed seams, tautological/coupled anti-patterns) | addy `test-driven-development` (discover stack, prove-it, first-run-pass suspicion), `constraint-driven-development` (guard the bar), `incremental-implementation` (~100 lines rule) |
| `debug` | matt `diagnosing-bugs` (red-capable loop gate, minimize, 3-5 hypotheses, debug tags) | pstack `principle-attack-the-premise` (two failed fixes), `principle-fix-root-causes` (guard test, always-on signal) |
| `principles` | pstack `principle-*` (one rule per file, cite the decision it changed, build-the-lever) | matt `codebase-design` (deletion test, one vs two adapters, design it twice); addy `code-simplification` / `code-review-and-quality` (Chesterton's fence, count the concepts) |
| `review` | pstack `interrogate` (intent lock, rubric lenses, lead judgment, act/consider/noted/dismissed) | matt `code-review` (two isolated axes, three-dot diff, fail early); addy `code-review-and-quality` (mutation check, approve on code health) |
| `doubt` | addy `doubt-driven-development` (withhold the claim, ordered classification, 3-cycle cap, doubt theater) | pstack `figure-it-out` / `swarm` (VERIFIED / NOT VERIFIED / INCONCLUSIVE, a gap is not a pass), `blast-radius` (proof ladder) |
| `blast-radius` | pstack `blast-radius` (one safety fact, proof ladder, report shape) | pstack `why` (null results count, never invent callers) |

## Install

```
/plugin marketplace add /path/to/agent-skills
/plugin install agent-skills@agent-skills
```

Skills appear as `/agent-skills:<name>`.

## Conventions

- `SKILL.md` stays under ~150 lines; detail goes in `references/`, loaded only when needed.
- Descriptions say what the skill does and when to use it, never the workflow steps.
- User-invoked orchestrators set `disable-model-invocation: true`; disciplines stay model-invoked.
- Skill text is English; skills talk to the user in the user's language.

## Translations

`README.md` (English) is the source of truth. Translations live next to it as `README.<lang>.md`, using an ISO 639-1 code (`README.ko.md`, `README.ja.md`, ...).

To add a language:
1. Copy `README.md` to `README.<lang>.md` and translate it. Keep skill names, file paths, commands, and code blocks as they are.
2. Add the language to the switcher line at the top of **every** README, in the same order.
3. When `README.md` changes, update each translation in the same commit, or note at its top that it is behind.
