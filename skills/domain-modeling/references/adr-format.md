# ADR format

Location: `docs/adr/NNNN-<slug>.md`. Find the highest existing number and add one. Create the directory only when the first ADR is written.

```md
# <Short title of the decision>

<1-3 sentences: the context, what we decided, and why.>
```

That is the whole template. An ADR's value is recording *that* a decision was made and *why*.

## Optional sections (only when they add something)

- `Status:` proposed | accepted | deprecated | superseded by ADR-NNNN (when decisions get revisited)
- **Considered options**: only when the rejected alternatives are worth remembering
- **Consequences**: only for non-obvious downstream effects

## What qualifies (if it also passes the three-part test)

- Architectural shape: "Writes are event-sourced; reads are projected into Postgres."
- Integration between contexts: "Ordering and Billing talk via events, not synchronous HTTP."
- Technology with lock-in: database, message bus, auth provider, deploy target. Not every library.
- Boundaries and ownership: "Customer data is owned by the Customer context; others reference it by ID." Explicit no's count.
- Deliberate deviations: "Raw SQL instead of the ORM because X." Stops the next engineer "fixing" it.
- Constraints invisible in code: compliance, partner SLAs.
- Non-obvious rejections: "Considered GraphQL; chose REST because Y."
