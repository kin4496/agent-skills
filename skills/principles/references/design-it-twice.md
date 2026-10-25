# Design it twice

Your first design is the one you thought of first, not the best one.

## Pattern
Before committing to an interface, schema, or module boundary, sketch at least two **structurally different** designs. Signatures and types only, bodies `not implemented`. Useful constraints to force difference:
- minimal: the smallest interface that could work
- caller-first: the ideal call site for the most common caller
- flexible: what survives the most likely next requirement
- ports and adapters: core logic with no I/O

For a big decision, give each constraint to a separate sub-agent so the designs don't converge.

## Compare on
- Depth: how much behavior hides behind how small an interface.
- What the most common caller writes.
- What the next contributor copies. Assume they copy the nearest example and take the shortest path that compiles. Does that path stay correct?
- What changes if the likely next requirement arrives.

## Stop
Pick one and write one sentence on why it beat the other. If they come out equal, pick the simpler.
