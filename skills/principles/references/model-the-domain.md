# Model the domain

Make illegal states unrepresentable, so the next feature adds a case instead of a branch.

## Smells
- A new feature grows an existing if/else or switch by one more branch.
- Two booleans that must stay in sync (`isLoading` and `hasError`).
- State encoded as strings or magic numbers checked in many places.
- "Execution order is not ownership": code that works only because A happens to run before B.

## Reach for
- **Discriminated union / sum type** for mutually exclusive states.
- **State machine** when transitions have rules.
- **Reducer** when many events update one state.
- **Registry / table** when behavior varies by kind and kinds will grow.
- **Value objects** for validated primitives (Email, Money) so validation happens once at the boundary.

## Use the domain's words
Name types and functions with the terms the users and the spec use. If a glossary exists, follow it; if a new term appears, apply the `domain-modeling` skill to settle and record it. Two names for one concept, or one name for two, is a bug in waiting.

## Stop
Before writing logic, write the data shape. If you can't name the states, you don't understand the problem yet.
