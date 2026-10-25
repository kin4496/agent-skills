---
name: grill-with-docs
description: Grill the user on a plan or design while recording the domain language in GLOSSARY.md and hard-to-reverse decisions as ADRs. Use when the user says "grill with docs", or wants the interview to leave durable docs behind.
disable-model-invocation: true
---

# Grill with docs

Run two skills together, from this skill's directory:

1. Read `../grill/SKILL.md` and follow it as the main loop (it also loads the operating rules).
2. Invoke the `domain-modeling` skill (or read `../domain-modeling/SKILL.md`) and apply it throughout every round.

## How they combine

- **Before round one**, read the existing glossary (`GLOSSARY-MAP.md` or `GLOSSARY.md`) and the ADRs in `docs/adr/` in the area being discussed. Use their terms in your hypothesis and questions.
- **Inside each round**, terminology conflicts and fuzzy terms become frontier questions, with your recommended canonical term as the answer. Code-vs-claim contradictions found by sub-agents become questions too.
- **When a term is settled**, write it to the glossary immediately, before the next round. Mention the update in one line ("Glossary: added **Refund**, avoid *Return*").
- **When a decision passes the ADR test**, offer the ADR in the same round; write it only after the user agrees.
- **The final restatement** uses only glossary terms and lists what was recorded:

```
Docs updated:
- GLOSSARY.md: <terms added or changed>
- docs/adr/NNNN-<slug>.md: <decision>
```

Then STOP, as `grill` says, and suggest `/agent-skills:spec`. The spec should use the glossary's terms.
