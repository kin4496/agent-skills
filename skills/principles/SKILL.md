---
name: principles
description: Small engineering principles for design decisions - modeling the domain, fixing root causes, subtracting before adding, designing twice, and building levers. Use when choosing a data shape or abstraction, deciding between a fix and a guard, adding a branch or flag, or doing the same edit many times.
---

# Principles

Each principle is one short file in `references/` (relative to this skill's directory). Read the one that applies; don't load them all.

| When you are about to... | Read |
|---|---|
| add another `if` branch, a second boolean that must stay in sync, or a string-typed state | `references/model-the-domain.md` |
| add a null check, try/catch, retry, or fallback to make an error go away | `references/fix-root-causes.md` |
| add a new module, option, layer, or abstraction | `references/subtract-before-you-add.md` |
| commit to an interface, schema, or module boundary | `references/design-it-twice.md` |
| make the same edit in more than ~5 places | `references/build-the-lever.md` |

## Citing a principle

When a principle shaped a decision, name it and the specific choice it changed: "model-the-domain: replaced `isLoading` + `hasError` with a `status` union". Cite only principles whose file you read this session. A citation that changed nothing is noise; leave it out.

A principle that says "build X" is applied only if X is in the diff. Citing build-the-lever with no script or codemod in the change means it was not applied.
