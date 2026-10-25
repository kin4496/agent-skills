# GLOSSARY.md format

```md
# <Context name>

<One or two sentences: what this context is and why it exists.>

## Language

**Order**:
A customer's request to buy one or more products, from placement until fulfilled or cancelled.
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request
```

## Rules

- **Be opinionated.** When several words exist for one concept, pick one and list the rest under `_Avoid_`.
- **Tight definitions.** One or two sentences: what it *is*, not what it does or how it's stored.
- **Domain terms only.** Ask: is this unique to this project's domain, or a general programming concept? Only the former belongs.
- **Group** under subheadings when natural clusters appear; a flat list is fine otherwise.

## Multi-context map

`GLOSSARY-MAP.md` at the root:

```md
# Glossary map

## Contexts
- [Ordering](./src/ordering/GLOSSARY.md): receives and tracks customer orders
- [Billing](./src/billing/GLOSSARY.md): invoices and payments

## Relationships
- **Ordering -> Billing**: Ordering emits `OrderPlaced`; Billing consumes it to create an Invoice
```
