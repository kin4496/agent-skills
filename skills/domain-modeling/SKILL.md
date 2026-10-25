---
name: domain-modeling
description: Actively build and sharpen a project's domain model by challenging terms and keeping GLOSSARY.md and ADRs current. Use when discussing or changing domain terminology, when the user's words conflict with the code or glossary, or when writing or editing GLOSSARY.md or an ADR.
---

# Domain modeling

This is the active discipline: challenging terms, probing boundaries with scenarios, and writing the glossary and decisions down the moment they settle. Merely reading `GLOSSARY.md` for vocabulary is a habit any skill has; this skill is for when the model changes.

## Files

- **Single context (most repos):** `GLOSSARY.md` at the root, ADRs in `docs/adr/`.
- **Multiple contexts:** `GLOSSARY-MAP.md` at the root lists each context's `GLOSSARY.md` and how contexts relate. System-wide ADRs in root `docs/adr/`, context-specific ones in `<context>/docs/adr/`. Infer the context from the topic; ask if unclear.

Create files lazily, only when there is something to write. Formats: `references/glossary-format.md`, `references/adr-format.md` (relative to this skill's directory).

## During the conversation

- **Challenge against the glossary.** When the user's term conflicts with it, say so immediately: "The glossary defines *cancellation* as X, but you seem to mean Y. Which is it?"
- **Sharpen fuzzy words.** Propose one canonical term for an overloaded one: "By *account*, do you mean the Customer or the User? They're different."
- **Probe with scenarios.** Invent concrete edge cases that force the boundary between two concepts to be precise (partial refunds, an order split across warehouses, a user in two orgs).
- **Cross-check the code.** When the user states how something works, verify it (a sub-agent for anything beyond a quick grep). Surface contradictions: "Your code cancels whole Orders, but you said partial cancellation is possible. Which is right?"
- **Update inline.** When a term is resolved, write it to the glossary right then. Don't batch.

## Glossary is only a glossary

No implementation details, no specs, no decisions, no scratch notes. Only terms specific to this domain; general programming concepts (retry, cache, timeout) don't belong.

## ADRs, sparingly

Offer an ADR only when all three hold:

1. **Hard to reverse**: changing your mind later is costly.
2. **Surprising without context**: a future reader would ask "why did they do it this way?"
3. **A real trade-off**: there were genuine alternatives and one was picked for reasons.

Also record a rejected suggestion when the rejection is non-obvious, so nobody re-proposes it in six months. Ask before writing; the user owns decisions.

## Red flags

- Two words for one concept, or one word for two, left unresolved.
- Glossary entries that describe implementation ("stored in the `orders` table").
- An ADR for an easily reversed or obvious choice.
- Terms in code, spec, and glossary that disagree after the session.
