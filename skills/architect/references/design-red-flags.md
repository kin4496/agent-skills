# Design red flags

Screen every candidate. A red flag is a reason to revise or reject the shape, or to list it explicitly as an accepted trade-off.

The lens: the next contributor is an agent that sees only the files it opened, copies the nearest example, and takes the shortest path that compiles.

## Shallow module
A large interface hiding little. Callers coordinate several calls for one operation; options expose internal stages; learning the interface doesn't spare you learning the implementation. A deep module concentrates capability behind one interface. Don't confuse it with a deep call chain, which scatters understanding.

## Information leakage
Several modules depend on the same internal decision (a representation, policy, or protocol detail), so changing it needs coordinated edits. Re-exporting wire or storage types is leakage. Parse external data into domain types behind the interface.

## Temporal decomposition
Modules split by execution order (load, validate, transform, save) instead of by the knowledge they own, repeating one representation and its invariants across boundaries. Group by ownership; code that runs at different times can share a module if it protects the same decisions.

## Pass-through method
Forwards the same arguments to another method of the same shape, adding a layer and hiding nothing. Remove it, unless it adds policy, adaptation, or a distinct abstraction.

## Split ownership
More than one module writes the same state or keeps its own copy. An agent editing one writer can't see the others, and their rules drift. One owner; others read or ask.

## Two ways to do one task
An agent copies whichever way it finds first, so both keep gaining callers. Keep one, migrate callers, delete the other in the same change.

## Importable internals
If outside code can import internals, the shortest path that compiles will, and internals become interface. Make them unreachable so an outside import fails the build.

## Hand-synced list
The same items listed in two or more places, so adding one means editing every list. Keep one list and derive the rest; if you can't derive it, make the build fail when they disagree.
