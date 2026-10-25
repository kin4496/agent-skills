# Subtract before you add

Every addition is maintained forever. The cheapest code is the code you deleted.

## Before adding, ask
1. Can I delete something so this need disappears?
2. Can an existing mechanism do this with a small change?
3. Does this need to vary in a second way **today**? If not, inline it; an abstraction with one implementation is a guess.

## Deletion test
Imagine deleting the module. If complexity vanishes, it was a pass-through; remove it. If complexity reappears across many callers, it was earning its keep.

## Seams
One adapter means a hypothetical seam. Two adapters means a real one. Don't build the interface until the second implementation exists or is scheduled.

## Count concepts
A refactor that moves complexity without removing concepts is a relocation, not a simplification. Count the concepts a reader must hold before and after.

## Chesterton's fence
Before removing something that looks pointless, find out why it's there (`git log -L`, blame, the PR). Remove it once you know.
